# Simple API Documentation

[![Run tests](https://github.com/karlosss/simple_api/actions/workflows/run_tests.yml/badge.svg)](https://github.com/karlosss/simple_api/actions/workflows/run_tests.yml)

Simple API is an extension to Django and Graphene-Django, aiming to provide a fast-to-work API for already 
defined Django models with as little code as possible. It provides the ability to include a model
in an API with basic operations on it with less than 10 lines of code. 
It aims to support multiple different API architectures, but currently just GraphQL is supported.

After writing Django models, Simple API builds an API from the information contained purely in models.

Built on some amazing ideas of [graphene-django-extras](https://github.com/eamigo86/graphene-django-extras). 
The main difference is that Simple API is a bit more high-level and allows the programmer to write even less code.
