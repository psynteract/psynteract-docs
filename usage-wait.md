# Wait

<img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_wait/psynteract_wait_large.png" align="right">

Another vital component of an interactive experiment is temporal synchronization
-- all participants should be at the same stage throughout the experiment. From
an experimental point of view, this gives participants the certainty that the
interaction is actually taking place. From a technical point of view,
synchronization ensures that the state of all clients is the same, in particular
that all variables are set correctly so that they can be accessed from other
clients.

## OpenSesame

To synchronize clients running OpenSesame, include the *wait* item in your
experiment. It ensures that each client will pause when the item is reached, and
continue only if all other clients have also arrived at the same point in the
experiment.

While waiting for the other clients, the preceding screen will remain visible.
As it often is desired to display a simple text message while waiting, this
message can be specified directly in the item. The text will be presented in the center
of the screen. It can be formatted using the [HTML tags supported in OpenSesame]
(http://osdoc.cogsci.nl/usage/text/#formatting-html-subset).

As soon as the last client has arrived at the *wait* item, the experiment will continue.
This means that if a waiting message is displayed, this message will only be displayed 
for an extremely short time for the last client. Therefore, it is usually helpful
to specify an additional time that is waited after the last client has arrived.

## Python

A connection's **`wait`** method can be used to pause until an arbitrary
criterion is met on part of all connected clients.

### Parameters

* The **`condition`** is the first parameter to the method. It has to be a
function of a client's document, and should return a boolean value indicating
whether this client has passed the wait condition (`True`) or whether this client
should still be waited for (`False`). All documents are checked initially and
re-checked as soon as they change, to ensure that wait periods are no longer than
they need be.
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
c.wait(
    # The first parameter contains the condition function,
    # in this case a simple check whether the document contains
    # the variable 'status', and whether it is set to the
    # value 'running'.
    # Only if this condition is reached, will the execution
    # of the code continue from this point.
    lambda doc: doc['status'] == 'running',
    # Because the condition relates to the session (which is
    # managed from the control panel), specify that the check
    # is applied to it (rather than any connected clients).
    check='session'
)
```

* The **`check_function`** parameter determines how the results of the checks
outlined above are integrated. By default, this is the `all` function, meaning
that the condition function needs to evaluate to `True` for every document
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
