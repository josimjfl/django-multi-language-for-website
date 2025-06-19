# Django Multilingual Website Tutorial (English + বাংলা)

This tutorial will walk you through setting up a multilingual website in Django that supports English and Bengali (বাংলা).

---

## 🔧 Step 1: Update `settings.py`

```python
LANGUAGE_CODE = 'en'
USE_I18N = True
USE_L10N = True
USE_TZ = True

LANGUAGES = [
    ('en', 'English'),
    ('bn', 'বাংলা'),
]

LOCALE_PATHS = [
    BASE_DIR / 'locale',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.locale.LocaleMiddleware',  # Important
    'django.middleware.common.CommonMiddleware',
    ...
]
```

---

## 🌐 Step 2: Update `urls.py`

```python
from django.conf.urls.i18n import i18n_patterns
from django.contrib import admin
from django.urls import path
from myapp import views

urlpatterns = i18n_patterns(
    path('admin/', admin.site.urls),
    path('', views.home, name='home'),
)

# Add this to handle language switch
from django.views.i18n import set_language
urlpatterns += [
    path('set-language/', set_language, name='set_language'),
]
```

---

## 📄 Step 3: Create Your View

```python
# views.py
from django.shortcuts import render
from django.utils.translation import gettext as _

def home(request):
    context = {
        'message': _("Welcome to our website!"),
    }
    return render(request, 'home.html', context)
```

---

## 🎨 Step 4: Create the Template

```html
<!-- templates/home.html -->
{% load i18n %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% trans "Home Page" %}</title>
</head>
<body>
    <h1>{{ message }}</h1>

    <form action="{% url 'set_language' %}" method="post">
        {% csrf_token %}
        <select name="language">
            <option value="en">English</option>
            <option value="bn">বাংলা</option>
        </select>
        <input type="submit" value="Change Language">
    </form>
</body>
</html>
```

---

## 📝 Step 5: Generate `.po` Translation Files

```bash
# Generate messages for Bengali
python manage.py makemessages -l bn
```

Edit the generated `locale/bn/LC_MESSAGES/django.po` file:

```po
msgid "Welcome to our website!"
msgstr "আমাদের ওয়েবসাইটে স্বাগতম!"

msgid "Home Page"
msgstr "হোম পেজ"
```

Then compile the messages:

```bash
python manage.py compilemessages
```

---

## 🚀 Step 6: Run the Server

```bash
python manage.py runserver
```

Visit [http://localhost:8000](http://localhost:8000), switch language using the dropdown, and enjoy your multilingual website!

---

## ✅ Tips

* Use `{% trans %}` or `gettext` for all text you want to translate.
* Avoid using hardcoded text in templates/views.
* Remember to run `makemessages` and `compilemessages` every time you add new translatable strings.

---

Happy Coding! 🌐🇧🇩🇬🇧
