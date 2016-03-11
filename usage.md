# Usage of Psynteract

After [installation](../installation.md), the remaining task for any
experimenter using psynteract is to build the study

## Basic Features

Four basic features make up the core of *psynteract*. Together, they provide the
building blocks for even complex interactive functionality.

### Connect

Before any communication can occur, a client must establish a **connection** to
the central hub. If not instructed otherwise, it will connect to the latest open
session, and appear on the control panel. Because the connection is crucial, it
is the core of the library.

#### OpenSesame

<img src="../OpenSesame plugin/plugins/niente_connect/niente_connect_large.png"
style="float: right"> In OpenSesame, you can connect to the server by dragging
the connection item into your experiment. You'll probably want to place it
toward the beginning of the study, so that you can access it afterward. All
other functions depend on a connection having been made initially.

When the item is run, the client will wait for the session to be started via the
control panel before continuing with the experiment. Any preceding screen will
remain visible

#### Python

In pure Python, you build a connection by creating a connection object, as shown
in the following:  

```python
from psyinteract import Connection

## Establish a new connection to a local server,
## through the variable c which can be used later
c = Connection(
    server_uri='http://localhost:5984',
    db_name='psynteract',
    client_name='my_pc', # Optional human-readable client identifier
    group='default', # Optional group
    initial_data={} # Optional initial data
)
```

#### Common Options

##### Obligatory
The __Server URI__ is the most vital option: It specifies the electronic address
of the server. You can think of it as a regular URL, such as
`http://google.com`. It will always begin with `http://` (or, in the case of
encrypted connections, with `https://`). This is followed by either a host name
or an IP address. If your CouchDB instance is running on your computer, use
`localhost`. If it's running on a different machine in your local network,
you'll probably want to insert its _IP address_ (which will look like
`192.168.0.2`). If the CouchDB instance is running somewhere else entirely,
you'll probably need a _host name_ such as `couch.my-institution.com`. If you
haven't changed its settings, a CouchDB instance will be available on port
`5984`. Thus, your total URI string will probably look something like
`http://my_server.com:5984`.

The __database name__ is also central. Because a server with CouchDB can
potentially store many separate databases, the client needs to know which one to
use. You will have set this up during the installation.

##### Optional

The __client name__ is available to help you identify connected clients more
easily. Any value set here is visible on the backend in the "name" column. The
setting has no further technical relevance.

__Initial data__ can be used to pass in any data that should be available in all
clients from the get-go. For example, if you would like to later append choices
to a list, you can create the list here. This is automatically taken care of in
OpenSesame.

__Offline mode__ enables you to test your experiment without connecting to an
actual server. This means that the client will never wait for others, and that
it will use dummy data in lieu of actual partners' responses.

The number of __groupings needed__ represents the number of unique groupings
needed by the experiment (as an integer). If this argument is supplied, and the
number of groupings cannot be supplied (because there are to few participants to
combine into groups), a warning is shown on the control panel.

### Push

Once a connection has been built, it can be used to share data between connected
clients. A **push** command transmits the local data to the server, where it can
be accessed from all other clients.

#### OpenSesame

<img src="../OpenSesame plugin/plugins/niente_push/niente_push_large.png"
style="float: right"> In OpenSesame, any data that is saved in local variables
can be shared between clients. To indicate the points at which the data on the
server is updated, drag the *push* item into the experiment tree.

By default, all local variables are made available in this way. Optionally, it
is possible to restrict the selection of variables transmitted to the server via
the lower panel of the push settings. To achieve this, enter the variable names
separated by commas.

#### Python

In pure Python, the data to be shared between clients must be collected
explicitly. This is done via the connection's `doc` attribute, which provides a
dictionary that can be filled with arbitrary data. When the connection's `push`
method is called, this document is transmitted to the server.

By default, the `doc` attribute contains several pieces of metadata that are
relevant only for technical reasons and should generally not be altered. It is
thus best practice to put data to be shared in the `data` attribute of the
`doc`. This is also where the initial data described above is placed. After the
connection has been established, the data can also be accessed via the `data`
attribute on the connection.

### Get

The counterpart of the push command is **get**, which downloads data from the
server for local use.

#### OpenSesame

<img src="../OpenSesame plugin/plugins/niente_get/niente_get_large.png"
style="float: right">

As for *push*, the *get* item in OpenSesame operates on local variables. In
particular, it will automatically fetch data previously pushed to the server by
the local participant's partners, and make it available as local variables. For
example, if another player makes a response which is collected in the `response`
variable in her OpenSesame instance and *pushed* to the server, once her
experimental partner *gets* the relevant data, it will be locally available in
the `partner01_response` variable.

#### Python

In Python, the mechanism operates slightly differently, in that the partners are
not automatically loaded. Instead, a connection's *get* method takes a document
`_id` as a parameter, and can thus, in principle, retrieve any document saved on
the server. This makes it very powerful, but also requires some additional code,
as demonstrated in the following snippet, which gathers all responses from the
other players in the same group as the client:

```python
def get_response(id):
  data = connection.get(id).data
  return data['response']

responses = map(get_response, connection.current_partners)
```

### Await

Another vital component of an interactive experiment is temporal synchronization
-- all participants should be at the same stage throughout the experiment. From
an experimental point of view, this gives participants the certainty that the
interaction is actually taking place. From a technical point of view,
synchronization ensures that the state of all clients is the same, in particular
that all variables are set correctly so that they can be accessed from other
clients.

#### OpenSesame

<img src="../OpenSesame plugin/plugins/niente_await/niente_await_large.png"
style="float: right">

To synchronize clients running OpenSesame, include the `await` item in your
experiment. It ensures that each client will pause when the item is reached, and
continue only if all other clients have also arrived at the same point in the
experiment.

#### Python

A connection's **`await`** method can be used to pause until an arbitrary
criterion is met on part of all connected clients.

##### Parameters

* The **`condition`** is the first parameter to the method. It has to be a
function of a client's document, and should return a boolean value depending on
whether or not this client should still be waited for. All documents are checked
initially and re-checked as soon as they change, to ensure that wait periods are
no longer than they need be.
* Although the **`check`** is typically run on all other clients, in certain
situations the condition specified above should be applied to, for example, only
`partners` or the `session` itself. Thus, the check parameter can take any of
the three values `clients`, `partners` or `session` depending on which data the
function should be applied to. For example, at the beginning of an experiment,
you will often want to wait until the session has been started from the control
panel:

```python
## We assume that the connection has been set up previously,
## as shown above...

## The experiment begins:
c.await(
    # The first parameter contains the condition function,
    # in this case a simple check whether the document contains
    # the variable 'status', and whether it is set to the
    # value 'running'.
    # Only if this condition is reached, will the execution
    # of the code continue from this point.
    lambda doc: doc['status'] == 'running',
    # Because the condition applied to the session (which is
    # managed from the control panel), specify that the check
    # is applied to it (rather than any connected clients).
    check='session'
)
```

* The **`check_function`** parameter determines how the results of the checks
outlined above are integrated. By default, this is the `all` function, meaning
that the condition function needs to evaluate to `true` for every document
inspected. However, one might, for example, want to stop waiting already when a
certain percentage (or all but one) of the clients has reached the condition. In
such a case, this argument can be passed any function that receives an array of
boolean values (the results of the condition function applied to the documents
that are checked), and returns a single boolean.
* The **`timeout`** parameter specifies a duration (in milliseconds) after which
a new http request is used for communication to the database. By default, it is
set to `None`, meaning that the same request is used continuously. If an integer
value is passed, a new connection will be used after this number of
milliseconds. This is worth trying if there are persistent network connection
issues.
* The **`heartbeat`** parameter specifies the interval (in milliseconds) in
which the database sends 'heartbeat' signals to the client while the latter is
waiting. By default, this occurs once a second. Again, decreasing the interval
(making the heartbeat signals more frequent) can be helpful if the connection is
lost or terminated prematurely. Because updated information sent from the
database is also printed on the debug console, irregular heartbeats or lost
packages can be used to diagnose database or connection issues. 
