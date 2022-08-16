Full Stack Django Development

Traditional MVC:

 

Django MVT:

 
   

      

   


Create your first Django App 

Learning Objectives
•	Create your first Django project and app using command line utils
•	Create your first Django view to return a simple HTML page
Before starting the lab, make sure your current Theia directory is /home/project.
First, we need to install Django related packages.
•	Go to terminal and run:
python3 -m pip install Django

•	Once the installation is finished, you can create your first Django project by running:
django-admin startproject firstproject

A folder called firstproject will be created which is a container wrapping up settings and configurations for a Django project.
•	If your current working directory is not /home/project/firstproject, cd to the project folder
cd firstproject

•	and create a Django app called firstapp within the project
django-admin startapp firstapp

Django created a project scaffold for you containing your first firstapp app.
Your Theia workspace should look like the following:

 

The scaffold contains the fundamental configuration and setting files for a Django project and app:
•	For project-related files:
o	manage.py is a command-line interface used to interact with the Django project to perform tasks such as starting the server, migrating models, and so on.
o	firstproject/settings.py contains the settings and configurations information.
o	firstproject/urls.py contains the URL and route definitions of your Django apps within the project.
•	For app-related files:
o	firstapp/admin.py: is for building an admin site
o	firstapp/models.py: contains model classes
o	firstapp/views.py: contains view functions/classes
o	firstapp/urls.py: contains URL declarations and routing for the app
o	firstapp/apps.py: contains configuration meta data for the app
o	firstapp/migrations/: model migration scripts folder
•	Before starting the app, you will to perform migrations to create necessary database tables:
python3 manage.py makemigrations
•	and run migration
python3 manage.py migrate
•	Then start a development server hosting apps in the firstproject:
python3 manage.py runserver
To see your first Django app from Theia,
•	Click Launch Application and enter the port for the development server 8000
and you should see the following welcome page:
 

When developing Django apps, in most cases Django will automatically load the updated files and restart the development server. However, it might be safer to restart the server manually if you add/delete files in your project.
Let's try to stop the Django server now by pressing:
•	Control + C or Ctrl + C in the terminal
Add Your First View
Next, let's include your firstapp into firstproject
•	Open firstproject/settings.py file, find INSTALLED_APPS section, and add a new app
entry as
'firstapp.apps.FirstappConfig',

Your INSTALLED_APPS should look like the following

INSTALLED_APPS = [
    'firstapp.apps.FirstappConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]


You can also see some pre-installed Django apps such as admin for managing the Admin site, auth for authentication, etc.
Next, we need to add the urls.py of firstapp to firstproject so that views of firstapp can be properly routed.
•	Create an empty urls.py under firstapp folder
•	Open firstproject/urls.py, you can find a path function from django.urls import path has been already imported.
Now also import an include method from django.urls package:
from django.urls import include, path
•	Then add a new path entry
path('firstapp/', include('firstapp.urls')),
Your firstproject/urls.py now should look like the following:

 

Now you can create your first view to receive HTTPRequest and return a HTTPResponse wrapping a simple HTML page as its content.
•	Open firstapp/views.py, write your first view after the comment # Create your views here.
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
    # Create a simple html page as a string
    template = "<html>" \
                "This is your first view" \
               "</html>"
    # Return the template as content argument in HTTP response
    return HttpResponse(content=template)


Next, configure the URL for the index view.
•	Open firstapp/urls.py, add the following code:

from django.urls import path
from . import views

urlpatterns = [
    # Create a path object defining the URL pattern to the index view
    path(route='', view=views.index, name='index'),
]

That's it, now let's test your first view.
•	Run Django sever if not started:
python3 manage.py runserver

•	Clicking Launch Application and enter 8000.
After a new browser tab is opened, add a /firstapp path to your full URL, note that the userid is a place holder and should be replaced by your real id shown after clicking Launch Application
https://userid-8000.theiadocker-1.proxy.cognitiveclass.ai/firstapp/
Basically, Django will map any HTTP requests starting with /firstapp to firstapp and search any matches for paths defined in firstapp/urls.py.
That's it, you should see your the HTTPResponse returned by your first view, which is a simple HTML page with content This is your first view.
 


Coding Practice: Add a View to Return Current Date
Complete and add the following code snippet to create a view to returning today's date.
Add the a get_date() view function in firstapp/views.py (remember to save the updated file):
from django.urls import path
from . import views

urlpatterns = [
    # Create a path object defining the URL pattern to the index view
    path(route='', view=views.index, name='index'),
    # Add another path object defining the URL pattern using `/date` prefix
    path(route='date', view=views.get_date, name='date'),
]
Go to following URL and the get_date() view should return current date:
`https://userid-8000.theiadocker-1.proxy.cognitiveclass.ai/firstapp/date/`
 

           

Admin Page Lab
Learning Objectives
•	Understand the concepts of Django admin site
•	Create and customize your Django admin site to manage the content of an onlinecourse app
Start PostgreSQL in Theia
PostgreSQL, also known as Postgres, is an open-source relational database management system and it is one of the main databases Django uses.
If you are using the Theia environment hosted by Skills Network Labs, a pre-installed PostgreSQL instance is provided for you.
You can start PostgreSQL from UI by finding the SkillsNetwork icon on the left menu bar and selecting PostgreSQL from the DATABASES menu item:
 


Once the PostgreSQL has been started, you can check the server connection information from the UI. Please markdown the connection information such as generated username, password, and host, etc, which will be used to configure Django app to connect to this database.


 

User is postgress

•	Django needs an adapter called Psycopg as an interface to work with PostgreSQL, you can install it using following command:
python3 -m pip install psycopg2-binary

Also, a pgAdmin instance is installed and started for you. It is a popular PostgreSQL admin and development tool for you to manage your PostgreSQL server interactively.

 

Import an onlinecourse App Template
If the terminal was not open, go to Terminal > New Terminal and make sure your current Theia directory is /home/project.
•	Run the following command-lines to download a code template for this lab
wget "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0251EN-SkillsNetwork/labs/m4_django_app/lab2_template.zip"  
unzip lab2_template.zip
rm lab2_template.zip

 

Next, we need to set up a proper runtime environment for Django app.
•	If the terminal was not open, go to Terminal > New Terminal and cd to the project folder
pip3 install -r requirements.txt

cd lab2_template
pip3 install -r requirements.txt
The requirements.txt contains all necessary Python packages for you to run this lab.
•	Open settings.py and find DATABASES section, and replace the values of USER and PASSWORD to be the generated PostgreSQL user and password.
Next, activate the models for an onlinecourse app which will be managed by admin site.
•	You need to perform migrations to create necessary tables
python3 manage.py makemigrations
 


•	and run migration
python3 manage.py migrate

 

Create a Superuser for Admin Site
Before you can access admin site, you will need to create a super user for the admin site.
•	run
python3 manage.py createsuperuser

 

Password:Sovik@1989

Let's start our app and login with the super user.
•	Then start the development server
python3 manage.py runserver

•	Click Launch Application and enter the port for the development server 8000
When the browser tab opens, add the /admin path and your full URL should look like the following
https://userid-8000.theiadocker-1.proxy.cognitiveclass.ai/admin
•	Login with the user name and password for the super user, then you should see
admin site with only Groups and Users entries created for us. These two tables are created by Django by default for authentication and authorization purposes.


 

Register Models with Admin site
Once you register models defined in adminsite/models.py with admin site, you can then create managing pages for those models.
•	Open adminsite/admin.py, and register Course and Instructor models
admin.site.register(Course)
admin.site.register(Instructor)

Now refresh the admin site, and you should be able to see Courses and Instructors under ADMINSITE section.

Let's create an instructor first. For simplification, we can assign the super user to be an instructor
•	Click the green Add button in the Instructors row
•	For the User field, choose the super user you just created:
 
For fields in Instructor model, Django automatically creates corresponding web widgets for us to receive their values. For example, a checkbox is created for Full Time field and numeric input field for Total learners.
•	Then choose if the instructor to be Full Time or not and enter 0 for Total learners.
•	Similarly, you can create a course by entering its Name, Description, Publication date, etc.
For the Image field, we defined an ImageField in Course model, and then Django admin site app automatically adds image uploading functionality for us.
 
Now you should have an Instructor and Course created.
We leave objects delete and edit for you to explore as practice. You could also play with the admin site yourself to create more instructors and courses.
Next, let's customize our admin site.
Customize Admin Site
You may only want to include some of the model fields in the admin site. To select the fields to be included, we create a ModelAdmin class and add a fields list with the field names to be included.
•	Open adminsite/admin.py, add a CourseAdmin class:
class CourseAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'name', 'description']
•	Update previous course registration admin.site.register(Course) with the CourseAdmin class:
admin.site.register(Course, CourseAdmin)
Now your adminsite/admin.py should look like the following:
from django.contrib import admin
from .models import Course, Instructor, Lesson

class CourseAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'name', 'description']

admin.site.register(Course,CourseAdmin)
admin.site.register(Instructor)


•	Refresh the admin page and try adding a course again, and you should see only
Pub date, Name, Description fields are included.
 

Coding Practice: Customize Fields for Instructor Model
Include only user and Full Time fields for Instructor model.


from django.contrib import admin
from .models import Course, Instructor, Lesson

class CourseAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'name', 'description']

admin.site.register(Course,CourseAdmin)

class InstructorAdmin(admin.ModelAdmin):
    fields = ['user','full_time']

admin.site.register(Instructor, InstructorAdmin)


Associate Related Models
In previous coding practice, you were asked to include user model, which acts as a parent model for instructor, and you can add such related object while creating an Instructor object.
Django admin provides a convenient way to associate related objects on a single model managing page. This can be done by defining Inline classes.
Let's try to manage Lesson model together with Course model on Course admin page.
•	Open adminsite/admin.py, add a LessonInline class before CourseAdmin:
class LessonInline(admin.StackedInline):
    model = Lesson 
    extra = 5

and update CourseAdmin class by adding a inlines list

class CourseAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'name', 'description']
    inlines = [LessonInline]

Now you can see by adding the inlines list, you can adding lessons while creating a course

ummary
In this lab, you have learned how to use the admin site provided by Django to manage the models defined in the models.py of your Django app. You have also learned how to customize the admin site to include the subset of model fields as well as creating Inline classes to add related objects on the same page.
Django View
   

 

Django Template

 
 

 

     
 


Learning Objectives
•	Create function-based views to handle HTTP requests and return HTTP responses
•	Create templates for rendering HTML pages
Learning Objectives
•	Create function-based views to handle HTTP requests and return HTTP responses
•	Create templates for rendering HTML pages
wget "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0251EN-SkillsNetwork/labs/m4_django_app/lab3_template.zip"  
unzip lab3_template.zip
rm lab3_template.zip

•	cd to the project folder
cd lab3_template

Next, install required packages for this lab:

pip3 install -r requirements.txt
Your Django project should look like following:

 

Open myproject/settings.py and find DATABASES section and you will notice that we use SQLite this lab.
SQLite is a file-based embedding database with some course data pre-loaded. Thus you don't need to use Model APIs or admin site to populate data by yourself and can focus on creating views and templates.
Next activate the models for an onlinecourse app.
•	You need to perform migrations for first-time running to create necessary tables
python3 manage.py makemigrations
•	and run migration to activate models for onlinecourse app.
python3 manage.py migrate

Create a Popular Course List View with Template
Now we can start to write views to read data from database using methods in models and add data to templates to render dynamic HTML pages.
In this step, we will create a popular course list view as the index or main page of the onlinecourse app.
First let's create a course list template.
•	Open onlinecourse/templates/onlinecourse/course_list.html and copy the following
code snippet under <!-- Add your template there -->
{% if course_list %}
    <ul>
    {% for course in course_list %}
        <div>
            <img src="{{ MEDIA_URL }}/{{ course.image }}" width="360" height="360" >
            <h1><b>{{ course.name }}</b></h1>
        </div>
    {% endfor %}
    </ul>
{% else %}
<p>No courses are available.</p>
{% endif %}
In above code snippet, we first add a {% if course_list %} if tag to check if the course_list exists in the context sent by the index view.
If it exists, we then add a {% for course in course_list %} to iterate the course list and display the fields of course such as image, name, etc.
Next, let's create a view to provide a course list to the template via Course model.
•	Open onlinecourse/views.py, add a view to get top-10 popular courses from models.
The courses objects are ordered by total_enrollment in desc order and the first ten objects are sliced as the top-10 courses, Course.objects.order_by('-total_enrollment')[:10].
Then we create a context dictionary object and append course_list into context.
def popular_course_list(request):
    context = {}
    # If the request method is GET
    if request.method == 'GET':
        # Using the objects model manage to read all course list
        # and sort them by total_enrollment descending
        course_list = Course.objects.order_by('-total_enrollment')[:10]
        # Appen the course list as an entry of context dict
        context['course_list'] = course_list
        return render(request, 'onlinecourse/course_list.html', context)

Next, add a route path for the popular_course_list view.
•	Open onlinecourse/urls.py, add a path entry in urlpatterns:
path(route='', view=views.popular_course_list, name='popular_course_list'),

and your urlpatterns list should look like the following:
urlpatterns = [
    # Add path here
    path(route='', view=views.popular_course_list, name='popular_course_list'),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)\
 + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
Now we have created a view and a template to display top-10 popular courses. Let's test the view and template
•	run
python3 manage.py runserver

•	Click Launch Application and enter the port for the development server 8000
When a new browser tab opens, add the /onlinecourse path and your full URL should look like the following
https://userid-8000.theiadocker-1.proxy.cognitiveclass.ai/onlinecourse
and you should see a course list with two courses Introduction to Django and Introduction to Python.
The template we just created is a plain HTML template without any styling or formatting, we will improve its looking in later steps.
Coding Practice: Add More Fields to Course
Complete the following template course_list.html to add total_enrollment and description fields to a course on the list.
{% if course_list %}
    <ul>
    {% for course in course_list %}
        <div>
            <img src="{{ MEDIA_URL }}/{{ course.image }}" width="360" height="360" >
            <h1><b>{{ course.name }}</b></h1>
            <p>{{course.total_enrollment}} enrolled</p>
            <p>{{course.description}}</p>
        </div>
    {% endfor %}
    </ul>
{% else %}
<p>No courses are available.</p>
{% endif %}
Create an Course Enrollment View
Next let's create a simplified user enrollment process by creating a view to update the total_enrollment field of a course.
•	Update course_list.html to add a form for submitting an enrollment request.
It sends a POST request to a URL parsed from {% url 'onlinecourse:enroll' course.id %} tag mapping to a update view.
{% if course_list %}
    <ul>
    {% for course in course_list %}
        <div>
            <img src="{{ MEDIA_URL }}/{{ course.image }}" width="360" height="360" >
            <h1><b>{{ course.name }}</b></h1>
            <p>{{course.total_enrollment}} enrolled</p>
            <p>{{ course.description }}</p>
            <form action="{% url 'onlinecourse:enroll' course.id %}" method="post">
                {% csrf_token %}
                <input type="submit" value="Enroll">
            </form>
        </div>
    {% endfor %}
    </ul>
{% else %}
<p>No courses are available.</p>
{% endif %}
Create an enroll view to add one to the total_enrollment field
•	Open onlinecourse/views.py, add an enroll view:
def enroll(request, course_id):
    # If request method is POST
    if request.method == 'POST':
        # First try to read the course object
        # If could be found, raise a 404 exception
        course = get_object_or_404(Course, pk=course_id)
        # Increase the enrollment by 1
        course.total_enrollment += 1
        course.save()
        # Return a HTTP response redirecting user to course list view
        return HttpResponseRedirect(reverse(viewname='onlinecourse:popular_course_list'))

The enroll view first reads a course object based on the course_id argument, if the course doesn't exist in database, it returns a HTTP response with status code 404 Not Found error.
Then it simply increases the total enrollment by one and updates the object.
You should always return an HttpResponseRedirect after successfully dealing with POST data. HttpResponseRedirect takes a URL argument to which the user will be redirected. Here we are using the reverse() function in the HttpResponseRedirect to generate the URL for popular_course_list view (e.g., https://userid-8000.theiadocker-1.proxy.cognitiveclass.ai/onlinecourse/).
For now, we just simply redirect user to the same page with viewname='onlinecourse:popular_course_list'.
•	Open onlinecourse/urls.py add a path('course/<int:course_id>/enroll/', views.enroll, name='enroll'),
to create a route to enroll view
path('course/<int:course_id>/enro
path('course/<int:course_id>/enroll/', views.enroll, name='enroll'),

Save all updated files and refresh the course list page, and you should see a Enroll button under each course. Click that the course list page will be refreshed with enrollment count increased by 1.


 - 
Helvetica N...
 
▼
   
 Step 6 of 9 
 
Create a Course Details View with Template
In the previous step, a user will be redirected to the course list page after enrolled a course. This is not a practical scenario because users should be redirected into the details of a course, so that they can read the learning materials and take exams.
To make the user scenario more practical, we will be creating a course detail page and redirecting users to the detail page after enrollment.
Let's start with creating a course detail template.
Open onlinecourse/templates/onlinecourse/course_detail.html, adding the following code snippet
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    {% load static %}
</head>
<body>
 <div>
      <h2>{{ course.name }}</h2>
      <h5>{{ course.description }}</h5>
 </div>

</body>
</html>

•	The above code snippet adds two variables {{course.name}} and {{course.description}}. They are wrapped in two HTML titles to represent the details of a course given by a course id.
•	Next, let's add a view to read the requested course and send context to course_detail.html template.
•	Open onlinecourse/views.py, add a course_details view
The above code snippet first try to get the specific course given by course id and add it to the context to be used for rendering HTML page.
If the course can not be found, a Http404 exception will be raised.
Next, we can configure the route for the course_details page.
•	Open onlinecourse/urls.py, add path entry path('course/<int:course_id>/', views.course_details, name='course_details'),
•	Next, go back to onlinecourse/views.py and update the enroll view function to redirect user to the web page generated by
course_details view.
return HttpResponseRedirect(reverse(viewname='onlinecourse:course_details', args=(course.id,)))
Note that we added a course.id in the URL as an argument so the redirecting URL will look like this:
https://userid-8000.theiadocker-1.proxy.cognitiveclass.ai/onlinecourse/course/1/
We have the view and template created for course details, let's test them.
You can refresh the course list page and click the enroll, then the app will bring you the course details page generated by the view and template we just created.

#Coding Practice: Add Lessons to Course Details Page
Each course has One-To-Many relationships to Lesson. In the pre-load database, we already created several lessons for a course (feel free to use admin site to add more lessons).
Complete and add the following HTML code snippet (under {{course.description }}) to course_details.html to show the lessons on course details page.
    <h2>Lessons: </h2>
    {% for lesson in course.lesson_set.all %}
         <div>
             <h5>Lesson {{lesson.order}} : {{lesson.title}}</h5>
             <p>{{lesson.content}}</p>
         </div>
    {% endfor %}

Add CSS to Templates
The course list and course details pages we created were looking very primitive because they are pure HTML pages without any CSS styling.
Let's try to stylize the templates by adding some CSS style classes to the HTML elements in the templates.
We have provided a sample css file called course.css, you just need to include it into your templates and use the style classes.
•	Open course_list.html, include the link of course.css static file to <head> element:
•	Then, stylize the course list <div> (under {% for course in course_list %} tag) in a .container class which adds some paddings.
•	Try to add a <div class="row"> to wrap up course fields as a table row.
•	Use a <div class="column-33"> to wrap up and divide the course image as a column takes up 33% width
•	Use a <div class="column-66"> to wrap up name, total_enrollment, description, and enroll form
Feel free to add more styles such as making the enrollment numbers green
•	The sytlized course list should look like the following:
{% if course_list %}
   <ul>
   {% for course in course_list %}
       <div class="container">
          <div class="row">
            <div class="column-33">
                <img src="{{MEDIA_URL}}/{{ course.image }}" width="360" height="360" >
            </div>
            <div class="column-66">
                <h1 class="xlarge-font"><b>{{ course.name }}</b></h1>
                <p style="color:MediumSeaGreen;"><b>{{course.total_enrollment}} enrolled</b></p>
                <p> {{ course.description }}</p>
                <form action="{% url 'onlinecourse:enroll' course.id %}" method="post">
                {% csrf_token %}
                <input class="button"  type="submit"  value="Enroll">
              </form>
            </div>
          </div>
       </div>
       <hr>
   {% endfor %}
   </ul>
{% else %}
<p>No courses are available.</p>
{% endif %}

Refresh the course list page, and check the result. Your course list page should look like the following:
 


Coding Practice: Stylize Course Details Page
Playing with other CSS classes in the onlinecourse/course.css file, such as .card class to make a lesson looking like a card.
•	Update course_detail.html and add .card class to the <div> element wrapping up
course name, description and each lesson in the course:



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    {% load static %}
    <link rel="stylesheet" type="text/css" href="{% static 'onlinecourse/course.css' %}">
</head>
<body>
    <div class="card">
        <h2>{{ course.name }}</h2>
        <h5>{{ course.description }}</h5>
    </div>
    <h2>Lessons: </h2>
    {% for lesson in course.lesson_set.all %}
    <div class="card">
        <h5>Lesson {{lesson.order}} : {{lesson.title}}</h5>
        <p>{{lesson.content}}</p>
    </div>
    {% endfor %}
</body>
</html>



