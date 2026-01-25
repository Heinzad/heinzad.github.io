---
layout: post
tags: Python
title: "Resume a Django project with Python"
author: "Adam Heinz"
date: 2023-01-12 01:00:00
---
How to reactivate the virtual environment and restart the server using the Powershell terminal when returning to a Django project.  


### Reactivate the virtual environment 

Instructions on [setting up a venv Python virtual environment for Django](https://docs.djangoproject.com/en/4.1/howto/windows/) can be found on the project website. 

In the PowerShell terminal, navigate to the project folder: 

```powershell

cd my-python-projects
cd my-django-project 

``` 

Reactivate the virtual environment: 

```powershell 

my_django_project_venvironment\Scripts\activate 

``` 

The virtual environment name will appear in brackets at the beginning of the command line: 

```powershell 

(my_django_project_venvironment) PS C: ... \my-python-projects\my-django-project>  

``` 


--- 
### Restart the Server 


Instructions on restarting the development server are given directly above the instructions for [logging in and using the site](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Admin_site#logging_in_and_using_the_site) in part four of the Mozilla MDN Local Library Tutorial. 

In PowerShell terminal run the "manage" script to restart the server: 

```powershell 

python manage.py runserver 

``` 



QED 

© Adam Heinz 

13 December 2023
