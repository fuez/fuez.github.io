---
title: django
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## [How to migrate data from sqllite to other db engine?][1]
```
python manage.py dumpdata --indent 2 --exclude auth.permission --exclude contenttypes > db.json
# change custom.py to use another data source
python manage.py loaddata db.json
```
## [django informix db engine](https://github.com/nutztherookie/django-informix)

## References:
## [Getting Started with Django Rest Framework and AngularJS](http://blog.kevinastone.com/getting-started-with-django-rest-framework-and-angularjs.html)
## [django settings reference](https://docs.djangoproject.com/en/1.9/ref/settings/)
## [TaskBuster Django Tutorial](http://www.marinamele.com/taskbuster-django-tutorial)
## [Install and Configure PostgreSQL](http://www.marinamele.com/taskbuster-django-tutorial/install-and-configure-posgresql-for-django)


[1]: https://coderwall.com/p/mvsoyg/django-dumpdata-and-loaddata