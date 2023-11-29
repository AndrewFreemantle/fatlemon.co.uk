---
title: Setting up Django on Windows Azure Websites
permalink: /2014/01/setting-up-django-on-windows-azure-websites/
tags:
  - Django
  - Windows Azure
---
A couple of things caught me out when I was trying to deploy a Django application to Windows Azure, so I'm making a note of them here for future reference. Oh, and I hope they might be of use to you as well 🙂

### Configuring Azure Websites to run Python and Django

The [Windows Azure Websites and Django getting started tutorial](http://www.windowsazure.com/en-us/documentation/articles/web-sites-python-create-deploy-django-app/#web-site-configuration) directs us to put the required settings into the Azure management portal, but that's not how Microsoft do it in the Gallery, oh no. If we create a Django application from the Windows Azure Website gallery, the resulting site puts all the configuration into a `web.config`.

Personally, I like the management portal approach, but I did play with the `web.config` and got it working for my custom site, so I present it here in case this works for you:

``` html
<configuration>
  <appSettings>
    <add key="PYTHONPATH" value="D:\home\site\wwwroot;D:\home\site\wwwroot\site-packages" />
    <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
    <add key="DJANGO_SETTINGS_MODULE" value="{django-app-name}.settings" />
  </appSettings>
  <system.webServer>
    <handlers>
      <add name="Python_FastCGI"
           path="handler.fcgi"
           verb="\*"
           modules="FastCgiModule"
           scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
           resourceType="Either"
           requireAccess="Script" />
    </handlers>
    <rewrite>
      <rules>
        <rule name="Django Application" stopProcessing="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
          </conditions>
          <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="false" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

There are three things you need to do to get this working:

 1. Check the **PYTHONPATH** - *It needs pointing to the root of your Django Application (the directory that holds `manage.py`, and the location of the directory `site-packages` (obviously, copy that directory into Azure from your development environment if it's not there)*
 2.  Set **DJANGO\_SETTINGS\_MODULE** - *Point this to your `settings.py` file. Replace directory slashes / with dots, and omit the .py from the end*
 3. Create the file **`handler.fcgi`** - *This is required, and it's a just a plain text file containing two double-quotes (i.e. `""`) on the first line followed by a return carriage*

Seriously though, just put the [settings into the Azure management portal](http://www.windowsazure.com/en-us/documentation/articles/web-sites-python-create-deploy-django-app/#web-site-configuration). It's easier 🙂

### Bad Request (400)

Following the advice in `settings.py` the first thing I did after copying the files into Azure and seeing the Django "cool, now get cracking" default page was to trip `DEBUG` to False (`DEBUG = False`), then refresh the web browser..

!["Django on Windows Azure Websites - Bad Request (400)"]({{ site.imageurl }}2014/Django-WindowsAzure-BadRequest.png)
<figcaption>Django on Windows Azure Websites - Bad Request (400)</figcaption>

Ouch 😢

Essentially, if `DEBUG` is `False`, we have to put our server's URL into the `ALLOWED_HOSTS` setting. Now, we could do that in `settings.py` but this is really Azure specific just like the configuration above, so it really belongs in Azure.

In the Azure management portal, under the "Website > Configure" tab, find the "app settings"; section near the bottom and add the following:

<table class="table">
  <thead>
    <tr>
      <td>Setting</td>
      <td>Value</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ALLOWED_HOSTS</code></td>
      <td>Your Azure URL: e.g. <code>mydjangosite.azurewebsites.net</code>
      <br /><br />If you have custom domains pointed at Azure, you can use a space-separated list like so: <code>mydjangosite.azurewebsites.net myjangosite.com</code></td>
    </tr>
    <tr>
      <td><code>SECRET_KEY</code></td>
      <td>While we're adding new application settings, create a new 50 character random string and copy it here. We'll use it in a second
      </td>
    </tr>
  </tbody>
</table>


Now, we could just modify our `settings.py`, but it's much better to have a configuration file specifically for production / Azure, so first of all, copy your existing `settings.py` to a new file in the same directory - I like to use the filename `settings_azure.py`.

Now, assuming you have a shiny new `settings_azure.py` file, delete everything in it and replace it with this:

``` python
# ./settings_azure.py
from settings import *
```

### Now just overwrite the important settings

<span class="text--warning"><strong><i class="fa-solid fa-fw fa-exclamation-circle"></i>SECURITY WARNING:</strong> keep the secret key used in production secret!</span>

``` python
# ./settings_azure.py
SECRET_KEY = os.getenv('SECRET_KEY')  #  - defined in Windows Azure app settings
```

<span class="text--warning"><strong><i class="fa-solid fa-fw fa-exclamation-circle"></i>SECURITY WARNING:</strong> don't run with debug turned on in production!</span>

``` python
# ./settings_azure.py
DEBUG = False

TEMPLATE_DEBUG = DEBUG

ALLOWED_HOSTS = os.getenv('ALLOWED_HOSTS').split()  #  - defined in Windows Azure app settings
```


### Handy file logging, in case we run into problems on production

``` python
# ./settings_azure.py
### Uncomment to log Django errors to the root of your Azure Website
"""
LOGGING = {
  'version': 1,
  'disable_existing_loggers': False,
  'filters': {
    'require_debug_false': {
      '()': 'django.utils.log.RequireDebugFalse'
    }
  },
  'handlers': {
    'logfile': {
      'class': 'logging.handlers.WatchedFileHandler',
      'filename': 'D:/home/site/wwwroot/error.log'
    },
  },
  'loggers': {
    'django': {
      'handlers': ['logfile'],
      'level': 'ERROR',
      'propagate': False,
    },
  }
}
"""
```

If you created this new `settings_azure.py` file, don't forget to adjust the `DJANGO_SETTINGS_MODULE` value to point to it in the Azure management portal, or your `web.config` if you went that route, you heathen!
