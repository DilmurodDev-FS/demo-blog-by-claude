# Django Blog — Full Setup Guide

A beginner-friendly blog application built with Django and Bootstrap 5.

## Features
- User registration, login, and logout
- Create, read, update, and delete (CRUD) blog posts
- Comment on posts
- Pagination (5 posts per page)
- Author-only edit/delete controls
- Django admin panel included
- SQLite database (zero configuration)

---

## Project Structure

```
blog_project/
│
├── manage.py                    ← Django's command-line tool (run this!)
│
├── blog_project/                ← Project configuration package
│   ├── settings.py              ← All settings (database, apps, etc.)
│   ├── urls.py                  ← Root URL routing
│   └── wsgi.py                  ← Web server gateway (for deployment)
│
├── blog/                        ← The blog app (all our logic lives here)
│   ├── models.py                ← Database tables: Post, Comment
│   ├── views.py                 ← Business logic (what happens on each page)
│   ├── forms.py                 ← Form definitions and validation
│   ├── urls.py                  ← Blog-specific URL patterns
│   └── admin.py                 ← Admin panel configuration
│
└── templates/                   ← All HTML files
    ├── base.html                ← Shared layout (navbar, footer, messages)
    ├── blog/
    │   ├── post_list.html       ← Homepage with all posts
    │   ├── post_detail.html     ← Single post + comments
    │   ├── post_form.html       ← Create / edit post form
    │   └── post_confirm_delete.html
    └── registration/
        ├── login.html
        └── register.html
```

---

## Step-by-Step Setup

### Step 1 — Make sure Python is installed

Open your terminal and check:

```bash
python --version
# Should show Python 3.8 or higher
```

If not installed, download it from https://python.org

---

### Step 2 — Create a virtual environment

A virtual environment keeps this project's dependencies isolated from other Python projects on your machine.

```bash
# Navigate to the blog_project folder
cd blog_project

# Create the virtual environment (only do this once)
python -m venv venv

# Activate it:
# On Mac/Linux:
source venv/bin/activate

# On Windows:
venv\Scripts\activate
```

You'll know it's active when you see `(venv)` at the start of your terminal prompt.

---

### Step 3 — Install Django

```bash
pip install django
```

Verify it installed correctly:

```bash
django-admin --version
# Should show 4.x or 5.x
```

---

### Step 4 — Run database migrations

Django uses "migrations" to create the database tables from your models.

```bash
python manage.py makemigrations
python manage.py migrate
```

This creates a `db.sqlite3` file — that's your database. No setup required!

---

### Step 5 — Create an admin superuser (optional but recommended)

This gives you access to Django's built-in admin panel at `/admin/`.

```bash
python manage.py createsuperuser
```

Follow the prompts to set a username, email, and password.

---

### Step 6 — Start the development server

```bash
python manage.py runserver
```

You should see:

```
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Open your browser and go to: **http://127.0.0.1:8000**

---

## Using the App

| URL | What it does |
|-----|-------------|
| `/` | Homepage — all posts |
| `/register/` | Create a new account |
| `/accounts/login/` | Log in |
| `/post/new/` | Write a new post (login required) |
| `/post/<id>/` | View a specific post + comments |
| `/post/<id>/edit/` | Edit a post (author only) |
| `/post/<id>/delete/` | Delete a post (author only) |
| `/admin/` | Django admin panel (superuser only) |

---

## How the Key Pieces Work

### Models (models.py)
Models are Python classes that define your database tables. Django reads them and creates the actual SQL tables for you — you never write SQL directly.

```python
class Post(models.Model):
    title   = models.CharField(max_length=200)  # TEXT column, max 200 chars
    content = models.TextField()                 # TEXT column, unlimited
    author  = models.ForeignKey(User, ...)       # Links to the users table
```

### Views (views.py)
Views are functions that receive an HTTP request and return an HTTP response. They're the glue between your database and your templates.

```python
def post_list(request):
    posts = Post.objects.all()        # SELECT * FROM blog_post
    return render(request, 'blog/post_list.html', {'posts': posts})
```

### Templates (templates/)
Templates are HTML files with special Django tags for displaying data:
- `{{ variable }}` — outputs a value
- `{% for item in list %}` — loops
- `{% if condition %}` — conditionals
- `{% url 'name' %}` — generates a URL by name
- `{% extends 'base.html' %}` — inherits a layout

### URLs (urls.py)
URL patterns map web addresses to view functions:
```python
path('post/<int:pk>/', views.post_detail, name='post-detail')
#     ↑ URL pattern     ↑ view function    ↑ name used in templates
```

---

## Common Commands Reference

```bash
# Start the server
python manage.py runserver

# Create new migrations after changing models.py
python manage.py makemigrations
python manage.py migrate

# Open the Django shell (interactive Python with DB access)
python manage.py shell

# Create a superuser for /admin/
python manage.py createsuperuser
```

---

## Next Steps to Extend This Project

- **Add tags/categories** to posts using a ManyToManyField
- **Add image uploads** to posts using ImageField + Pillow
- **Add a search bar** using Q objects in Django ORM
- **Add user profiles** with a Profile model linked to User
- **Add like/upvote** functionality on posts
- **Deploy to the web** using Railway, Render, or PythonAnywhere (all free tiers available)
