# Installing Tailwind CSS in Django

[![Django](https://img.shields.io/badge/django-4.2-brightgreen)](https://www.djangoproject.com/)
[![Tailwind CSS](https://img.shields.io/badge/tailwind-3.3-38B2AC)](https://tailwindcss.com/)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

## ✨ Features
- Instant CSS hot-reloading
- Production-optimized builds
- Mobile-first responsive design
- Cross-platform compatibility

```bash
pip install django-tailwind 'django-tailwind[reload]'
```
# ⛯ `settings.py` Configuration

```python
import os
from pathlib import Path
from django.core.exceptions import ImproperlyConfigured

BASE_DIR = Path(__file__).resolve().parent.parent

DEBUG = True                    # Or False in production

# Production-ready configuration
TAILWIND_DEV_MODE = DEBUG  # Auto-switches dev/prod modes

INSTALLED_APPS = [
    # ...
    'tailwind',
    'django_browser_reload',
]

TAILWIND_APP_NAME = 'theme'
INTERNAL_IPS = ['127.0.0.1']

# Auto-detection first
NPM_BIN_PATH = os.path.join(BASE_DIR, 'node_modules/.bin/npm')

# Fallback to global npm (development only)
if not os.path.exists(NPM_BIN_PATH):
    NPM_BIN_PATH = "/home/ahmad-hussain/.nvm/versions/node/v18.20.7/bin/npm"  # Check your path by running `which npm` or `where npm`
    if DEBUG:
        print(f"WARNING: Using global npm at {NPM_BIN_PATH}")
    else:
        raise ImproperlyConfigured("Production requires local npm. Run 'npm install'.")

# Static files
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "theme" / "static"]
STATIC_ROOT = BASE_DIR / "staticfiles"

# Middleware (CRITICAL ORDER)
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django_browser_reload.middleware.BrowserReloadMiddleware',  
    # ...
]
```


# 🛠️ Setting Up the Environment

Once the Django settings are configured, the next step is to set up **Node.js** and **Tailwind CSS**.  


### 🔹 Install Node.js
1. Install nvm:
   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

2. **Close and restart your terminal**
3. Install Node.js
    ```bash
    nvm install 18 && nvm use 18
    ```

### 🌟 Initialize Tailwind in Django

Run the following commands step by step:

```bash
python manage.py tailwind init         # Initializing the tailwind app. (Press Enter again to accept by default theme app)
'theme',                               # Add the app name in settings.py below the 'tailwind', (under INSTALLED_APPS)
mkdir -p theme/static                  # Required for CSS generation
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

## 🚨 Troubleshooting

| Issue                      | Solution                      |
|----------------------------|-------------------------------|
| Missing CSS                | Run `python manage.py tailwind install` |
| Auto-reload not working    | Verify `INTERNAL_IPS` in settings |
| NPM errors                 | Check `NPM_BIN_PATH` configuration |

---

# Let's Connect 🧛 
[<img src="https://img.icons8.com/3d-fluency/30/secured-letter.png" alt="Email" style="vertical-align: middle;"/> official.ahmadrandhawa@gmail.com](mailto:official.ahmadrandhawa@gmail.com)   
[<img src="https://icon.icepanel.io/Technology/svg/GitHub.svg" width="26" alt="GitHub"/> GitHub Portfolio](https://github.com/AhmadHussainRandhawa)   
[<img src="https://icon.icepanel.io/Technology/svg/LinkedIn.svg" width="26" alt="LinkedIn"/>  Daily Updates](https://www.linkedin.com/in/ahmad-hussain-randhawa/) 


*"The best AI models are built by those who understand real human needs first." – Ahmad Hussain*
