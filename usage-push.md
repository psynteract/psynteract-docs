# Push

<img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_push/psynteract_push_large.png" align="right">

Once a connection has been built, it can be used to share data between connected
clients. A **push** command transmits the local data to the server, where it can
be accessed from all other clients.

## OpenSesame

In OpenSesame, any data that is saved in[experimental variables]
(http://osdoc.cogsci.nl/usage/variables-and-conditional-statements/)
can be shared between clients. To indicate the points at which the data on the server
is updated, drag the *push* item into the experiment tree.

By default, all experimental variables are made available in this way. Optionally, 
it is possible to restrict the selection of variables transmitted to the server via
the lower panel of the push settings. To achieve this, enter the variable names
separated by commas in the *Custom variable selection* field.

## Python

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
