# scollector

## Linux

### Manual Installation

Download `scollector binary` for Linux.

```sh
mkdir scollector
```

```sh
cd scollector
```

```sh
wget https://github.com/bosun-monitor/bosun/releases/download/0.6.0-beta1/scollector-linux-amd64
```

```sh
chmod 700 scollector-linux-amd64
```

Replace username, password, hostname and port number with actual connection parameters.

```sh
echo 'Host = "http://username:password@atsd_hostname:8088/"' > scollector.toml
```

The default ATSD HTTP port is `8088`, HTTPS port is `8443`.

scollector does not support untrusted SSL certificates. If ATSD is running on a CA-signed SSL certificate, you can specify the secure connection.

```sh
echo 'Host = "https://username:password@atsd_hostname:8443/"' > scollector.toml
```

Start scollector.

```sh
nohup ./scollector-linux-amd64 &
```

### Auto-start in Privileged Mode

This section describes how to configure scollector start under a `sudo` user.

:::warning Note
`init` scripts for systems without `systemd` do not support daemon stopping and do not check if service is already running.
:::

#### Ubuntu 14.04

Change to the scollector installation directory.

Create `/etc/init.d/scollector` file:

```sh
sudo cat <<EOF > /etc/init.d/scollector
#chkconfig: 2345 90 10
#description: scollector is a framework to collect data points and store them in a TSDB.
### BEGIN INIT INFO
# Provides: scollector
# Required-Start:
# Required-Stop:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: start scollector
# Description:
### END INIT INFO

SCOLLECTOR_BIN=$(pwd)/scollector-linux-amd64
SCOLLECTOR_CONF=$(pwd)/scollector.toml

"\$SCOLLECTOR_BIN" -conf="\$SCOLLECTOR_CONF"
EOF
```

Make `/etc/init.d/scollector` file executable.

```sh
chmod a+x /etc/init.d/scollector
```

Enable scollector launch when the system is started.

```sh
sudo update-rc.d scollector defaults 90 10
```

Start scollector.

```sh
sudo service scollector start
```

#### CentOS 6.x and RHEL 6.x

Change to the scollector installation directory.

Create `/etc/init.d/scollector` file:

```sh
sudo cat <<EOF > /etc/init.d/scollector
#chkconfig: 2345 90 10
#description: scollector is a framework to collect data points and store them in a TSDB.
### BEGIN INIT INFO
# Provides: scollector
# Required-Start:
# Required-Stop:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: start scollector
# Description:
### END INIT INFO

SCOLLECTOR_BIN=$(pwd)/scollector-linux-amd64
SCOLLECTOR_CONF=$(pwd)/scollector.toml

"\$SCOLLECTOR_BIN" -conf="\$SCOLLECTOR_CONF"
EOF
```

Make the `/etc/init.d/scollector` file executable.

```sh
chmod a+x /etc/init.d/scollector
```

Enable scollector launch when the system is started.

```sh
sudo chkconfig --add scollector
```

#### Ubuntu 16.04, CentOS 7.x and RHEL 7.x

Change to the scollector installation directory.

Create a service file for scollector `/lib/systemd/system/scollector.service`:

```bash
sudo cat <<EOF > /lib/systemd/system/scollector.service
[Unit]
Description=scollector daemon
After=network.target

[Service]
Type=simple
ExecStart=$(pwd)/scollector-linux-amd64

[Install]
WantedBy=multi-user.target
EOF
```

Enable scollector launch when the system is started.

```sh
sudo systemctl enable scollector
```

### Auto-start scollector as a non-sudo user

#### Ubuntu 14.04, CentOS 6.x and RHEL 6.x

Modify the `/etc/init.d/tcollector` file:

```bash
#chkconfig: 2345 90 10
#description: collect OS metrics and store them in ATSD
### BEGIN INIT INFO
# Provides: scollector
# Required-Start:
# Required-Stop:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: start scollector
# Description:
### END INIT INFO

SCOLLECTOR_BIN=/home/axibase/scollector-linux-amd64
SCOLLECTOR_CONF=/home/axibase/scollector.toml
SCOLLECTOR_USER=axibase

if [ `whoami` != "$SCOLLECTOR_USER" ]; then
su - "$SCOLLECTOR_USER" -c "$SCOLLECTOR_BIN" -conf="$SCOLLECTOR_CONF"
else
"$SCOLLECTOR_BIN" -conf="$SCOLLECTOR_CONF"
fi
```

Change `SCOLLECTOR_BIN` and `SCOLLECTOR_CONF` to the actual scollector directory path.
Set `SCOLLECTOR_USER` to the user under which scollector is launched.

#### Ubuntu 16.04, CentOS 7.x and RHEL 7.x

Add `User` option to `[Service]` section of the service file

```sh
sudo sed -i '/\[Service\]/a User=[user_name]' /lib/systemd/system/scollector.service
sudo systemctl daemon-reload
```

## Windows

Download [`scollector executable`](http://bosun.org/scollector/) for Windows.

Change to the directory with the `exe` file and create a `scollector.toml` file in notepad.

> Ensure the name of the file is `scollector.toml` and not `scollector.toml.txt`

Add `Host` setting to `scollector.toml`:

```toml
Host = "http://username:password@atsd_hostname:8088/"
```

scollector does not support untrusted SSL certificates. If you installed a CA-signed SSL certificate into ATSD, you can change the above setting to connect to the secure HTTPS endpoint.

```toml
Host = "https://username:password@atsd_hostname:8443/"
```

Open the prompt as Administrator and create an scollector service with automated startup:

```bash
scollector-windows-amd64.exe -winsvc=install
```

Start scollector service:

```bash
scollector-windows-amd64.exe -winsvc=start
```

If the service exits a few seconds after startup, check the following:

* `scollector.toml` file does not exist in the same directory.
* `scollector.toml` file is not valid or is empty.
* `Host` parameter value is specified without double quotes.

Open Windows event log and review the scollector service startup error.

If the service is running but there are no `scollector` metrics in ATSD, verify the protocol, URL, and user credentials specified in the `scollector.toml` file.