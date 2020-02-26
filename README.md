# WireGuard UI

[![Build Status](https://github.com/embarkstudios/wireguard-ui/workflows/Docker%20Image%20CI/badge.svg)](https://github.com/EmbarkStudios/wireguard-ui/actions)
[![Embark](https://img.shields.io/badge/embark-open%20source-blueviolet.svg)](https://github.com/EmbarkStudios)
[![Contributor Covenant](https://img.shields.io/badge/contributor%20covenant-v1.4%20adopted-ff69b4.svg)](CODE_OF_CONDUCT.md)

A basic, self-contained management service for [WireGuard](https://wireguard.com) with a self-serve web UI.

## Features

 * Self-serve and web based
 * QR-Code for convenient mobile client configuration
 * Optional multi-user support behind an authenticating proxy
 * Zero external dependencies - just a single binary using the wireguard kernel module
 * Container-first deployment

## Forked Codebase Features

 * Added automatic authentication if cookie wguserfront is set
 * Re-coded root relative UI for directory relative UI so UI would work on a reverse proxy subdirectory

![Screenshot](wireguard-ui.png)

## Running

The easiest way to run wireguard-ui is using the container image. To test it, run:

```docker run --rm -it --privileged --entrypoint "/wireguard-ui" -v /tmp/wireguard-ui:/data -p 8080:8080 -p 5555:5555 embarkstudios/wireguard-ui:latest --data-dir=/data --log-level=debug```

When running in production, we recommend using the latest release as opposed to `latest`.

## Developing

### Start frontend server
```
npm install --prefix=ui
npm run --prefix=ui dev
```

### Use frontend server when running the server

```
go get -u github.com/go-bindata/go-bindata/...
go get github.com/elazarl/go-bindata-assetfs/...
go-bindata-assetfs -prefix ui/dist ui/dist
go build .
sudo ./wireguard-ui --log-level=debug --dev-ui-server http://localhost:5000
```

## ARM Build

The code can be asily build for the ARM platform so it can, for example, run natively on a Raspberry PI.

```
make ui
go-bindata-assetfs -prefix ui/dist ui/dist
env GOOS=linux GOARCH=arm GOARM=5 go build .
```

### Running on ARM

As root copy the wireguard-ui ARM binary to /usr/local/bin and then run

```
/usr/local/bin/wireguard-ui --wg-dns="192.168.1.10" --wg-endpoint="my.domain.com:51820"
```

Where 192.168.1.10 is a local DNS server (for example when running PiHole) and my.domain.com is how the VPN can be accessed over the Internet.

You may want to add the --client-ip-range flag if there is a previously configured wireguard interface on the machine and you need to use those IPs instead.

### Apache2 Reverse Proxy

You can access the wg-ui interface via an apache2 reverse proxy by adding the following before the </Virtualhost> tag in /etc/apache2/sites-enabled/000-default.conf. Your configuration file may differ depending on the services enabled.

Install mod_proxy and restart apache. The following example is for Ubuntu.

```
sudo a2enmod proxy
sudo systemctl restart apache2
```

Add the following lines.

```
ProxyPass /wireguardui/ http://192.168.1.10:8080/
ProxyPassReverse /wireguardui/ http://192.168.1.10:8080/
```

Where /wireguardui/ is the relative URL to access the wg-ui interface on the apache server and http://192.168.1.10:8080/ is the address that the wg-ui can be accessed.
When complete restart the  web server.


## Contributing

We welcome community contributions to this project.

Please read our [Contributor Guide](CONTRIBUTING.md) for more information on how to get started.

## License
Licensed under either of

* Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
* MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FEmbarkStudios%2Fwireguard-ui.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FEmbarkStudios%2Fwireguard-ui?ref=badge_large)
