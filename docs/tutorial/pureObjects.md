# Creating Objects without Django model

It is also possible to create an object without Django model. In fact, Simple API only
extracts information from Django models, and then completely forgets that Django exists!
These two layers allow for simple modification of the objects - for example, required fields
in Django do not need to be required in Simple API: once the information is extracted from
Django, it can be easily rewritten without looking back to Django, not causing any conflicts.

An example can be seen here:

```python
from adapters.graphql.graphql import GraphQLAdapter
from adapters.utils import generate
from object.actions import Action
from object.datatypes import StringType, IntegerType, ObjectType
from object.object import Object
from simple_api.adapters.graphql.utils import build_patterns
from utils import AttrDict


def get_by_id(request, params, **kwargs):
    return AttrDict(id=params["id"], car=AttrDict(model="BMW", color="blue"))


class Car(Object):
    fields = {
        "model": StringType(),
        "color": StringType()
    }


class Owner(Object):
    fields = {
        "id": IntegerType(),
        "car": ObjectType(Car)
    }

    actions = {
        "getById": Action(parameters={"id": IntegerType()}, return_value=ObjectType("self"), exec_fn=get_by_id)
    }

schema = generate(GraphQLAdapter)
patterns = build_patterns("api/", schema)
```

The `getById` action returns a blue BMW with the `id` specified in its parameter.