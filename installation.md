# Installation

Before psynteract can be used, it must be setup on every computer on which the experiment will be run (the clients), as well as a central server that coordinates the clients and allows them to communicate.

We have tried to make the installation as easy as possible -- in a medium-sized lab with a dozen seats, deployment should not take longer than 45 minutes to an hour, and for a local test installation, you should be up and running within 20 minutes at most.

The following documentation will walk you through the installation of the [Server](#Server), which is a computer that coordinates the activity of all participants, and each [Client](#Client), that is, each computer which is used by participants.

----

## CouchDB Server

The server is the computer through which all communication during the experiment takes place, and which hosts the control panel for the experimenter.

The foundation underlying the server is an instance of [Apache CouchDB](https://couchdb.apache.org), a powerful database engine. CouchDB is open source, and freely available for all major platforms. We recommend that you install it on a computer within your lab's network; any typical desktop machine with a stable network connection (for example, the lab's experimenter PC) will do. (We recommend wired networks, though psynteract has been successfully tested via WiFi and there are no reasons why it should not work with wireless connections.)

Thus, the first step is to [download and install](https://couchdb.apache.org/#download) a copy of CouchDB on your designated server. To do so, please download the appropriate package for your operating system, and follow the steps required by your operating system (i.e. start the installer on Windows; on Mac OS, download and copy the application into the appropriate folder; or install the CouchDB package supplied by your Linux distribution). You should then be able to start CouchDB as you would any other application.

When you start CouchDB for the first time, your operating system may inquire whether it should run CouchDB as a publicly accessible server. We recommend accepting this, given that your computer is connected to a closed, internal network (e.g. your laboratory network). Additional precautions should be taken if the computer running CouchDB is connected *directly* to the internet. If you are unsure about this step, please contact your local system administrator, who will be able to advise you in achieving a secure setup. CouchDB need not run continuously, but can be started and shut down as required.

After starting CouchDB, it is good practice to ascertain that you can access the database from the server itself. To do so, please navigate to `http://localhost:5984` in a browser. You should be greeted by a line of text similar to the following: `{"couchdb":"Welcome","uuid":"...","version":"[A.B.C]","vendor":[...]}`. If you see this, congratulations! You're more than half way there.

The next step is to make sure that the CouchDB instance is accessible across the network, from the machines on which the actual experiment will be run. To do so, please first take note of your server's host name and IP address (Please consult the relevant documentation for [Windows](http://windows.microsoft.com/en-us/windows/find-computers-ip-address), [Mac OS](https://support.apple.com/kb/PH13790), or your preferred operating system). If your CouchDB and system have been set up correctly, you should be able to access the database from all computers in your lab by opening the URL `http://[IP-or-hostname]:5984` in a browser. You should see the same message you encountered when you accessed CouchDB from a local browser. If so, congratulations! You have successfully completed the first and most difficult step of the installation.

[screenshot?]

One final step is still necessary, however: We need to create a database within CouchDB. To do so, please navigate to the URL you have opened above, but add `/_utils/`, for example `http://[IP-or-hostname]:5984/_utils/`. This will open the CouchDB Admin interface. From there, please *create a new database*, and give your new database a name (we suggest using `psynteract`, but you are free to choose).

## Client

The second part of the installation concerns the clients, that is, the computers on which the experiment itself will run. Here, a small piece of software needs to be installed alongside your experimental software, to enable it to communicate with the server you have just setup. This step will depend on the type of experimental software you use.

### OpenSesame

If you are using OpenSesame to build and conduct your experiments, you will need to add the psynteract plugin and extensions to your local OpenSesame installation. In most cases, this is a simple drag-and-drop operation.

Whatever your operating system, please download the latest compressed [release version of psynteract](https://github.com/psynteract/psynteract-os/releases), and extract it to a convenient folder.


#### Windows

To install the plugin and extension, please copy the contents of the folders `plugins` and `extensions` from the downloaded psynteract release into the corresponding folders that OpenSesame searches for plugins and extensions. One option for this is to use the `plugins` and `extensions` folders in the OpenSesame installation directory. Please see the [OpenSesame documentation](http://osdoc.cogsci.nl/plug-ins/installation/) for additional options.

#### Mac OS

On Mac OS, OpenSesame collects all of its plugins and extensions in the folder `.opensesame` in the user's home directory. Because the folder name starts with a dot, it is hidden from view. To access it, open the Finder, and, in the menu 'Go to', select the entry `Go to folder` (or alternatively, press ⌘ + shift + G). A small popup should appear in your Finder window. Here, enter `~/.opensesame` and confirm. The Finder should now show the contents of the previously hidden folder. Please copy the contents of the folders `plugins` and `extensions` from the downloaded psynteract package into the corresponding folders within this directory (or create them first, if necessary).

#### Other operating systems

Please consult the [OpenSesame documentation on installing plugins](http://osdoc.cogsci.nl/plug-ins/installation/) for further information.

In each case, after copying the relevant files and restarting OpenSesame, you should see new psynteract elements in the item toolbar.

### Pure Python

#### Via `pip`

The easiest way to install the psynteract python library system-wide is to install it via the [pip package installer](https://pip.pypa.io). To do so, please open a terminal window and enter `pip install [psynteract-package-url]`, where the url points to the [latest release](https://github.com/psynteract/psynteract-py/releases) of the `psynteract-py` package. The exact command with the latest URL is also available in the [readme file](https://github.com/psynteract/psynteract-py/blob/master/README.rst) of said package.

#### Per drag-and-drop

If you are using the psynteract library from python code, for example through PsychoPy, ExPyriment or a similar library, another simple way to include psynteract is to make sure that the psynteract folder is on your `pythonpath` so that the library is available from Python.

The easiest way to ensure that the library is available is to import it manually in your experiment's code with the `site` package:

```python
import site
site.addsitedir('[path_to_folder_containing_psynteract]')
```

Please make sure that the directory path you specify at this point does not include the psynteract directory itself, but rather the folder containing the psynteract package (i.e. if you navigate to the path specified in the `addsitedir` command, you should see a directory named psynteract, which in turn contains a file named `__init__.py` as well as several directories. You should *not* see these files directly).

#### Checking an installation

To validate your installation, see if you can run `from psynteract import Connection` either from within your code or from the python prompt.

## Psynteract server software

The final step is to install the psynteract software and control panel on the CouchDB database that you installed in the first step. This software governs the behavior of the database, and provides an interface for the experimenters to review and interact with their participants' data. Like the database itself, this software need only be installed once.

### OpenSesame

Completing the installation from OpenSesame is as simple as opening the extensions menu and selecting "Install control panel" from the psynteract entry. Enter the URL on which you found your CouchDB installation earlier, followed by the name of the database you setup, for example `http://localhost:5984/psynteract/`.
You will be shown the URL of your control panel once the installation is completed. Be sure to make a bookmark, or otherwise consult the [finding the control panel URL](faq.md) from our FAQ. If the server is accessible over the network, this will complete the installation.

### Pure Python

To complete the installation using Python alone, run the following commands from within your code or from the Python prompt:

```python
from psynteract import install_psynteract_server

install_psynteract_server(
    server_uri='http://[your-server-here]:5984',
    db_name='psynteract'
)
```

Again, you will be shown the URL of the control panel once the installation is complete -- please be sure to make a note.
