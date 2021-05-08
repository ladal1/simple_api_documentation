# Multiple Objects for one model

Sometimes, we want multiple `Object`s for the same model - for example, the email of a user should be visible only by the user himself and maybe by admins. A way to do this is creating two objects: one for admins, and one for the rest.
```python
from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from django_object.django_object import DjangoObject
from .models import CustomUser as CustomUserModel, Post as PostModel
from simple_api.adapters.graphql.utils import build_patterns


class CustomUser(DjangoObject):
    model = CustomUserModel
    class_for_related = False


class CustomUserPublic(DjangoObject):
    model = CustomUserModel
    exclude_fields = ("email", "password")


class Post(DjangoObject):
    model = PostModel


schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)
```

Everything should be self-explanatory except for `class_for_related`. When building relations automatically, Simple API needs to determine a type to return. As long as we have only one `Object` per model, it is obvious as there is no choice. But now Simple API does not know: should it make the field `author` of the `Post` model of type `CustomUser`, or `CustomUserPublic`? And this is exactly what `class_for_related` means: it uses the one which has it set to `True`. That can be seen in the schema - the type of `author` is indeed `CustomUserPublic`, because it is set to be the class to resolve relations:
```
// Post
id: Int!
title: String!
content: String!
author: CustomUserPublic!
__str__: String!
__actions: [ActionInfo!]!
```

If the one-fits-them-all method is not fine-grained enough, the field for relations can be overridden the same way as any other field in `custom_fields`.