# Security

The primary features for securing the API are [validation](tutorial/validators.md) and [authorization](tutorial/permissions.md). 
Further checks can also be implemented in the resolver function. Focus of this chapter is on preventing DoS attacks. If you do need more security tips, OWASP has a [short summary](https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.htm) of what you should consider.

Usage of GraphQL brings not just advantages, but some compromises as well. One of which is the propensity for denial of service attacks (DoS).
Simple API implements three limits for scanning the query before its execution, that can be applied to the endpoint.
They are 

- **Depth** - Limits the number of selections that can be nested inside each other.
- **List** - Default paginated list has an argument of limit, if this limit is set, it requires the query to contain the argument and have value less than the limit
- **Weight** - When no fields or actions have a set weight works as a selection limit, you can however customize the weights from default

Setting the límits in settings.py:

```python
SIMPLE_API = {
    "SECURITY": {
        "LIST_LIMIT": 100,
        "DEPTH_LIMIT": 20,
        "WEIGHT_LIMIT": 200000
    }
}
```

Setting the values for the weight limit:

For actions:
```python
actions = {
    "Heavy_Action": Action(return_value=StringType(), exec_fn=ping, action_weight=100001),
    "Light_Action": Action(return_value=StringType(), exec_fn=ping, action_weight=1)
}
```
For fields in object:
```python
class Book(Object):
    fields = {
        "id": IntegerType(),
        "car": ObjectType(Car)
    }
    
    field_difficulty_scores = {
        "id": 1,
        "car": 50
    }
```
