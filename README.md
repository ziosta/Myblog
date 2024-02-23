# Install Django (Terminal):
pip install django

# Create a Django Project (Terminal):
django-admin startproject myblog

# Create an App (Terminal):
cd myblog
python manage.py startapp blog

# Define Models (blog/models.py):
# blog/models.py

from django.db import models
from django.contrib.auth.models import User

class Category(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

# Update Database (Terminal):
python manage.py makemigrations
python manage.py migrate

# Configure Django Admin Panel (blog/admin.py):
# blog/admin.py

from django.contrib import admin
from .models import Post, Category

admin.site.register(Post)
admin.site.register(Category)

# Configure URL Routing (myblog/urls.py):
# myblog/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls')),
]

# Define Blog App URLs (blog/urls.py):
# blog/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]

# Define Views (blog/views.py):
# blog/views.py

from django.shortcuts import render
from .models import Post

def post_list(request):
    posts = Post.objects.all()
    return render(request, 'blog/post_list.html', {'posts': posts})

# Create Templates:
# Create templates/blog/post_list.html and add your desired HTML structure.

# Run the Server (Terminal):
python manage.py runserver

# Regarding the virtual environment, you can create and activate a virtual environment using the following commands:
# python -m venv myenv
# source myenv/bin/activate  # For Unix/macOS
# myenv\Scripts\activate      # For Windows
# After activating the virtual environment, you can proceed with the installation and setup of Django within that environment.
# Myblog
