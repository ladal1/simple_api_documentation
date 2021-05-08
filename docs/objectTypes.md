# Object types reference

### Object
(simple_api.object.object)

| Attribute name          | Type                              | Description                                                                                              |
|-------------------------|-----------------------------------|--------------------------------------------------------------------------------------------|
| fields                  | Dictionary (fieldName: FieldType) | Fields usable for both <br />identifying and selection in an object                                            |
| input_fields            | Dictionary (fieldName: FieldType) | Fields to be used when identifying object                                                                |
| output_fields           | Dictionary (fieldName: FieldType) | Fields to return for selection -<br />can be either primitive type or ObjectType                              |
| actions                 | Dictionary (actionName: Action)   | Actions over the object                                                                                  |
| field_difficulty_scores | Dictionary (fieldName: value)     | Values of individual field to be used <br />when using WEIGHT_LIMIT and generate_w,<br />default for each field is set in default_field_difficulty|
| default_field_difficulty | Integer     | Default value to use if not speciefied in field_difficulty_scores, default is 1|


### DjangoObject
(simple_api.django_object.django_object)

| Attribute name       | Type         | Description                                                                                                                  |
|----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------|
| model                | Django Model | Django model to be converted                                                                                                 |
| class_for_related    | Boolean      | Should this class be used for returns of this object?<br />(default: True),<br />Use when declaring multiple Objects per Django model |
| only_fields          | Tuple        | Only use these fields when generating object<br />(Cannot be used with exclude_fields)                                            |
| exclude_fields       | Tuple        | Use all but these fields<br />(Cannot be used with only_fields)                                                                   |
| custom_fields        | Dictionary   | Additional fields to be added,<br />equivalent with fields in Object                                                              |
| input_custom_fields  | Dictionary   | Additional fields to be added,<br />equivalent with input_fields in Object                                                        |
| output_custom_fields | Dictionary   | Additional fields to be added,<br />equivalent with output_fields in Object                                                       |
| detail_action        | Action       | Action to add as a Read<br />default DetailAction(),<br />set None if you don't want detail action                                  |
| list_action          | Action       | Action to add as a multiple access -<br />default ListAction(),<br />set None if you don't want detail action                         |
| create_action        | Action       | Action to add as a Create<br />default CreateAction(),<br />set None if you don't want detail action                                |
| update_action        | Action       | Action to add as a Update<br />default UpdateAction(),<br />set None if you don't want detail action                                |
| delete_action        | Action       | Action to add as a Delete<br />default DeleteAction(),<br />set None if you don't want detail action                                |
| custom_actions       | Dictionary   | Additional actions to be added to the object<br />see actions in Object                                                         |