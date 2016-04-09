# Get

<img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_get/psynteract_get_large.png" align="right">

The counterpart of the push command is **get**, which downloads data from the
server for local use.

## OpenSesame

As for *push*, the *get* item in OpenSesame operates on [experimental variables]
(http://osdoc.cogsci.nl/usage/variables-and-conditional-statements/). In
particular, it will automatically fetch data previously pushed to the server by
the local participant's partners, and make it available as experimental variables
adding the respective partner prefix (e.g. `partner01_`)  to the variable name.

For example, if another player makes a response which is collected in the
`response` variable in her OpenSesame instance and *pushed* to the server, once
her experimental partner *gets* the relevant data, it will be locally available in
the `partner01_response` variable.

If the *Offline test mode* has been checked in the [connect](usage-connect.md) item,
the client's own variables will be used as partner variables (if the respective option
is selected).

## Python

In Python, the mechanism operates slightly differently, in that the partners are
not automatically loaded. Instead, a connection's *get* method takes a document
`_id` as a parameter, and can thus, in principle, retrieve any document saved on
the server. This makes it very powerful, but also requires some additional code
to gather the document ids that are of interest.

The combination of both is demonstrated in the following snippet, which gathers
all responses from the other players in the same group as the client:

```python
def get_response(id):
  data = connection.get(id).data
  return data['response']

responses = map(get_response, connection.current_partners)
```
