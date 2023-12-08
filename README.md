# Admin Interface for Open Balena

Open source admin interface for [openbalena](https://github.com/balena-io/open-balena), a platform to deploy and manage connected devices.

## Features
The goal of this project is to provide the following areas of functionality to openbalena via a web interface:
- Support for multiple organizations and users in open-balena
- Remote access to devices (ssh, vnc and http into host or containers)
- Fleet / device management (create fleets, add devices, manage variables / tags / labels)
- Support for creating and managing custom device types
- Device management dashboard
- Remote device diagnostics

## Screenshots

Login Screen:
![Login screen](/assets/screenshots/login.png "Login Screen")

Dashboard:
![Dashboard](/assets/screenshots/dashboard.png "Dashboard")

Org Management:
![Org Management](/assets/screenshots/orgs.png "Org Management")

User Management:
![User Management](/assets/screenshots/user.png "User Management")

Fleet Management:
![Fleet Management](/assets/screenshots/fleets.png "Fleet Management")

Device Management:
![Device Management](/assets/screenshots/devices.png "Device Management")

Device Dashboard (Summary):
![Device Dashboard (Summary)](/assets/screenshots/device_dashboard_1.png "Device Dashboard (Summary)")

Device Dashboard (Logs):
![Device Dashboard (Logs)](/assets/screenshots/device_dashboard_2.png "Device Dashboard (Logs)")

Device Dashboard (Connect - SSH):
![Device Dashboard (Connect - SSH)](/assets/screenshots/device_dashboard_3.png "Device Dashboard (Connect - SSH)")

Device Dashboard (Connect - Web):
![Device Dashboard (Connect - Web)](/assets/screenshots/device_dashboard_4.png "Device Dashboard (Connect - Web)")

## Compatibility
This project is compatible with `open-balena-api` v0.139.0 or newer, all the way up to the current builds.  See [this project](https://github.com/dcaputo-harmoni/open-balena-helm) which has helm scripts to build a current version of `open-balena` including `open-balena-admin` and many other additional features.

## Installation (Docker Compose)

**Note**: Skip steps 1 and 2 if you have a running instance of openbalena

1. Download open-balena
```sh
git clone https://github.com/balena-io/open-balena.git
```

2. Configure open-balena
```sh
open-balena/scripts/quickstart -U balena@openbalena.local -P balena
```
**Note**: When the script is complete, take note of the values of `OPENBALENA_JWT_SECRET` from `config/activate`, and `OPENBALENA_API_VERSION_TAG` from `compose/versions`

3. Download open-balena-admin
```sh
git clone https://github.com/dcaputo-harmoni/open-balena-admin.git
```

4. Configure open-blena-admin
```sh
open-balena-admin/scripts/quickstart -j [OPENBALENA_JWT_SECRET] -v [OPENBALENA_API_VERSION_TAG]
```
**Note**: If you are running on a domain other than `openbalena.local`, be sure to also add `-d [DOMAIN]` to the quickstart script.  For a full list of quickstart configuration options, run `open-balena-admin/scripts/quickstart -h`.

If you are installing on the same host as open-balena, you can use these commands:
```sh
source open-balena/config/activate ; source open-balena/compose/versions
open-balena-admin/scripts/quickstart -j $OPENBALENA_JWT_SECRET \
	-v $OPENBALENA_API_VERSION_TAG -d $OPENBALENA_HOST_NAME
```

**Note**: If you did not complete steps 1 and 2 (i.e. you have a running instance of openbalena) you need to ssh into your running instance of open-balena-api, where you will find `OPENBALENA_JWT_SECRET` via the environment variable `JSON_WEB_TOKEN_SECRET`, and `OPENBALENA_API_VERSION` as "version" within `/usr/src/app/package.json`

5. Set up hostnames

If running locally, edit `/etc/hosts` or `C:\Windows\System32\Drivers\etc\hosts` to include:

```sh
127.0.0.1 openbalena.local
127.0.0.1 api.openbalena.local
127.0.0.1 registry.openbalena.local
127.0.0.1 vpn.openbalena.local
127.0.0.1 s3.openbalena.local
127.0.0.1 tunnel.openbalena.local
127.0.0.1 admin.openbalena.local
127.0.0.1 dashboard.openbalena.local
127.0.0.1 postgrest.openbalena.local
127.0.0.1 remote.openbalena.local
```
If hosted, set up your hostnames to point to the public IP addresses of your containers as follows:

- **<yourdomain.com>**: IP address / hostname of `open-balena-haproxy`
- **api.<yourdomain.com>**: IP address / hostname of `open-balena-haproxy`
- **registry.<yourdomain.com>**: IP address / hostname of `open-balena-haproxy`
- **vpn.<yourdomain.com>**: IP address / hostname of `open-balena-haproxy`
- **s3.<yourdomain.com>**: IP address / hostname of `open-balena-haproxy`
- **tunnel.<yourdomain.com>**: IP address / hostname of `open-balena-haproxy`
- **admin.<yourdomain.com>**: IP address / hostname of `open-balena-ui`, or `open-balena-admin-haproxy` if using K8S ingress
- **dashboard.<yourdomain.com>**:  IP address / hostname of `open-balena-ui`, or `open-balena-admin-haproxy` if using K8S ingress
- **postgrest.<yourdomain.com>**:  IP address / hostname of `open-balena-postgrest`, or `open-balena-admin-haproxy` if using K8S ingress
- **remote.<yourdomain.com>**:  IP address / hostname of `open-balena-remote`, or `open-balena-admin-haproxy` if using K8S ingress

6. Start open-balena
```sh
open-balena/scripts/compose up
```

7. Start open-balena-admin
```sh
open-balena-admin/scripts/compose up
```

## Web Access

Once both open-balena and open-balena-admin are running, you can access the admin interface via `http://admin.<openbalena domain>` (or `http://admin.openbalena.local` if running locally).  Log in using the credentials that were used in step 2 or when your openbalena instance was initially set up.  Device dashboards can be accessed directly at `http://dashboard.<openbalena domain>/devices/<UUID>/summary`, which will require a successful login. 

## Components

The open-balena-admin package is a compilation of three separate but related projects: [open-balena-ui](https://github.com/dcaputo-harmoni/open-balena-ui), [open-balena-remote](https://github.com/dcaputo-harmoni/open-balena-remote) and [open-balena-postgrest](https://github.com/dcaputo-harmoni/open-balena-postgrest).  Take note of the instructions within each of these projects to ensure your openbalena projects are configured to utilize features of open-balena-admin (i.e. remote device access).

## K8S Deployment

`open-balena-admin` has been fully integrated into the [open-balena-helm](https://github.com/dcaputo-harmoni/open-balena-helm) project.

## Limitations and Known Issues
- User authorization is not implemented at the resource level

## Credits

- The [ra-data-postgrest](https://github.com/raphiniert-com/ra-data-postgrest) project was instrumental in establishing the link to the open-balena database
