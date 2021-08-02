---
title: "How to Make a Traceback Less Verbose in Jupyter Notebooks"
date: 2021-08-02T14:51:04+02:00
tags:
- jupyter-notebook
- traceback
---

For a project I wanted to try out something new - using Jupyter Notebook to document its XML-RPC API,
so documentation and specification cannot drift apart.

This worked out great.

Except - as the API also has to handle invalid input, Jupyter Notebook shows - correctly - the traceback.

The traceback is super verbose.

### request

```python
server.get_licenses("not-existing-id")
```

### current print out in Jupyter Notebook

```python
---------------------------------------------------------------------------
Fault                                     Traceback (most recent call last)
<ipython-input-5-366cceb6869e> in <module>
----> 1 server.get_licenses("not-existing-id")

/usr/lib/python3.9/xmlrpc/client.py in __call__(self, *args)
   1114         return _Method(self.__send, "%s.%s" % (self.__name, name))
   1115     def __call__(self, *args):
-> 1116         return self.__send(self.__name, args)
   1117 
   1118 ##

/usr/lib/python3.9/xmlrpc/client.py in __request(self, methodname, params)
   1456                         allow_none=self.__allow_none).encode(self.__encoding, 'xmlcharrefreplace')
   1457 
-> 1458         response = self.__transport.request(
   1459             self.__host,
   1460             self.__handler,

/usr/lib/python3.9/xmlrpc/client.py in request(self, host, handler, request_body, verbose)
   1158         for i in (0, 1):
   1159             try:
-> 1160                 return self.single_request(host, handler, request_body, verbose)
   1161             except http.client.RemoteDisconnected:
   1162                 if i:

/usr/lib/python3.9/xmlrpc/client.py in single_request(self, host, handler, request_body, verbose)
   1174             if resp.status == 200:
   1175                 self.verbose = verbose
-> 1176                 return self.parse_response(resp)
   1177 
   1178         except Fault:

/usr/lib/python3.9/xmlrpc/client.py in parse_response(self, response)
   1346         p.close()
   1347 
-> 1348         return u.close()
   1349 
   1350 ##

/usr/lib/python3.9/xmlrpc/client.py in close(self)
    660             raise ResponseError()
    661         if self._type == "fault":
--> 662             raise Fault(**self._stack[0])
    663         return tuple(self._stack)
    664 

Fault: <Fault 1: 'company id is not valid'>
```

### output should look like this

```
Fault: <Fault 1: 'company id is not valid'>
```

## an almost perfect solution

The following solution,
using [sys.excepthook](https://docs.python.org/3/library/sys.html#sys.excepthook) works in a REPL...

### code

```python
import sys

def my_exc_handler(type, value, traceback):
    print(repr(value), file=sys.stderr)
    
sys.excepthook = my_exc_handler

1 / 0
```

### bash

```bash
❯ python3.9 main.py 
ZeroDivisionError('division by zero')
```

... but unfortunately not in **Jupyter Notebook** - I still get the full traceback.

When I have a look at Python's documentation...

> When an exception is raised and uncaught

... maybe the "uncaught" is the problem. When I have to guess, I think **Jupyter Notebook** catches all exceptions, and does the formatting and printing itself.


## the actual solution

This weekend, during the wonderful [PyOhio](https://www.pyohio.org/) conference,
I chatted with a couple of fellow participants in the channel for the
"9 Jupyter Notebook Tricks for Your Next Advent of Code" talk,
and boom... there was the solution, thanks to Kaspar (or David)*:

```bash
%xmode Minimal
```

This is a [builtin](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-xmode) of Jupyter Notebook. You just put it into a cell at the top of your notebook.


*) Yes - that is the actual user name :-)
