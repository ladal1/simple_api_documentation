# Simple API admin

With Simple API a custom testing interface is developed, to allow for easy understanding of the endpoint.
It can be either downloaded from [GitHub](https://github.com/ladal1/simple_api_admin) or installed from test PyPI - 
```shell
  $ pip install -i https://test.pypi.org/simple/ simple-api-frontend
```

**The interface contains insecure by desing login form - it will only work when the DEBUG setting is set to True**

The interface allows:
- testing of authentification
- permissions 
- automatic query Generation
- showing list of objects and actions over them
- filtering and sorting the instances

The interface is independent from GraphQL schema - it uses Simple API specific [Metaschema](tutorial/metaschema.md)
![Example view](assets/SimpleAPIAdmin.png)

## Installation
###Github
`pip install -e ./build` in the directory with setup.py file
###PyPI
`pip install -i https://test.pypi.org/simple/ simple-api-admin`

After either of steps add the simple_api_admin to INSTALLED_APPS in Django
```python
  # ....
  INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'graphene_django',
    'simple_api',
    'simple_api_admin',
    #...
  ]
  #...
```
```'django.contrib.staticfiles'```  is a required dependency for Simple API Admin.


And then add simple_api_admin into your urls.py (patterns is simple_api.adapters.graphql.utils.build_patterns to create Simple API):

```python
from django.urls import path
from simple_api_admin import views


urlpatterns += [path('simple/', views.site.urls)]
```

**Warning: Currently frontend is hardcoded to expect API at ../api/**