# Tests and their description
Reference for developing Simple API

| Test name and location                                          | Tested functionality                                                                      |
|-----------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| objects/action_retry_in                                         | Permissions and retry_in field of permission part of Metaschema                           |
| objects/and_or_not_permissions                                  | Permissions and logical connectors for permissions                                        |
| objects/car_owner                                               | Manually written connected objects                                                        |
| objects/date_time                                               | Handling of datetime type, support for which is added                                     |
| objects/duration_type                                           | Handling of duration type, support for which is added                                     |
| objects/hide_action_if_denied                                   | Hiding action within Metaschema when the client is lacking permission                     |
| objects/infinite_recursion                                      | Handling of object with infinite recursion (referencing itself)                           |
| objects/int_list_records_pagination                             | Manually defined pagination lists                                                         |
| objects/int_list_records_pagination_recursive                   | Recursion in manually defined pagination lists                                            |
| objects/multiple_modules                                        | Separation of object definition to multiple modules (imported to objects.py)              |
| objects/nested_object_as_input                                  | Action with object (with possible nesting) as an argument                                 |
| objects/object_list_as_input                                    | Action with a list as an argument                                                         |
| objects/object_list_of_objects_field                            | Action with list of objects (with possible nesting) as an argument                        |
| objects/object_list_records_pagination                          | Action with a list of objects as arguments and pagination over lists                      |
| objects/objects_primitive_fields_class_ref_by_string            | Ability to reference a class as an object type (in action definition) by string           |
| objects/objects_simple_list_field                               | PlainListType as a return value in action                                                 |
| objects/object_type_as_input                                    | Usage of Simple API object as argument definition                                         |
| objects/object_type_with_list_as_input                          | Action with defined object of PlainListType as an argument                                |
| objects/object_with_field_parameters                            | Arguments on fields of the object                                                         |
| objects/object_with_input_output_fields                         | Testing of Object attributes input/output fields                                          |
| objects/object_with_params_and_resolver                         | Field with a resolver function using parameters                                           |
| django_objects/autocomplete_user_model                          | Generation of object based on existing model                                              |
| django_objects/primitive_model_fields_only_exclude              | exclude_fields and only_fields attributes for DjangoObject                                |
| stack_overflow/graphene_python_list_resolve_null_for_all_fields | Correct resolution of Object list data with Graphene                                      |
| readme_forum/readme_forum_basic                                 | General functionality based on example usage                                              |
| readme_forum/readme_forum_class_for_related                     | Multiple objects for one django Model - class_for_related attribute                       |
| readme_forum/readme_forum_custom_actions                        | Addition of custom actions to a generated DjangoObject                                    |
| readme_forum/readme_forum_custom_fields                         | Addition of custom fields to a generated DjangoObject                                     |
| readme_forum/readme_forum_custom_filters                        | Addition of custom filters on a list to a generated DjangoObject                          |
| readme_forum/readme_forum_permissions                           | Django permissions, logical connectors on permissions                                     |
| library_system/library_system_basic                             | General functionality based on example usage                                              |
| library_system/library_system_permissions                       | Django permissions, logical connectors on permissions                                     |
| library_system/library_system_validation                        | Simple API validation, consumption of Django validators, logical connectors on validators |
| library_system/library_system_query_security                    | Depth limit, action/field difficulty assignment, query cost limit                         |