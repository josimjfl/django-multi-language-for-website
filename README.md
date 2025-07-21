Django i18n (অনুবাদ) যুক্ত করার টিউটোরিয়াল Django Multi Language Translation webstie Tutorial.
---

### 📄 `django_translation_tutorial.md`

````markdown
# 🌐 Django Website Translation (i18n) Setup Tutorial

This tutorial shows how to add multi-language (e.g., English and Bangla) support to your Django website using the built-in internationalization (i18n) system.

---

## ✅ Step 1: Enable i18n in Django Settings

In your `settings.py`, add or check the following settings:

```python
USE_I18N = True

LANGUAGE_CODE = 'en'

LANGUAGES = [
    ('en', 'English'),
    ('bn', 'বাংলা'),
]

LOCALE_PATHS = [
    BASE_DIR / 'locale',
]
````

---

## ✅ Step 2: Use `{% trans %}` or `{% blocktrans %}` in Templates

Wrap text like this:

```django
<!-- For single words or short phrases -->
<h1>{% trans "Welcome" %}</h1>

<!-- For longer paragraphs or variables inside text -->
{% blocktrans %}
  Hello {{ username }}, welcome to our platform!
{% endblocktrans %}
```

---

## ✅ Step 3: Mark Strings in Python Code for Translation

Use `gettext` (aliased as `_`) in views, models, forms, etc.

```python
from django.utils.translation import gettext as _

def home(request):
    message = _("Welcome to our website!")
    return render(request, 'home.html', {'message': message})
```

---

## ✅ Step 4: Create Translation Files

Run this command to create `.po` files for each language:

```bash
django-admin makemessages -l bn
```

It will generate:
📁 `locale/bn/LC_MESSAGES/django.po`

---

## ✅ Step 5: Translate the Strings

Open `locale/bn/LC_MESSAGES/django.po`
Translate each `msgid` to `msgstr`. Example:

```po
msgid "Welcome"
msgstr "স্বাগতম"
```

---

## ✅ Step 6: Compile Translations

After editing `.po` files, compile them with:

```bash
django-admin compilemessages
```

It creates `.mo` files Django uses to load translations.

---

## ✅ Step 7: Language Switcher View

In your `views.py`, add:

```python
from django.utils import translation
from django.http import HttpResponseRedirect

def set_language(request):
    lang = request.GET.get('lang', 'en')
    if lang in ['en', 'bn']:
        translation.activate(lang)
        request.session['django_language'] = lang
    return HttpResponseRedirect(request.META.get('HTTP_REFERER', '/'))
```

---

## ✅ Step 8: Language Switcher Link

In your template:

```html
<a href="{% url 'set_language' %}?lang=en">English</a> |
<a href="{% url 'set_language' %}?lang=bn">বাংলা</a>
```

Make sure the `set_language` view is mapped in `urls.py`.

---

## ✅ Step 9: Use the Translated Text

Anywhere you use `{% trans %}` or `_()`, the translated text will be shown automatically based on session language.

---

## ✅ Optional: Detect & Use Browser Language

You can auto-detect language using middleware or accept-language headers. But session-based approach is easier and reliable.

---

## ✅ Notes

* Static HTML files must be translated manually.
* Use `makemessages -l bn -e html,txt,py` if you're translating multiple file types.
* You must restart the server after changing translations.

---

## ✅ Bonus: Paragraph Translation with Variables

```django
{% blocktrans with total=order.total user_name=user.name %}
  Hello {{ user_name }}, your order total is {{ total }}.
{% endblocktrans %}
```

---

## ✅ Helpful Commands

```bash
# Create message file for a language
django-admin makemessages -l bn

# Compile translated messages
django-admin compilemessages
```

---

Enjoy your multilingual Django app! 🌍

```

---

**If you'd like, I can export this tutorial as a downloadable `.md` file or copy it to your project folder. Let me know!**
```
