Working with the models from the [Quick Start](../QuickStart.md)
You for sure noticed that the `CustomUser` model has a property that is not accessible in the API. This is expected - Simple API only automatically converts fields, relations, and reverse relations (you maybe noticed that a `CustomUser` in the first example has `post_set` field in the API). At the end of this section, we show how to add the property.

But let's start from the beginning. Assume the same model, but the following `Object`s:

```python
from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from django_object.django_object import DjangoObject
from object.datatypes import StringType
from .models import CustomUser as CustomUserModel, Post as PostModel
from simple_api.adapters.graphql.utils import build_patterns


class ShortCustomUser(DjangoObject):
    model = CustomUserModel
    only_fields = ("first_name", "last_name")
    output_custom_fields = {"full_name": StringType()}


class ShortPost(DjangoObject):
    model = PostModel
    exclude_fields = ("content",)


schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)
```

By default, Simple API extracts all fields, models, and reverse relations. If this is not what we want, we have two ways to specify: either list the fields we want using `only_fields`, or list the fields we don't want using `exclude_fields` (but not both at the same time, of course).

After the set of extracted fields is determined, we can add `custom_fields`: each custom field must also have its type specified, as there is no way Simple API can find that out.

Note that model properties are only output fields. Simple API objects can be also used as inputs and in such case, we do not want read-only fields there. Custom fields that should be both for input and output are specified in `custom_fields`, fields only for input in `input_custom_fields`, and those only for output in `output_custom_fields`.

The schema then looks like this:
```
// Short post
id: Int!
title: String!
author: ShortCustomUser!
__str__: String!
__actions: [ActionInfo!]!

// Short custom user
id: Int!
first_name: String!
last_name: String!
full_name: String!
__str__: String!
__actions: [ActionInfo!]!
```

As we can see, the primary key field (in this case `id`) is always included and cannot be got rid of. That comes with a reason - without primary key, the CRUD operations would just not work.

If a custom field happens to have the same name as an extracted field, the original field is overridden. This might be useful for example if a model field is nullable, but we don't want it to be nullable in the API.