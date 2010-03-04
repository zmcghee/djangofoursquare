A basic django foursquare login app.
====================================

This application will let your users login with foursquare to your django app. It was inspired by [henriklied's twitter-django-oauth application](http://github.com/henriklied/django-twitter-oauth).

## Requirements

- Django 1.0+
- You must register [a new foursquare oAuth application](http://foursquare.com/oauth/). Set its return url to http://mysite.com/auth/return/.

## Installation

Add the 'djangofoursquare' app somewhere in your PYTHONPATH.

Add the following to your settings.py file:

    FOURSQUARE_CONSUMER_KEY = 'something'
    FOURSQUARE_CONSUMER_SECRET = 'something'
    
Add the 'djangofoursquare' app to your INSTALLED_APPS:

    INSTALLED_APPS = (
        ...
        'djangofoursquare',
        ...
    )

And the default foursquare authentication backend (You can replace this with your own authentication backend to get more customized behavior).

    AUTHENTICATION_BACKENDS = (
        ...
        'djangofoursquare.backends.FoursquareBackend',
        ...
    )

And add this pattern to your urls.py file:

    urlpatterns = patterns('',
        ...
        (r'^auth/', include('djangofoursquare.urls')),
        ...
    )

## Usage

Use a link like the following in your templates to redirect to foursquare. The return url /auth/return will process the data and create a new django user with this information, and log that user in, then redirect back to root.

    <a href='{% url foursquare_oauth_auth %}'>Login with foursquare</a>
    
## Customizing user creation

The basic django foursquare backend looks like this:

    class FoursquareBackend(BasicBackend):
        def authenticate(self, foursquare_user_data, access_token):
        
            id = foursquare_user_data['id']
            first_name = foursquare_user_data.get('firstname', '')
            last_name = foursquare_user_data.get('lastname', '')
            photo = foursquare_user_data.get('photo', '')
            gender = foursquare_user_data.get('gender', '')
            phone = foursquare_user_data.get('phone', '')
            email = foursquare_user_data.get('email', '')
        
            username = '%s%d' % (FOURSQUARE_USER_PREFIX, id)
        
            try:
                user = User.objects.get(username=username)
            except User.DoesNotExist:
                user = User.objects.create_user(username, email, None)
                user.first_name = first_name
                user.last_name = last_name
                user.save()
            
            return user

By creating a customized version of this backend, you can store the extra fields to your liking.