### Meta schemas

**Note: this part will likely significantly change in the future.**

GraphiQL already contains some information about types, parameters, etc. Meta schemas
of Simple API extend this schema by the information about actions and permissions. These
are supposed to be used in order to maintain loose coupling between the backend and any
possible frontend clients.

On the top level, there are three meta endpoints `__types`, `__objects` and `__actions`. They can be
queried as follows:

```graphql
{
      __types{
        typename
        fields{
          name
          typename
        }
      }
      __objects{
        name
        pk_field
        actions{
          name
          parameters{
            name
            typename
            default
          }
          data {
            name
            typename
            default
          }
          mutation
          return_type
          permitted
          deny_reason
          retry_in
        }
      }
    __actions{
      name
      parameters{
        name
        typename
        default
      }
      data {
        name
        typename
        default
      }
      mutation
      return_type
      permitted
      deny_reason
      retry_in
    }
}
```

`__objects` contains information about objects. Each object created in Simple API by the user
is automatically registered. Besides the `name`, Simple API also tells the frontend which
is the primary key field, which is necessary for all actions operating on a specific instance
of an object. Follows a list of actions not bound to any specific instance (such as `create`
or `list`). For each of those actions, Simple API also tells if, based on the user and current
system state, the action is allowed or not, why it is disallowed, and in what time 
this information should be refreshed. This way, frontend can display (or not display)
buttons, disabled buttons, error messages etc.
The `__types` has information about the types of objects provided by the API.

The `__actions` endpoint covers the actions not connected to a particular object.

Each instance of an object also has `__actions` field:

```graphql
query list_users{
  CustomUserList{
    data{
      id
      username
      password
      __actions{
          name
          parameters{
            name
            typename
            default
          }
          data {
            name
            typename
            default
          }
          mutation
          return_type
          permitted
          deny_reason
          retry_in
        }
    }
  }
}
```

This field contains information about actions for that particular instance, for example `update`
or `delete`.
