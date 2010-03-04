A basic django foursquare login app.
====================================

This application will let your users login with foursquare to your django app. It maps onto django's default users table.

To use, you'll need to set up a foursquare oauth consumer. Set its return url to http://YOURWEBSITE/auth/return/. Once that's done, add the following to your settings.py file:

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

And this pattern to your urls.py file:

    urlpatterns = patterns('',
        ...
        (r'^auth/', include('djangofoursquare.urls')),
        ...
    )

That's it! No extra database tables are needed. Use a link like the following in your templates to redirect to foursquare. The return url /auth/return will process the data and create a new django user with this information, and log that user in, then redirect back to root.

    <a href='{% url foursquare_oauth_auth %}'>Login with foursquare</a>
    
    