# Custom actions

So far, we only were using the automatically generated actions - `list`, `detail`, `create`, `update` and `delete`. Now we show how to add more: say a user should be able to change their password and list their own posts. But let's start from the beginning.

First, we can modify the way the basic actions are generated:

```python
from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from django_object.actions import CreateAction
from django_object.django_object import DjangoObject
from .models import CustomUser as CustomUserModel, Post as PostModel
from simple_api.adapters.graphql.utils import build_patterns


def custom_create_user(request, params, **kwargs):
    return CustomUserModel.objects.create(
        email="default@example.com",
        username=params["data"]["username"],
        first_name="Name",
        last_name="Surname",
        password="default"
    )


class CustomUser(DjangoObject):
    model = CustomUserModel

    create_action = CreateAction(only_fields=("username",), exec_fn=custom_create_user)
    update_action = None
    delete_action = None


class Post(DjangoObject):
    model = PostModel


schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)
```

```graphql
mutation create_user_username_only{
  CustomUserCreate(data: {username: "cindy"}){
    id
  }
}

// output
{
  "data": {
    "CustomUserCreate": {
      "id": 1,
      "email": "default@example.com",
      "username": "cindy",
      "first_name": "Name",
      "last_name": "Surname",
      "password": "default",
      "bio": "",
      "is_admin": false
    }
  }
}
```

Our customized `CreateAction` has only one data parameter, the username, and the rest is filled with some default values, as specified in `custom_create_user`. If an action should not be generated at all, just set it to `None`.

Let's say we want the users to be able to change their own password. For this, we need to add a custom actions as follows:

```python
from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from django_object.actions import UpdateAction
from django_object.django_object import DjangoObject
from .models import CustomUser as CustomUserModel, Post as PostModel
from simple_api.adapters.graphql.utils import build_patterns

class CustomUser(DjangoObject):
    model = CustomUserModel

    custom_actions = {
        "changePassword": UpdateAction(only_fields=("password",), 
                                       required_fields=("password",))
    }


class Post(DjangoObject):
    model = PostModel


schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)
```

By default, the fields of an `UpdateAction` are not mandatory - it would be very annoying to
specify the whole large object when modifying just a single value! This, however, is not
always what we want: when the action is supposed to change only the password, the new password
should be mandatory to provide. This is exactly what `required_fields` stands for: specify
the fields which should be required, even in an `UpdateAction`.

Sometimes, an action is not a CRUD operation. For these cases, the custom `exec_fn` must be
written by the user. Refer to `ModelAction` or `ModelObjectAction`, depending if the action
should operate on the object class (like `create` for example), or on specific instances
of the object (like `update` or `delete` for example) respectively.