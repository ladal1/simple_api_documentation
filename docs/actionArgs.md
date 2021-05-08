# Action reference

| Argument name | Type                                     | Description                                                              |
|---------------|------------------------------------------|--------------------------------------------------------------------------|
| parameters    | Dictionary (parameterName:parameterType) | Arguments to be assigned to the action                                   |
| data          | Dictionary (parameterName:parameterType) | Arguments to be assigned to the action under the data top level argument |
| return_value  | ObjectType                               | Type that is returned by the action                                      |
| exec_fn       | Function                                 | Resolver function for the action                                         |
| permissions   | List/Tuple/Permission                    | Permissions to be resolved when the action is requested                  |
| validators    | List/Tuple/Validator                     | Validators to be resolved when the action is requested                   |
| action_weight | Number                                   | Action cost to be used when applying the query cost limit from [Query Security](security.md)      |