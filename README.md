# wazuh-agent-openbsd
This is a initial try to port Wazuh agent to OpenBSD. It needs testing because it could not work correctly yet. I hope it helps another users or developers interested on test wazuh on OpenBSD.

![image](https://github.com/user-attachments/assets/c36b7d10-aec8-4a33-bd5a-bac60e4cb6f6)

## How can test it?

### Pre-installation tasks

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
Finally, add inotify library path to shared library cache

```sh
vi /etc/rc.conf.local
```
```sh
shlib_dirs="/usr/local/lib/inotify"
```

### Compiling and installing

```sh
cd /usr/ports/security/wazuh-agent && make install clean
```
### Notes

If you want change default ports paths to test it from another ports directory, you can set the following:

```sh
vi /etc/mk.conf
```
```sh
PORTSDIR=/home/user/ports
WRKOBJDIR=/home/user/ports/obj/ports
DISTDIR=/home/user/ports/distfiles
PACKAGE_REPOSITORY=/home/user/ports/packages
```
**Wazuh agent version: 4.10.1**
