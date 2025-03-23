# Installing Tailwind CSS in Django

```bash
pip install django-tailwind 'django-tailwind[reload]'
```
# ⛯ `settings.py` Configuration

```python
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

DEBUG = True                    # Or False in production

# Production-ready configuration
TAILWIND_DEV_MODE = DEBUG  # Auto-switches dev/prod modes

INSTALLED_APPS = [
    # ...
    'tailwind',
    'theme',  # Your Tailwind app
    'django_browser_reload',
]

TAILWIND_APP_NAME = 'theme'
INTERNAL_IPS = ['127.0.0.1']

# Auto-detect npm path (no hardcoding!)
NPM_BIN_PATH = os.path.join(BASE_DIR, 'node_modules/.bin/npm')


# Static files
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "theme" / "static"]
STATIC_ROOT = BASE_DIR / "staticfiles"

# Middleware (CRITICAL ORDER)
MIDDLEWARE = [
    # ...
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django_browser_reload.middleware.BrowserReloadMiddleware',  
    # ...
]
```


# 🛠️ Setting Up the Environment

Once the Django settings are configured, the next step is to set up **Node.js** and **Tailwind CSS**.  


### 🔹Install Node.js

##### After installing `nvm`, you **MUST** restart the terminal.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash | bash
```  

##### CLOSE/REOPEN TERMINAL BEFORE RUNNING:
```bash
nvm install 18 && nvm use 18
```

### 🌟 Initialize Tailwind in Django

Run the following commands step by step:

```bash
python manage.py tailwind init         # Initializing the tailwind app. (Registeration 1st)
mkdir -p theme/static                  # Create static directory
python manage.py tailwind install      # installs the actual Tailwind CSS framework using npm
python manage.py tailwind start        # Starts the tailwind in the side terminal
```

# 🔄 Enable Browser Auto-Reloading
In your project's `urls.py`, add:

```python
from django.urls import path, include

urlpatterns = [
    # Other URLs...
    path("__reload__/", include("django_browser_reload.urls")),
]
```

# Template Setup (layout.html or base.html)

```django

{% load static tailwind_tags %}    <!-- Just after <!DOCTYPE html> -->
{% tailwind_css %}                 <!-- Just before </head> -->

<body>
  <!-- Your content -->
  {% if settings.DEBUG %}
    {% include 'django_browser_reload/script.html' %}
  {% endif %}
</body>
```

# Production Deployment
```bash
python manage.py tailwind build  
python manage.py collectstatic --noinput
```

# 😍 Bonus
## Optional: Automate Workflow  (Recommended)

```bash
npm install concurrently --save-dev
```
- Create a file package.json in root directory 
- Then update package.json:

```json
{
  "scripts": {
    "dev": "concurrently 'python manage.py tailwind start' 'python manage.py runserver'"
  }
}
```
Now, you can run both Django and Tailwind simultaneously with:

```bash
npm run dev
```
