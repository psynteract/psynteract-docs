# Installation

Before psynteract can be used, it must be setup on both a server and every OpenSesame instance on which the experiment will be run.

We have tried to make the installation as easy as possible -- in a medium-sized lab with a dozen seats, deployment should not take longer than 45 minutes to an hour, and for a local test installation, you should be up and running within 20 minutes at most.

## CouchDB Server

The server, through which all communication during the experiment takes place, and which hosts the control panel for the experimenter.

The foundation underlying the server is an instance of [Apache CouchDB](https://couchdb.apache.org), a powerful database engine. It is open source, and thus freely available for all major platforms. We recommend that you install it on a computer within your lab's network; any typical desktop machine (for example, the lab's experimenter PC) with a stable network connection will do. (We recommend wired networks, though psynteract has been successfully tested via WiFi and there are no reasons why it should not work with wireless connections)

Thus, the first step is to [download and install](https://couchdb.apache.org/#download) a copy of CouchDB on your designated server. After the installation, please ascertain that the CouchDB instance is accessible across the network, for example by exempting it from restrictions imposed by the system's firewall.

[screenshot?]

Please also take note of your server's host name and IP address (Please consult the relevant documentation for [Windows](http://windows.microsoft.com/en-us/windows/find-computers-ip-address), [Mac OS](https://support.apple.com/kb/PH13790), or your preferred operating system). If your CouchDB has been setup correctly, you should be able to access it from all computers in your lab by opening the URL `http://[IP-or-hostname]:5984` in a browser. If you do so, you should then be greeted by a line of text similar to the following: `{"couchdb":"Welcome","uuid":"...","version":"[A.B.C]","vendor":[...]}`. If this is visible, congratulations! You have successfully completed the first and most difficult step of the installation.

One final step is still necessary, however: We need to create a database within CouchDB. To do so, please navigate to the URL you have opened above, but add `/_utils/`, for example `http://[IP-or-hostname]:5984/_utils/`. This will open the CouchDB Admin interface. From there, please *add a new database*, and give your new database a name (we suggest using `psynteract`, but you are free to choose).

## Client

### OpenSesame

If you are using OpenSesame to build and conduct your experiments, you will need to add the psynteract plugin and extensions to your local OpenSesame installation. In most cases, this is a simple drag-and-drop operation.

Whatever your operating system, please download the latest compressed [release version of psynteract](https://github.com/psynteract/psynteract-os/releases), and extract it to a convenient folder.

In each case, after copying the relevant files and restarting OpenSesame, you should see new psynteract elements in the item toolbar.

#### Windows

To install the plugin and extension, please navigate to your OpenSesame installation (<mark>which you will typically find at `c:\Programs\OpenSesame`</mark>), and copy <mark>...?</mark>

#### Mac OS

The OpenSesame program on Mac OS is encapsulated in an `.app` package. Please locate your OpenSesame installation in your `Applications` folder, right-click it and select "Show package contents". Within the OpenSesame package, please then navigate to `Contents/Resources/extensions` and <mark>copy...</mark>.

<mark>Check whether Opensesame can load plugins from ~/.opensesame/plugins on Mac OS</mark>

#### Other operating systems

Please consult the [OpenSesame documentation on installing plugins](http://osdoc.cogsci.nl/plug-ins/installation/) for further information.

### Pure Python

If you are using the psynteract library from python code, for example through PsychoPy, ExPyriment or a similar library, please make sure that the psynteract folder is on your `pythonpath` so that the library is available from Python.

The easiest way to ensure that the library is available is to import it manually in your experiment's code with the `site` package:

```python
import site
site.addsitedir('[path_to_folder_containing_psynteract]')
```

Please make sure that the directory path you specify at this point does not include the psynteract directory itself, but rather the folder containing the psynteract package (i.e. if you navigate to the path specified in the addsitedir command, you should see a directory named psynteract, which in turn contains a file named `__init__.py` as well as several directories. You should *not* see these files directly).

To validate your installation, see if you can run `from psynteract import Connection` either from within your code or from the python prompt.

## Psynteract server software

The final step is to install the psynteract software and control panel on the server. This only needs to be done once.

### OpenSesame

Completing the installation from OpenSesame is as simple as opening the extensions menu and selecting "Install control panel on server" from the psynteract entry. Enter the URL on which you found your CouchDB installation earlier, followed by the database name, for example `http://localhost:5984/psynteract/`. If the server is accessible over the network, this will complete the installation.

<mark>You will be shown the URL of your control panel once the installation is completed. Be sure to make a bookmark, or otherwise consult the [finding the control panel URL](#) from our FAQ.</mark>

### Pure Python

To complete the installation using Python alone, run the following commands from within your code or from the Python prompt:

```python
from psynteract import install_psynteract_server

install_psynteract_server(
    server_uri='http://[your-server-here]:5984',
    db_name='psynteract'
)
```

Again, you will be shown the URL of the control panel once the installation is complete.
