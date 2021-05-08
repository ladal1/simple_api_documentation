# Permissions

The forum now has one obvious problem mentioned above - everyone can do everything. Now it is time to change it. 
Each action in Simple API might include a set of permissions required to pass - if it does not, an error occurs
and the action is not performed.

The permissions should only determine if a user is allowed to execute an action or not based on the user and the
current application state. The input data for the action should not be considered. 
**For rejection on input or saved data use [Validators](validators.md)**

Permissions are shipped as classes. The key part is overriding the `permission_statement` method:

```python
from django_object.permissions import DjangoPermission

class IsAuthenticated(DjangoPermission):
    def permission_statement(self, request, **kwargs):
        return request and request.user and request.user.is_authenticated
```

Let's say we now want to create an `IsAdmin` permission class, which would allow through only those users
which have either `is_admin` or `is_superuser` set. This is what we would do:

```python
from django_object.permissions import IsAuthenticated

class IsAdmin(IsAuthenticated):
    def permission_statement(self, request, **kwargs):
        return request.user.is_staff or request.user.is_superuser
```

You might be wondering why don't we call the `permission_statement` method of the superclass. One would do
this all the time, therefore Simple API just calls the method of the superclass automatically, based on
the inheritance chain. This saves some boilerplate code and makes the result a bit more readable.

Let's see how to use the permissions in practice. Our forum API enhanced with the permissions could look
as follows:

```python
from django.contrib.auth.models import User as UserModel

from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from django_object.actions import CreateAction, UpdateAction, DeleteAction, DetailAction, ListAction, ModelAction
from django_object.django_object import DjangoObject
from django_object.permissions import IsAuthenticated
from object.datatypes import StringType
from object.permissions import Or
from .models import Post as PostModel
from simple_api.adapters.graphql.utils import build_patterns


class IsAdmin(IsAuthenticated):
    def permission_statement(self, request, obj, **kwargs):
        return request.user.is_staff or request.user.is_superuser


class IsSelf(IsAuthenticated):
    def permission_statement(self, request, obj, **kwargs):
        return request.user == obj


class IsTheirs(IsAuthenticated):
    def permission_statement(self, request, obj, **kwargs):
        return request.user == obj.author


def set_password(request, params, **kwargs):
    # this would set a password to request.user, but for the showcase it is not needed
    pass


def create_post(request, params, **kwargs):
    data = params["data"]
    return PostModel.objects.create(title=data["title"], content=data["content"], author=request.user)


class User(DjangoObject):
    model = UserModel
    only_fields = ("username", )

    create_action = CreateAction(permissions=IsAdmin)
    update_action = UpdateAction(permissions=IsAdmin)
    delete_action = DeleteAction(permissions=IsAdmin)
    detail_action = DetailAction(permissions=IsAdmin)
    list_action = ListAction(permissions=IsAdmin)

    custom_actions = {
        "changePassword": ModelAction(data={"password": StringType()}, exec_fn=set_password,
                                      permissions=IsAuthenticated),
        "myProfile": ModelAction(exec_fn=lambda request, **kwargs: request.user, permissions=IsAuthenticated)
    }


class Post(DjangoObject):
    model = PostModel

    create_action = CreateAction(exclude_fields=("author_id",), permissions=IsAuthenticated, exec_fn=create_post)
    update_action = UpdateAction(permissions=Or(IsAdmin, IsTheirs))
    delete_action = DeleteAction(permissions=Or(IsAdmin, IsTheirs))
    list_action = ListAction(permissions=IsAuthenticated)
    detail_action = DetailAction(permissions=IsAuthenticated)


schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)
```

This slightly more comprehensive example shows how one could do the forum. Let's have two
types of users: admins and the others. Everyone can view their own profile, change their own
password, create and list all posts and edit and delete their own posts. Admins on top of that
can manipulate the database of users and also their posts.

As we can see, for a logical expression consisting of multiple permissions, we can use
`And`, `Or` and `Not` connectors, which form a universal set of connectors, and therefore
can express any logical expression. For sure, we could write a permission class with permission 
statement covering exactly those options, but this way the code is more readable and therefore
less error-prone.

Besides the logical expressions, we also show how to use `ModelAction` and a custom create 
(the autor of the post is the one who sent the create request).
