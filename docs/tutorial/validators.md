# Validators
There are occasions when permissions are not exactly the check you want to use - for such occasions Simple API has validators. 
This is an example validator:
```python
class NotNegative(Validator):
    def validation_statement(self, request, value=None, **kwargs):
        return value >= 0
```
Validators are meant to be used for validation of specific argument field (they can however be assigned to the full action as well).
Simple API then calls the function `validation_statement` expecting Boolean back, and denying the action call if it ends up False.
For example the Action getById would check that the argument id is not less than zero before the action is executed.
```python
class Book(DjangoObject):
    model = BookModel
    custom_actions = {
        "getById": Action(parameters={
                            "id": IntegerType(validators=NotNegative)
                          },
                          return_value=ObjectType("self"),
                          exec_fn=get_by_id)
}
```
`validation_statement` receives the value in the argument in `value`, detail about Meta information such as authorization in `request` and anything else in `kwargs`.
Just like Permissions, Validators support inheritance, and have their own logical connectors in `simple_api.object.validators`.
Django validators detailed in model definitions are also consumed by Simple API and validation is handled even before the Django model interface is called.