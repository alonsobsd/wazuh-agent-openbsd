# wazuh-agent-openbsd
Wazuh agent port for OpenBSD

## How can test it?

### Preparing environment

Install wazuh-agent dependencies from packages
```sh
pkg_add cmake gmake libinotify sqlite3 nghttp2
```
Extract ports tree to **/usr** directory
```sh
cd /usr/ports && ftp https://cdn.openbsd.org/pub/OpenBSD/snapshots/ports.tar.gz
```
```sh
cd /usr/ports && tar xvfz ports.tar.gz && rm ports.tar.gz
```
Copy **wazuh-agent** directory to **/usr/ports/security**

```sh
cd ~/ && ftp https://github.com/alonsobsd/wazuh-agent-openbsd/archive/refs/heads/main.tar.gz
```
```sh
cd ~/ && tar main.tar.gz && cp -rf ~/wazuh-agent-openbsd/wazuh-agent /usr/ports/security/
```

Edit **user.list** from ports tree

```sh
vi /usr/ports/infrastructure/db/user.list
```
Add the following entry to end of file

```sh
900 _wazuh		_wazuh		security/wazuh-agent
```
If an entry with id 900 exists, use other number (901, 902, etc). Don't forget change the user id into **/usr/ports/security/wazuh-agent/pkg/PLIST**

```sh
@newgroup _wazuh:900
@newuser _wazuh:900:_wazuh::Wazuh Owner:/var/ossec:/sbin/nologin
```
### Compiling and installing

```sh
cd /usr/ports/security/wazuh-agent && make install clean
```
