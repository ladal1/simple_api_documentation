# Custom filtering

As shown in the beginning, a `ListAction` has a builtin way to filter the results: it does so by filters automatically generated from fields of the target model.

It is, of course, possible to alter the set of filters generated for a field by overriding the field and specify the filters as follows:

```python
from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from django_object.django_object import DjangoObject
from object.datatypes import StringType, ObjectType
from .models import CustomUser as CustomUserModel, Post as PostModel
from simple_api.adapters.graphql.utils import build_patterns


class CustomUser(DjangoObject):
    model = CustomUserModel
    custom_fields = {
        "email": StringType(only_filters=("email__exact", "email__icontains")),
        "first_name": StringType(exclude_filters=("first_name__regex", "first_name__iregex")),
    }


class Post(DjangoObject):
    model = PostModel
    output_custom_fields = {
        "author": ObjectType(CustomUser, exclude_filters=(),
                             custom_filters={"author__email": StringType(nullable=True)})
    }


schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)

```

The `only_filters`, `exclude_filters` and `custom_filters` work the same way as filtering fields, with the exception that without modifiers, no filters are created. This is for obvious reasons: if we create a custom field, we probably don't want any filters for it, as it is probably not a field in the database and Django would just crash on those non-existent filters.

Note that when overriding a foreign key field, both the original filters (if intended to keep) and the custom ones need to be specified.