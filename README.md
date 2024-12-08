# wazuh-agent-openbsd
Wazuh agent port for OpenBSD

## How can test it?

Install wazuh-agent dependencies from packages
```sh
pkg_add cmake gmake libinotify sqlite3 nghttp2
```
Extract ports tree to **/usr** directory
```sh
cd /usr/ports && ftp https://cdn.openbsd.org/pub/OpenBSD/snapshots/ports.tar.gz
```
```sh
tar xvfz ports.tar.gz && rm ports.tar.gz
```
Extract wazuh-ports to **/usr/ports/security**

Edit **user.list** from ports tree

```sh
vi /usr/ports/infrastructure/db/user.list
```
Add the following entry to end of file. If an entry with id 900 exists, use an empty one (901, 902, etc)

```sh
900 _wazuh		_wazuh		security/wazuh-agent
```
