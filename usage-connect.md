# Connect

<img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_connect/psynteract_connect_large.png" align="right">

Before any communication can occur, a client must establish a **connection** to
the central hub. If not instructed otherwise, it will connect to the latest open
session, and appear on the control panel. Because the connection is crucial, it
is the core of the library.

## OpenSesame

In OpenSesame, you can connect to the server by dragging the connection item
into your experiment. You'll probably want to place it toward the beginning of
the study, so that you can use the connection afterward. All other functions
depend on a connection having been made initially.

When the item is run, the client will wait for the session to be started via the
control panel before continuing with the experiment.

## Python

In pure Python, you build a connection by creating a connection object, as shown
in the following:  

```python
from psyinteract import Connection

## Establish a new connection to a local server,
## through the variable c which can be used later
c = Connection(
    server_uri='http://localhost:5984',
    db_name='psynteract',
    design='stranger', # Type of design
    group_size=2, # Number of clients in a group
    groupings_needed=1, # Number of distinct groupings needed
    roles=['dictator', 'recipient'], # Assign roles if required
    ghosts=False, # Optionally activate ghost mode
    client_name='my_pc', # Optional human-readable client identifier
    group='default', # Optional group
    initial_data={} # Optional initial data
)
```

## Common Options

### Obligatory
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

The __design__ option determines how the users are allocated to groups. Choosing
the *stranger* design allocates clients into groups at random, and a
reassignment of groups will again result in a random allocation. The *perfect
stranger* design also allocates clients at random initially, but thereafter
ensures that no two clients are in the same group again. Depending on the other
design parameters chosen, the perfect stranger design requires significantly
more participants overall.

The __group size__ defines the number of clients in a group.

### Optional

__Roles__ can be assigned to the members of a group, and will be distributed
to all clients.

__Offline mode__ enables you to test your experiment without connecting to an
actual server. This means that the client will never wait for others, and that
it will use dummy data in lieu of actual partners' responses. In OpenSesame,
the client's own data will be used as a stand-in for the data from connected
clients. In Python, it is possible to define dummy data that are substituted
during calls to `get` (see below).

__Ghost mode__ enables Psynteract to deal with numbers of participants that
cannot be split evenly across the group size specified. Using this mode, excess
participants become 'ghosts' and are assigned a properly allocated participant
to 'haunt'. That is, the 'ghosted' clients will receive all the input that this
participant experiences from his or her fellow group members, but the 'ghosts'
will not themselves affect the other participants -- the interaction is
unidirectional.

The number of __groupings needed__ represents the number of unique groupings
needed by the experiment (as an integer). If this argument is supplied, and the
number of groupings cannot be supplied (because there are to few participants to
combine into groups), a warning is shown on the control panel. By default,
only a single grouping will be generated.

## Python-only

The __client name__ is available to help you identify connected clients more
easily. Any value set here is visible on the backend in the "name" column. The
setting has no further technical relevance. In OpenSesame, this is automatically
generated from the user ID entered when starting the experiment.

__Initial data__ can be used to pass in any data that should be available in all
clients from the get-go. For example, if you would like to later append choices
to a list, you can create the list here. This is automatically taken care of in
OpenSesame.

## OpenSesame-only

__Identical random seeds__ can be applied to all clients to ensure that any
randomization is performed in synchrony. The random seed is calculated from
the session id.

OpenSesame can be instructed to __display a message while waiting for the
session to start__. If this is not set, any preceding screen will remain
visible.
