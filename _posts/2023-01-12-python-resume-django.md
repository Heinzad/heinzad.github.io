AIDE MEMOIRE 

How to Resume a Django project
================================= 

When returning to a Django project, reactivate the virtual environment and restart the server using the Powershell terminal. 



--- 
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
