# djproxy [![Build Status](https://travis-ci.org/thomasw/djproxy.png?branch=master)](https://travis-ci.org/thomasw/djproxy)

djproxy is a class-based generic view reverse HTTP proxy for Django.

## Why?

If your application depends on a proxy (to get around Same Origin Policy issues
in JavaScript, perhaps), djproxy can be used during development to provide that
functionality.

djproxy is not intended to be used in production, but should suffice for
development. Use your web server's proxy capabilities in the wild.

## Installation

```
pip install djproxy
```

djproxy requires requests >= 1.0.0 and django >= 1.4.0.

## Usage

Start by defining a new proxy:

```python
from djproxy.views import HttpProxy

class LocalProxy(HttpProxy):
    base_url = 'https://google.com/'
```

Add a url pattern that points at your proxy view. The `url` kwarg will be
urljoined with base_url:

```python
urlpatterns = patterns(
    '',
    url(r'^local_proxy/(?P<url>.*)$', LocalProxy.as_view(), name='proxy')
)
```

`/local_proxy/some/content` will now proxy `https://google.com/some/content/`.


### HttpProxy configuration:

* `base_url`: The proxy url is formed by
   `urlparse.urljoin(base_url, url_kwarg)`
* `ignored_downstream_headers`: A list of headers that shouldn't be forwarded
  to the browser from the proxied endpoint.
* `ignored_request_headers`: A list of headers that shouldn't be forwarded
  to the proxied endpoint from the browser.
* `pass_query_string`: A boolean indicating whether the query string should be
  sent to the proxied endpoint.

## Contributing

To run the tests, first install the dependencies:

```
pip install -r requirements.txt
```

If you'd like to test this against a version of Django other than 1.5, wipe out
the 1.5 installation from `requirements.txt` by installing the desired version.

Run `nosetests` to execute the test suite.

To automatically run the test suite, pyflakes, and pep8 checks whenever python
files change use testtube by executing `stir` in the top level djproxy
directory.

To run a Django dev server that proxies itself, execute the following:

```
django-admin.py runserver --settings=tests.test_settings --pythonpath="./"
```

Similarly, to run a configure Django shell, execute the following:

```
django-admin.py shell --settings=tests.test_settings --pythonpath="./"
```

See `tests/test_settings.py` and `tests/test_urls.py` for configuration
information.
