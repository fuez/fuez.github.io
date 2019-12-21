---
title: python
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-12-21T01:33:29Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## How to upgrade chromedriver?

Simply download driver version that's compatible with your current chrome version from [web driver](https://sites.google.com/a/chromium.org/chromedriver/downloads), unzip it and then `mv chromedriver /usr/local/bin`

## How to sort name in Chinese?

```
from pypinyin import lazy_pinyin

# names is a string separated by `;`
sorted([_.strip() for _ in names.split(";")], key=lambda x: ''.join(lazy_pinyin(x)))

```

## Configure webdriver.FireFox to use proxy

``` python
    profile = webdriver.FirefoxProfile()
    profile.set_preference("network.proxy.type", 1)
    profile.set_preference("network.proxy.http", "127.0.0.1")
    profile.set_preference("network.proxy.http_port", 1087)
    profile.set_preference("network.proxy.ssl", "127.0.0.1")
    profile.set_preference("network.proxy.ssl_port", 1087)
    profile.update_preferences()
```

## When debugging, try to use `var.__dict__.keys()` to show help for that variable, which show less than `dir`

## Python now supports [overload](https://www.python.org/dev/peps/pep-3124/) like this:

```python
from overloading import overload
from collections import Iterable

def flatten(ob):
    """Flatten an object to its component iterables"""
    yield ob

@overload
def flatten(ob: Iterable):
    for o in ob:
        for ob in flatten(o):
            yield ob

@overload
def flatten(ob: basestring):
    yield ob
```

## Great code to study and review on python: [Pallets](https://github.com/pallets)

## Use `functools.total_ordering` to create a comparable object in python

```python
@total_ordering
class WrappedNode:
    def __init__(self, node: ListNode):
        self._node = node

    def __eq__(self, other):
        if not other:
            return False
        return self._node.val == other._node.val

    def __lt__(self, other):
        if not other:
            return False
        return self._node.val < other._node.val
```

## How to automatically import your favorite libraries to ipython?
>> Navigate to ~/.ipython/profile_default
>> Create a folder called startup if it’s not already there
>> Add a new Python file called start.py
>> Put your favorite imports in this file
>> Launch IPython or a Jupyter Notebook and your favorite libraries will be automatically loaded every time!

## Run flaks app with gunicorn: `gunicorn -b 0.0.0.0:8000 pythonfile:app`

##  Difference between `__new__` and `__init__`
> Use __new__ when you need to control the creation of a new instance. Use __init__ when you need to control initialization of a new instance.
>
> __new__ is the first step of instance creation. It's called first, and is responsible for returning a new instance of your class. In contrast, __init__ doesn't return anything; it's only responsible for initializing the instance after it's been created.
>
> *In general, you shouldn't need to override __new__ unless you're subclassing an immutable type like str, int, unicode or tuple.*

## python provides permutations and combinations algorithm in `itertools`, the limitation is that those algorithms do not handle duplicate items

```python
from itertools import permuations, combinations

list(permutations(range(1, 6)))
list(combinations(range(1, 6), 3))

```

## Set up [autopep8](https://pypi.org/project/autopep8/)

For autopep8 to work for large source file in vscode, make sure to add timeout conf such as  `"editor.formatOnSaveTimeout": 1500` to settings.json
And autopep8 reads configuration from `pycodestyle.ini`, not `flake8.ini` even if you configure flake8 in vscode.

## `pytest` supports running Python [`unittest`-based](https://docs.pytest.org/en/latest/unittest.html) tests out of box

## Python `str` provides not only `split` function, but also `rsplit()` to split from right and an optional count parameter

## Read an article explaining [TCP "three-way handshake"](https://packetlife.net/blog/2010/jun/7/understanding-tcp-sequence-acknowledgment-numbers/)
> "SYN, SYN/ACK, ACK."
> 
> TCP utilizes a number of flags, or 1-bit boolean fields, in its header to control the state of a connection. The three we're most interested in here are:

    SYN - (Synchronize) Initiates a connection
    FIN - (Final) Cleanly terminates a connection
    ACK - Acknowledges received data

## Read wsgi spec in [PEP3333](https://www.python.org/dev/peps/pep-3333/)
> This PEP, therefore, proposes a simple and universal interface between web servers and web applications or frameworks: the Python Web Server Gateway Interface (WSGI).
>
> The WSGI interface has two sides: the "server" or "gateway" side, and the "application" or "framework" side. The server side invokes a callable object that is provided by the application side.
>
> In addition to "pure" servers/gateways and applications/frameworks, it is also possible to create "middleware" components that implement both sides of this specification.
>
> The application object is simply a callable object that accepts two arguments
>
> The server or gateway invokes the application callable once for each request it receives from an HTTP client, that is directed at the application.

> "middleware" components can perform such functions as:
	- Routing a request to different application objects based on the target URL, after rewriting the environ accordingly.
	- Allowing multiple applications or frameworks to run side-by-side in the same process
	- Load balancing and remote processing, by forwarding requests and responses over a network
	- Perform content postprocessing, such as applying XSL stylesheets
>
> The application object must accept two positional arguments. For the sake of illustration, we have named them environ and start_response, but they are not required to have these names. A server or gateway must invoke the application object using positional (not keyword) arguments.
>
> The start_response parameter is a callable accepting two required positional arguments, and one optional argument. For the sake of illustration, we have named these arguments status, response_headers, and exc_info.
> The start_response callable must return a write(body_data) callable that takes one positional parameter: a bytestring to be written as part of the HTTP response body.
>
> When called by the server, the application object must return an iterable yielding zero or more bytestrings.
>
> The server or gateway must transmit the yielded bytestrings to the client in an unbuffered fashion, completing the transmission of each bytestring before requesting another one.

A good reading about this topic [Python WSGI](https://www.toptal.com/python/pythons-wsgi-server-application-interface)

## Two ways to let *argparse* accept a list as its arg

Use the nargs option or the 'append' setting of the action option

```python
parser.add_argument('-l','--list', nargs='+', help='<Required> Set flag', required=True)  # '+' means one or more, while '*' means 0 or more
parser.add_argument('-l','--list', action='append', help='<Required> Set flag', required=True)
```

## `os.path.splitext` is a helper function to split path to a tuple of dot extension and other part

A path like `'/tmp/a....pdf'` will be splitted into `('/tmp/a...', '.pdf')`

## Find a cool python command line interface called [click](https://click.palletsprojects.com/en/7.x/)

I think it is worth to study its source code, for its simple to use interface design, and its implementation.

## For Jupyter to support golang, there are two open source enabler:
	- [gophernotes](https://github.com/gopherdata/gophernotes): on mac, need to install zmq by `brew install zmq`
	- [lgo](https://github.com/yunabe/lgo), but currently does not natively run on mac

## For Jupyter to download notebook as pdf file, you need to:
	- Install `pandoc` and `tex` for mac. `tex` is very large. After install `mactex`, you need to restart terminal to include it to PATH.
	- Install `pip install pandoc`
	- Optionally, install [XeLaTex](http://www.texts.io/support/0001/), which seems to be a slim version of `mactex`

## Jupyter notebook viewer:
	- [nbviewer](https://nbviewer.jupyter.org/) is the official jupyter viewer that you can start on you local machine as a web server
	- [JupyterHub](https://jupyterhub.readthedocs.io/en/stable/) is the full fledged one
	- [nteract](https://nteract.io/desktop) is definitely a cool desktop tool for jupyter notebook, and it natively supports [node.js](https://nteract.io/kernels/node). Looks like nteract fail to find python kernel, I worked around this by directly running `jupyter run`. But to be honest, it is not as convenient as native web one.

## Jupyter: How to reset input prompt numbering?
> Select 'Kernel > Restart and clear Output' (restart the kernel) and then 'Cell > Run All' (run the script)

## Juypter: Enter command mode by pressing `Esc` to use shortcut keys for quick edit

## [Best practice with retries with requests](https://www.peterbe.com/plog/best-practice-with-retries-with-requests)

```python
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry


def requests_retry_session(
    retries=3,
    backoff_factor=0.3,
    status_forcelist=(500, 502, 504),
    session=None,
):
    session = session or requests.Session()
    retry = Retry(
        total=retries,
        read=retries,
        connect=retries,
        backoff_factor=backoff_factor,
        status_forcelist=status_forcelist,
    )
    adapter = HTTPAdapter(max_retries=retry)
    session.mount('http://', adapter)
    session.mount('https://', adapter)
    return session
```

BTW, `requests.Session()` can be used in place of usual `requests` with session headers to avoid duplicate code.

## Understand some confused concept: `int()`, `hex()`, `str.encode()`, `ascii`, `ord()`, and `chr()`
> `int()` can be used to parse a string to an integer value, and int python3, there is no limit for an interger, and it accept a second positional argument as integer base such as 16 to parse hex string like `0xa0`

> `hex()` accept an integer value to translate an integer to its hex string format. So `int(hex(random.randint(1, 2 << 100)),16)` will return a random number.

> `str.encode()` will encode a string to bytes according to encoding codec. And its reverse operation is `str.decode()`

> `ascii()` will return a string containing a printable representation of an object, but escape the non-ASCII characters.

> `ord()` will return a character's unicode value, and `chr` is its reverse operation.

## For python3, the default division operator `/` (also called true division) will do double number operation. For positie number, you can use *floor division* `//` to get floor int value. But beware that `-3//2` returns `-2` since that is the *floor* for `-1.5`.

## `yaml` module can be used for parsing and dumping yaml file
> Use `yaml.safe_load()` to load a yaml file stream, and `yaml.safe_dump()` to dump a yaml dict object to a string, and it provides flag `default_flow_style` to suppress nesting collections. But in order to dump customized objects such as namedtuple, you might need to specify its representer like the following one:

```python
	Node = namedtuple("Node", "index name uri path items")
    def reprent_node_yaml(dumper, data: Node):
        return dumper.represent_mapping(u'tag:yaml.org,2002:map', data._asdict())
    yaml.SafeDumper.add_multi_representer(Node, reprent_node_yaml)
```

But looks like there is no good and simple way to construct a python object from `yaml.safe_load()`. Maybe we should just use returned `dict`, and use static method to load individual objects.


## `random.sample()` function is useful to generate random sample among a collection: `random.sample(string.ascii_letters,16)`

## string trim method in python is called `strip()` and there are also `lstrip()` and `rstrip()`, try `string.whitespace.strip()` will return empty string.

## Get to know `string` built-in module
> It provides string consts such as `string.ascii_letters`,`string.digits`, `string.hexdigits`, as well string `Template` class

## [html scraping with lxml and requests](https://docs.python-guide.org/scenarios/scrape/)
> By the way, [BeatifulSoap](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) is kind of higher level language for html. To install it for python3, you should install bs4: `pip install bs4`, refer to [how to web scrape](https://towardsdatascience.com/how-to-web-scrape-with-python-in-4-minutes-bc49186a8460) for details.

**NOTE**: the above two methods won't be able to scape page when JavaScript provides or "hides" contenet. In this case, you need to resort to `selenium` to get page source like what you see. Refer to this artical from [freecodecamp](https://www.freecodecamp.org/news/better-web-scraping-in-python-with-selenium-beautiful-soup-and-pandas-d6390592e251/) for details.

## How to [distribute python project](https://packaging.python.org/guides/distributing-packages-using-setuptools/)
> Need to first install `pip install wheel` before running `python setup.py bdist_wheel`. And `setup.py` will read `setup.cfg` file for command options, so you can add options for `universal=1` to `[bdist_wheel]` section into that file to configure command running option for `bdist_wheel`. This also shows us a good example to extend `setup.py` to provide more commands. Maybe like [this](https://github.com/pypa/wheel/blob/master/setup.py):

```python
      entry_points={
          'console_scripts': [
              'wheel=wheel.cli:main'
              ],
          'distutils.commands': [
              'bdist_wheel=wheel.bdist_wheel:bdist_wheel'
              ]
          }
      )
```

For a single project, it might be a good idea to add [more custom build steps and commmand](https://jichu4n.com/posts/how-to-add-custom-build-steps-and-commands-to-setuppy/)

## Python regular expression [tutorial](https://www.guru99.com/python-regular-expressions-complete-tutorial.html)

One thing to remind for `findall` method is that, if you use grouping `()` in regular expression, it will output list of tuple of strings, or just list of strings. And make sure to ad `re.MULTILINE` flag to capture from multiple line text.

## [Using StringIO and BytesIO for managing data as file object](https://webkul.com/blog/using-io-for-creating-file-object/)
> `io.StringIO` requires a unicode string, while `io.BytesIO` requires a byte string.

## Understand `raise ValueError(‘Invalid inputs’) from e`
> The from clause is used for exception chaining: if given, the second expression must be another exception class or instance, which will then be attached to the raised exception as the __cause__ attribute (which is writable). If the raised exception is not handled, both exceptions will be printed

## In Python 3, `/` operator does floating point division for both int and float arguments.

```py
print (5/2) --> 2.5
```

## Python regular expression needs to escape `/` as well as other characters such as `\`, `.`, `*`, etc since they have special meanings

This expression will match repository org name as well as repo name: `github\.com\/(?P<org>.+)\/(?P<repo>.+)\.git`.
Please note that for named group, a `P` is after `?`, and in python code, that could be retrieved in this way: `re.search(r"github\.com\/(?P<org>.+)\/(?P<repo>.+)\.git", url).groupdict().get("repo")`
BTW, you can easily check re from this nice website called [regex101](https://regex101.com/)

## use `jsonpickle.encode(<obj<)` to dump any python object to json string.

Example: `open("/tmp/a.json", 'wt').write(jsonpickle.encode(repo))`

## `ConfigParser` from `configparser` package is very useful for parsing and merging `ini` type configuration file, as used in `backy2`

## Install `python3` and `pip` on centos machine: `yum install -y python36 python36-pip python36-devel`

## Max integer value

For python 2, this is `sys.maxint`, for python 3, int value is not bounded, but you can still rely on `sys.maxint` to get machine's word size.

## Use `os.urandom` to generate random bytes

> Return a string of n random bytes suitable for cryptographic use.On a UNIX-like system this will query /dev/urandom. 

```py
from hashlib import sha1
sha1(os.urandom(64)).hexdigest()
```

## Understand `set.difference_update(iterable, …)`

> Updates the set, removing elements found in others.

## Understand `itertools.chain(*iterables)`

> Make an iterator that returns elements from the first iterable until it is exhausted, then proceeds to the next iterable, until all of the iterables are exhausted. Used for treating consecutive sequences as a single sequence. `chain.from_iterable` is classmethod for the same purpose.

## Format string by its representation method like this `{x!r}.format(x=(123,234))`, for other format specifier, just use `:`, like this `{x:.2f}.format(100)`

## Understand `map`, `filter`, and `reduce` build-in functions

> map() function returns a list of the results after applying the given function to each item of a given iterable (list, tuple etc.), where iterable is a sequence, collection or an iterator object. You can send as many iterables as you like, just make sure the function has one parameter for each iterable.
The filter() function returns an iterator were the items are filtered through a function to test if the item is accepted or not.
In python3, reduce is moved to *functools* module, its syntax is `reduce(function, sequence[, initial]) -> value`

Sample code: 

```py
from functools import reduce
numbers1 = [1, 2, 3]
numbers2 = [4, 5, 6]
result = map(lambda x: x + x, numbers1)
result = map(lambda x, y: x + y, numbers1, numbers2)
result = filter(lambda x: x%2==0, numbers2)
result = reduce(lambda x,y: x+y, numbers1, 10)

```

NOTE: what returned from `map`, `filter` is iterable/generator. Tha's said, `map` is not like *foreach* function, in order for `map` to execute function, just use `list` to exhaust iterable.

## A little trick to check whether a var is set in environment vars

```py
    SENTINEL = object
    # Environment setting
    path_from_env = os.getenv("ANSIBLE_CONFIG", SENTINEL)
    if path_from_env is not SENTINEL:
	....
```

## Understand `vars()`, `locals()`, `globals()`

>The locals() function returns a dictionary containing the variables defined in the local namespace. Calling locals() in the global namespace is same as calling globals() and returns a dictionary representing the global namespace of the module.
>vars() returns either a dictionary of the current namespace (if called with no argument) or the dictionary of the argument.

It is possible to add more vars like this `vars()['x'] = 1`

## Understand [__slot__](http://book.pythontips.com/en/latest/__slots__magic.html)

Basically, use `__slot__` will save a lot of RAM, and also make attributes explicit.

```python
class MyClass(object):
    __slots__ = ['name', 'identifier']
    def __init__(self, name, identifier):
        self.name = name
        self.identifier = identifier
        self.set_up()
...
```


## *operator* package

It provides a lot of useful small functions or callable that could be easily implemented as lambda expression, such as `lt`, `itemgetter`, which can be used in places that require callable, such as `sort` method.

## [Understand `self.__class__.mro()`](https://stackoverflow.com/questions/2010692/what-does-mro-do)

>As long as we have single inheritance, __mro__ is just the tuple of: the class, its base, its base's base, and so on up to object
> in __mro__, no class is duplicated, and no class comes after its ancestors, save that classes that first enter at the same level of multiple inheritance.
>Every attribute you get on a class's instance, not just methods, is conceptually looked up along the __mro__, so, if more than one class among the ancestors defines that name, this tells you where the attribute will be found -- in the first class in the __mro__ that defines that name

## Understand `%012x`%100

The 12 here means alighment size, and the leading 0 means filling with 0 (default is whitespace)

## About *frozenset*

> Frozen set is just an immutable version of a Python set object. While elements of a set can be modified at any time, elements of frozen set remains the same after creation.
Due to this, frozen sets can be used as key in Dictionary or as element of another set. But like sets, it is not ordered (the elements can be set at any index).
The syntax of frozenset() method is: `frozenset([iterable])`

## Define `__eq__` method by `getattr` like this:

```py
    def __eq__(self, other):
        if not isinstance(other, HostState):
            return False

        for attr in ('_blocks', 'cur_block', 'cur_regular_task', 'cur_rescue_task', 'cur_always_task',
                     'run_state', 'fail_state', 'pending_setup', 'cur_dep_chain',
                     'tasks_child_state', 'rescue_child_state', 'always_child_state'):
            if getattr(self, attr) != getattr(other, attr):
                return False

        return True
```

## There is a default`__main__` module for each python program, we can test that to use its attributes like this:

```py
try:
    from __main__ import display
except ImportError:
    from ansible.utils.display import Display
    display = Display()
```

## Understand `from __future__ import (absolute_import, division, print_function)`

Those imports are to make python2 code like python3 code. `print_function` will bring `print` function to python2.

## Get parameter passed into *kwargs* since kwargs is indeed a dict

```py
class_only = kwargs.pop('class_only', False)
```

## pythonic way to check "interface" is *hasattr*:

```py
if (self._stdout_callback and
        hasattr(self._stdout_callback, 'set_play_context')):
    self._stdout_callback.set_play_context(play_context)
```

## python3 now supports unpack a list like this:

```py
name,_, sur = "Andy J Zhang".split()
```

## Use class flags for plugin pattern like this to support capabilities

```py
callback_type = getattr(callback_plugin, 'CALLBACK_TYPE', '')
callback_needs_whitelist = getattr(callback_plugin, 'CALLBACK_NEEDS_WHITELIST', False)
```

## To support *in* operator, just provides `__contains__` method like this

```py
    def has_plugin(self, name):
        ''' Checks if a plugin named name exists '''

        return self.find_plugin(name) is not None

    __contains__ = has_plugin

```

## Should learn more about pythnon _buildins_(builtins.py generated in pycharm, but actually implemented in c)

## About python regex special characters:

> (?:...)
A non-capturing version of regular parentheses. Matches whatever regular expression is inside the parentheses, but the substring matched by the group cannot be retrieved after performing a match or referenced later in the pattern.

## How to skip test cases with condition for pytest?

Use something like this for skip without condition, `@pytest.mark.skip(reason="no way of currently testing this")`, and `@pytest.mark.skipif('pandas' not in sys.modules,reason="requires the Pandas library")` to skip with condition check.

## `__new__` vs `__init__`

>Use __new__ when you need to control the creation of a new instance. Use __init__ when you need to control initialization of a new instance.
__new__ is the first step of instance creation. It's called first, and is responsible for returning a new instance of your class. In contrast, __init__ doesn't return anything; it's only responsible for initializing the instance after it's been created.
In general, you shouldn't need to override __new__ unless you're subclassing an immutable type like str, int, unicode or tuple.

## Use [cachetools](https://www.thepythoncorner.com/2018/04/how-to-make-your-code-faster-by-using-a-cache-in-python/?doing_wp_cron=1550651738.5658590793609619140625) for caching

```py
import time
import datetime

from cachetools import cached, TTLCache  # 1 - let's import the "cached" decorator and the "TTLCache" object from cachetools
cache = TTLCache(maxsize=100, ttl=300)  # 2 - let's create the cache object.

@cached(cache)  # 3 - it's time to decorate the method to use our cache system!
def get_candy_price(candy_id):
	....
	return (datetime.datetime.now().strftime("%c"), price)
```

## How to use *pandas* to group data?

The problem is to group different squence typing (ST) by its drug resistence (DR)

```py
import pandas as pd
st = pd.read_csv("st-dr.csv") # load data
stg = st.groupby("ST型别").sum() # sum to get value like this: RSRRRRR
res = stg.applymap(lambda x: int(x.count('R')/len(x)*10000)/100) # use applymap to calculate DR percent rate
res.to_csv('st-dr-g.csv') # output to csv file
res.T.to_csv('dr-st-g.csv') # output its rotation to csv file
```

## How to get unicode value for a character?

```py
s = "【歡樂頌1】"
s.encode("unicode_escaple")
```

## pytest: Fixture finalization / executing teardown code

>pytest supports execution of fixture specific finalization code when the fixture goes out of scope. By using a yield statement instead of return, all the code after the yield statement serves as the teardown code:

```py
# content of conftest.py

import smtplib
import pytest


@pytest.fixture(scope="module")
def smtp_connection():
    smtp_connection = smtplib.SMTP("smtp.gmail.com", 587, timeout=5)
    yield smtp_connection  # provide the fixture value
    print("teardown smtp")
    smtp_connection.close()

```

## Fix `error while loading shared libraries: libpython3.4m.so.rh-python34-1.0` when using *centos/python-34-centos7* base image
Add *LD_LIBRARY_PATH* like this to Dockerfile: `ENV LD_LIBRARY_PATH = $LD_LIBRARY_PATH:/opt/rh/rh-python34/root/usr/lib64/`

## Fix *ImportError: No module named blabla* even after installing it?
It might be fixed by setting *PYTHONPATH* like this `PYTHONPATH=$PYTHONPATH:/usr/lib64/python3.4/site-packages/`, especially for cpython module like this: `/usr/lib64/python3.4/site-packages/rados.cpython-34m.so`

 to fix wield pip install failure *AttributeError: _DistInfoDistribution__dep_map*: upgrade *pip* and *setuptools* by `pip install --upgrade pip setuptools`

## How to configure pycharm to format code when saving?

In Preference -> Other Settings -> Save Actions, change corresponding options.

## How to create test "category", which is call [marker](https://docs.pytest.org/en/latest/example/markers.html) in pytest?

## How to get nice iso format current time string since python3.6?
`datetime.datetime.now().astimezone().replace(microsecond=0).isoformat()`

## [f string since python3.6](https://realpython.com/python-f-strings/)

> The __str__() and __repr__() methods deal with how objects are presented as strings, so you’ll need to make sure you include at least one of those methods in your class definition. If you have to pick one, go with __repr__() because it can be used in place of __str__().
> Calling str() and repr() is preferable to using __str__() and __repr__() directly.By default, f-strings will use __str__(), but you can make sure they use __repr__() if you include the conversion flag !r

## [How to test async code](https://stackoverflow.com/questions/23033939/how-to-test-python-3-4-asyncio-code)

```py
class Test(unittest.TestCase):
    def setUp(self):
		# Note: use a new event loop to isolate tests
        self.loop = asyncio.new_event_loop()
        asyncio.set_event_loop(None)

    def test_xxx(self):
        @asyncio.coroutine
        def go():
            reader, writer = yield from asyncio.open_connection(
                '127.0.0.1', 8888, loop=self.loop)
            yield from asyncio.sleep(0.01, loop=self.loop)
        self.loop.run_until_complete(go())
```

## [celery configuration](http://docs.celeryproject.org/en/latest/userguide/configuration.html)

## How to call async function in *sync* way? (not recommended)

```py
@asyncio.coroutine
def async_gettter():
    return (yield from http_client.get('http://example.com'))

def sync_getter()
    return asyncio.get_event_loop().run_until_complete(async_getter())
```

## [pytest:How to set up classic xunit-style setup](https://docs.pytest.org/en/latest/xunit_setup.html)
Just add the following methods:

``` py
# Module level, where module parameter is optional
def setup_module(module):
    """ setup any state specific to the execution of the given module."""

def teardown_module(module):
    """ teardown any state that was previously setup with a setup_module
    method.
    """

# Class level
@classmethod
def setup_class(cls):
    """ setup any state specific to the execution of the given class (which
    usually contains tests).
    """

@classmethod
def teardown_class(cls):
    """ teardown any state that was previously setup with a call to
    setup_class.
    """

# method level, where the method parameter is optional
def setup_method(self, method):
    """ setup any state tied to the execution of the given method in a
    class.  setup_method is invoked for every test method of a class.
    """

def teardown_method(self, method):
    """ teardown any state that was previously setup with a setup_method
    call.
    """
```

## [About python mixin](https://easyaspython.com/mixins-for-fun-and-profit-cb9962760556)

> In Python, mixins are supported via multiple inheritance.
> Mixins take various forms depending on the language, but at the end of the day they encapsulate behavior that can be reused in other classes.

```py
import logging

class LoggerMixin(object):
    @property
    def logger(self):
        name = '.'.join([
            self.__module__,
            self.__class__.__name__
        ])
        return logging.getLogger(name)

class EssentialFunctioner(LoggerMixin, object):
    def do_the_thing(self):
        try:
            ...
        except BadThing:
            self.logger.error('OH NOES')

class BusinessLogicer(LoggerMixin, object):
    def __init__(self):
        super().__init__()
        self.logger.debug('Giving the logic the business...')
```

 on *pytest*
    - Enable logging output using option `-s` to stop capturing
    - Show verbose testing progress with option: `-v` or `-vv`

## [How to configure pyCharm to use autopep8?](https://github.com/hscgavin/autopep8-on-pycharm)

## [test with pytest and tox](https://tox.readthedocs.io/en/latest/example/pytest.html)
An example *tox.ini* file:

```ini
[tox]
envlist = py35,py36
[testenv]
changedir=tests
deps=pytest
commands= pytest  --basetemp={envtmpdir}  \ # pytest tempdir setting
                  {posargs}                 # substitute with tox' positional arguments
```

## [Testing Python Applications with Pytest](https://semaphoreci.com/community/tutorials/testing-python-applications-with-pytest)
> Fixture functions are created by marking them with the @pytest.fixture decorator. Test functions that require fixtures should accept them as arguments.

> Each test is provided with a newly-initialized Wallet instance, and not one that has been used in another test.

> It is a good practice to add docstrings for your fixtures. To see all the available fixtures, run the following command:`pytest --fixtures`

```py
@pytest.fixture
def empty_wallet():
    '''Returns a Wallet instance with a zero balance'''
    return Wallet()

@pytest.fixture
def wallet():
    '''Returns a Wallet instance with a balance of 20'''
    return Wallet(20)

def test_default_initial_amount(empty_wallet):
    assert empty_wallet.balance == 0

def test_setting_initial_amount(wallet):
    assert wallet.balance == 20
```

An example combining *fixture* and *parametrized* test functions:

```py
# test_wallet.py

@pytest.fixture
def my_wallet():
    '''Returns a Wallet instance with a zero balance'''
    return Wallet()

@pytest.mark.parametrize("earned,spent,expected", [
    (30, 10, 20),
    (20, 2, 18),
])
def test_transactions(my_wallet, earned, spent, expected):
    my_wallet.add_cash(earned)
    my_wallet.spend_cash(spent)
    assert my_wallet.balance == expected
```

Also refer to officail doc about [Basic parametrize](https://docs.pytest.org/en/latest/parametrize.html#parametrize-basics), and [more parametrize](https://docs.pytest.org/en/latest/example/parametrize.html#apply-indirect-on-particular-arguments); pytest also provides a way to accept input from pytest command line option like this:

```
def pytest_addoption(parser):
    parser.addoption("--stringinput", action="append", default=[],
        help="list of stringinputs to pass to test functions")

def pytest_generate_tests(metafunc):
    if 'stringinput' in metafunc.fixturenames:
        metafunc.parametrize("stringinput",
                             metafunc.config.getoption('stringinput'))
```

## python3 has build-in *virtualenv*, which can be called in the same way as *pip*: `python3 -m venv somepath`

## [Type hint in python3](https://docs.python.org/3/library/typing.html)

```py
from typing import List
Vector = List[float]

def scale(scalar: float, vector: Vector) -> Vector:
    return [scalar * num for num in vector]

    # typechecks; a list of floats qualifies as a Vector.
    new_vector = scale(2.0, [1.0, -4.2, 5.4])
```

## [Understand python Decorators](https://www.programiz.com/python-programming/decorator)
> A decorator takes in a function, adds some functionality and returns it. This is also called metaprogramming as a part of the program tries to modify another part of the program at compile time.

> We must be comfortable with the fact that, everything in Python (Yes! Even classes), are objects. Names that we define are simply identifiers bound to these objects. Functions are no exceptions, they are objects too (with attributes). Various different names can be bound to the same function object. Such function that take other functions as arguments are also called higher order functions. 

> Functions and methods are called callable as they can be called. In fact, any object which implements the special method __call__() is termed callable. So, in the most basic sense, a decorator is a callable that returns a callable. Basically, a decorator takes in a function, adds some functionality and returns it.

> We can see that the decorator function added some new functionality to the original function. This is similar to packing a gift. The decorator acts as a wrapper. The nature of the object that got decorated (actual gift inside) does not alter. But now, it looks pretty (since it got decorated).

```py
@make_pretty
def ordinary():
    print("I am ordinary")
```

is equivalent to

```py
def ordinary():
    print("I am ordinary")
ordinary = make_pretty(ordinary)
```

This is just a syntactic sugar to implement decorators.

A whole example:

```py
def smart_divide(func):
   def inner(a,b):
      print("I am going to divide",a,"and",b)
      if b == 0:
         print("Whoops! cannot divide")
         return

      return func(a,b)
   return inner

@smart_divide
def divide(a,b):
    return a/b

# in python, inner(*args, **kwargs) can match any function.
def works_for_all(func):
    def inner(*args, **kwargs):
        print("I can decorate any function")
        return func(*args, **kwargs)
    return inner
```

## How to generate random token?

```sh
python -c 'import secrets,base64; print(base64.b64encode(base64.b64encode(secrets.token_bytes(16))));
```

## [How to configure Flake8](http://flake8.pycqa.org/en/latest/user/configuration.html)

## [Understand python propety](https://www.programiz.com/python-programming/property)

```python
class Celsius:
    def __init__(self, temperature = 0):
        self._temperature = temperature

    def to_fahrenheit(self):
        return (self.temperature * 1.8) + 32

    @property
    def temperature(self):
        print("Getting value")
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        if value < -273:
            raise ValueError("Temperature below -273 is not possible")
        print("Setting value")
        self._temperature = value
```

## Install python module through library module *pip*: `python -m pip install SomePackage`

## How to pause for testing?
Check out [zmq topic example](https://github.com/zeromq/pyzmq/blob/master/examples/pubsub/topics_pub.py)

```py
    print("Hit Ctrl-C to stop broadcasting.")
    print("Waiting so subscriber sockets can connect...")
    print("")
    time.sleep(1.0)

    msg_counter = itertools.count()
    try:
        for topic in itertools.cycle(all_topics):
            msg_body = str(msg_counter.next())
            print('   Topic: %s, msg:%s' % (topic, msg_body))
            s.send_multipart([topic, msg_body])
            # short wait so we don't hog the cpu
            time.sleep(0.1)
    except KeyboardInterrupt:
```

## [How to upload a new pypi package?](https://packaging.python.org/tutorials/packaging-projects/)

```sh
# Install/upgrade setuptools and wheel
 pip install --upgrade setuptools wheel
 # Build distribution package
 python setup.py bdist_wheel
 # Install twine
pip install --upgrade twine
# Upload package to pypi
twine upload dist/*
```

## [How to configure Flake8](http://flake8.pycqa.org/en/latest/user/configuration.html)

## About mypy: An example for python3+, note that for *yield*, the type is *Iterator*:

```py
from typing import Iterator

def fib(n: int) -> Iterator[int]:
    a, b = 0, 1
    while a < n:
        yield a
        a, b = b, a + b
```

For python2.7, comment can serve the same purpose:

```py
def is_palindrome(s):
    # type: (str) -> bool
    return s == s[::-1]
```

*NOTE*: the third parameter in slicing means steps for each increment

## How to install python3 on centos?

Just run `yum install -y python36`, and then create a symbol link for *python3*: `sudo ln -s /usr/bin/python36 /usr/bin/python3`

## How to install pip for python3:

```sh
wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
rm get-pip.py
```

## How to support gui?

Use [ActivePython](https://www.activestate.com/). More [detail](http://docs.activestate.com/activepython/3.4/gettingstarted.html).

## [Create a local PyPI mirror](https://aboutsimon.com/blog/2012/02/24/Create-a-local-PyPI-mirror.html). __seems not work__
## [Create a local pypi by pip2pi](https://github.com/wolever/pip2pi)
  - Create a requirements.txt file for packages to collect
  - Run `pip2tgz <package path> -r requirements.txt` to download packages
  - Run `dir2pi <package path` to generate _simple_
  - Make sure package path is indexed by web server like _nginx_
## [Named group for regex match](https://docs.python.org/2/howto/regex.html)

>The syntax for a named group is one of the Python-specific extensions: (?P<name>...). name is, obviously, the name of the group. Named groups also behave exactly like capturing groups, and additionally associate a name with a group

## [Encoding and decoding in python](http://pythoncentral.io/python-unicode-encode-decode-strings-python-2x/)
  - You need to add to head _# -*- coding: utf-8 -*-_ if a python script contains unicode
  - Before you can pretty print or store some unicode string, try to encoding it into __utf_8__ first by `str.encode('utf_8')`

## [How to fix unresolved reference issue?](http://stackoverflow.com/questions/21236824/unresolved-reference-issue-in-pycharm)

>Manually adding it as you have done is indeed one way of doing this, but there is a simpler method, and that is by simply telling pycharm that you want to add the src folder as a source root, and then adding the sources root to your python path.

## How to reload a module? Just call build-in function `reload`, such as `reload(tmp)`

For `python >=3.4`, the syntax is:

```py
import importlib
importlib.reload(module)
```

## [vim-flake8 configuration](http://flake8.readthedocs.org/en/latest/config.html)

Need to configure _~/.config/flake8_ like the following:

```conf
[flake8]
ignore = E226,E302,E41
max-line-length = 160
exclude = tests/*
max-complexity = 10
```

## [How to format string with alignment][102]?
  - Left aligned: _{:<10}_
  - Right aligned: _{:10}_
  - Center aligned: _{:^10}_
  - Center aligned and padded by _-_: _{:-^10}_

## How to show help ?

> Just use build-in help(<var>) function, and it will print out doc string for that var.

## How to start a new virtualenv? Refer to [Virtual Environments][101] for detail.

```shell
pip install virutalenv
cd yourenvdir
virtualenv pname
source pname/bin/activate

# later when you want to exit from virtualenv
deactivate
```

## How to cleanly run ipython inside virutalenv?

Try to add a new alias:
`alias ipy="python -c 'import IPython; IPython.terminal.ipapp.launch_new_instance()'"`
After source virtualenv's activate file, run ipy

## My coding style

  - Prefer double quote(") than single quote

## Tutorial

## [sqlalchemy tutorial](http://www.rmunn.com/sqlalchemy-tutorial/tutorial.html)

## References

## [Python string format](https://mkaz.tech/python-string-format.html)
## [Write a tumblelog application with flask](https://docs.mongodb.org/ecosystem/tutorial/write-a-tumblelog-application-with-flask-mongoengine/)
## [Flask by Example - Project Setup](https://realpython.com/blog/python/flask-by-example-part-1-project-setup/)
## [Packaging and Distributing Projects](https://packaging.python.org/en/latest/distributing/#uploading-your-project-to-pypi)
## [pypi classifiers](https://pypi.python.org/pypi?%3Aaction=list_classifiers)
## [Getting started with wxPython](https://wiki.wxpython.org/Getting%20Started)
## [20 Python libraries you can’t live 20-python-libraries-you-cant-live-without](https://pythontips.com/2013/07/30/20-python-libraries-you-cant-live-without/)
## [Good logging practice in Python](https://fangpenlin.com/posts/2012/08/26/good-logging-practice-in-python/)

[Python built-in functions]: https://docs.python.org/3.6/library/functions.html
[Python special methods]: https://docs.python.org/3/reference/datamodel.html#special-method-names
[101]:http://docs.python-guide.org/en/latest/dev/virtualenvs/
[102]: https://mkaz.tech/python-string-format.html
