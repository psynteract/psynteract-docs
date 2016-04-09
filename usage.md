# Usage of Psynteract

After [installation](installation.md), the remaining task for any
experimenter using psynteract is to build the study. This is what we cover
in this part of the documentation.

## Basic Features

Four basic features make up the core of *psynteract*. Together, they provide the
building blocks for even complex interactive functionality. For clarity,
we have split these into separate pages:

* [Connect](usage-connect.md) <img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_connect/psynteract_connect_large.png" align="right"><br>
  Establish a connection between a client and the central server at the beginning of the experiment
* [Push](usage-push.md) <img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_push/psynteract_push_large.png" align="right"><br>
  Send data from a client to the server, making it available to the other connected clients
* [Get](usage-get.md) <img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_get/psynteract_get_large.png" align="right"><br>
  Retrieve data provided by the other clients
* [Wait](usage-wait.md) <img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_await/psynteract_await_large.png" align="right"><br>
  Pause an experiment until a specific condition is met on part of the other

## Additional Features

The additional features exist mainly to simplify complex experiments:

* [Reassign](usage-reassign.md) <img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_reassign/psynteract_reassign_large.png" align="right"><br>
  Regroup participants and assign new roles
* [Communicate](usage-communicate.md) (OpenSesame only) <img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_communicate/psynteract_communicate_large.png" align="right"><br>
  Combine the push, wait and get steps into one
