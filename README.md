Conkolla is a HTTP API Gateway (aka proxy) for interacting with the AppGate API. The name is a portmanteau for `Controller` and `kolla`, translated from a discovered rune stone in Gothenburg in Sweden meaning: "to look at the controller".

# History
While we, the Customer Reliability Team, have been working with various integration projects which involved AppGate API, we learned that a tool would be handy to rapid prototyping. Eventually "the tool" became more feature rich and we started to believe it is useful for anyone working with AppGate SDP integrations. 

From an educational standpoint, conkolla allows you to:

* Discover the API against a real system.
* Model or craft API requests and work with real responses.
* Let's you retrieve tokens when you have not yet covered the auth process in your script.
* Can be used as a API gateway connecting to many AppGate controllers at the same time.
* Gives you discovery with a Web based UI.
* Gives you all the scripting flexibility to use the http rest API (JSON).
* Administrate your AppGate.
* and many more....

The AppGate API can be learned and discovered in different ways. One of the common ways is to use the developer tools in a web browser while browsing through the AppGate admin UI, or use existing API Gateway or API Mocker such as postman or prism. 
However, with conkolla you get another option to choose from.


# Quick start
## Get conkolla
Conkolla (64bit) runs on macOS, Windows and linux. Download the binaries:
* [Conkolla latest release](https://github.com/Cyxtera/appgate-conkolla/releases/latest)

Or for docker:
```shell
docker pull mar8x/conkolla:latest
docker run -p 4433:4433 mar8x/conkolla:latest
```

## Run conkolla
By default conkolla serves on `https://localhost:4433`. Different serving options are available, check with `conkolla --h` for more information.
Keep the window in the background, it will reveal a good part of the interactions between client - conkolla - controller, and can be useful to follow the requests & responses, and payloads.

### Windows
Open a cmd line or powershell, then type `conkolla.exe`. 

### Linux & macOS
```shell
$ chmod +x conkolla
$ ./conkolla
```
### Docker
```shell
$ docker run -p 4433:4433 mar8x/conkolla:latest
```
*you will see a start message, something like this:*
```shell
bash-3.2$ ./conkolla
{
    "copyright": "Cyxtera Technologies  2019",
    "date": "2019-03-29",
    "directory": "/Users/tornrösa/execbin",
    "certPath": "templates/cert.pem",
    "keyPath": "templates/key.pem",
    "url": "https://localhost:4433",
    "tls": true,
    "mode": "release",
    "version": "4.4.5 (2d3c)",
    "defaultAPIVersion": 9,
    "operatingSystem": "darwin",
    "memoryUsageMBbase2": 68.68773651123047,
    "numberGoRoutines": 1,
    "systemTime": "04-09-2019 10:49:27",
    "systemTimeZone": "Local"
}
{
    "universal.impactor": [
        "127.0.0.1/8",
        "::1/128",
        "fe80::1/64",
        "fe80::c38:9c81:e8e3:b333/64",
        "172.28.100.8/16",
        "fe80::8808:cdff:fe91:9ed/64",
        "fe80::aede:48ff:fe00:1122/64",
        "fe80::f9b5:38ea:e3f0:2c2b/64",
        "fe80::bcaa:a14c:98a5:1e5f/64",
        "fe80::63de:2e74:7ed5:e7e8/64",
        "172.16.133.39/ffffff0f"
    ]
}
```

## Connect to a controller
At this stage you will need to have a user with admin permission and privileges to read or write AppGate objects. For more information read:
* [SDP Help > rest api](https://sdphelp.cyxtera.com/adminguide/v4.2/rest-apis.html)

Make sure you know where conkolla is serving:
* look for "url" in start message, or
* use the default `https://localhost:4433`

### Connect to a controller through web-based UI
* Open a browser and point to the url (look for "url" in start message) 
* Default is [https://localhost:4433/login/](https://localhost:4433/login/)
* Allow the exception for the self signed cert when using TLS (recommended)

Fill the form, use or follow the defaults if unsure.

![login form](/login.png)

### Connect with an application, example curl

```shell
controllerURL=cc1.packnot.com
password=secretsauce
apiVersion=9
label=dev

curl -s -H "Accept: application/json" -X POST \
     -kL https://localhost:4433/login --data \
     "{\"username\":\"admin\",\"password\":\"${password}\",\
      \"controllerURL\":\"${controllerURL}\",\"apiVersion\":${apiVersion},\
      \"skipVerifySSL\":\"true\",\"label\":\"${label}\"}"
```

Response example:
```json
 {
  "EntitlementTokenExpires": "2019-04-10T10:24:31.246Z",
  "authBody": {
    "machineId": "0e9076cd-e09e-4db7-6d24-480196700a42",
    "providerName": "local",
    "username": "admin",
    "password": ""
  },
  "connectionID": "1",
  "loginParameters": {
    "controllerURL": "cc1.packnot.com",
    "controllerPort": 444,
    "username": "admin",
    "password": "removed",
    "otp": "",
    "providerName": "local",
    "apiVersion": 9,
    "acceptHeaderSuffix": "+json",
    "contentTypeHeader": "application/json",
    "label": "dev",
    "machineId": "0e9076cd-e09e-4db7-6d24-480196700a42",
    "hideToken": "",
    "skipVerifySSL": "true",
    "dumpAGResponse": "",
    "autoTokenRenewal": ""
  },
  "message": "Successfully connected."
```

## Crafting REST calls via Conkolla
### Rest calls to an AppGate controller
Rest calls torwards an AppGate controller are in the form of:
* `https://{URL}:{PORT}/admin/{RESOURCE or PATH}`

where as:
* `URL` is the controllers URL
* `PORT`: the port of the admin access, default is `444`
* `RESOURCE or PATH`: the actual rest resource

Example: 
* `https://cc1.packnot.com:444/admin/stats/appliances`

### Rest calls via conkolla
In conkolla you reference a call to a connection, a connection which you previously logged into. Fro example for the same rest call as in the above example, you would use one of the following in the UI:
* `https://localhost:4433/get/1/stats/appliances`
* `https://localhost:4433/get/cc1.packnot.comdev/stats/appliances`

From the command line:
```shell
$ curl -k -H "Accept: application/json" https://localhost:4433/get/1/appliances
```

**Note: you need to drop the `/admin` part of the path.** Do not specify the `/admin` part in the path for the rest call. Conkolla adds it automatically on the upstream call.

Use also the [conkolla > help page](https://localhost:444/help.html) for your conkolla help.

# Conkolla internals
## Proxying scheme
Proxying API to a connected controller connection follows the scheme:
* `{HTTPS or HTTP}://{HOST}:{PORT}{HTTP METHOD}/{CONNECTION}/{UPSTREAM RESTCALL} :: {BODY}`

`HTTP METHOD`
* GET, POST, PUT, DELETE

`CONNECTION`: every connection has two references which can be used in conkolla proxying calls, either 
* a numeric `ID` assigned by conkolla at login time,  
* or the combined string of `{controllerURL}{label}`.

`UPSTREAM RESTCALL`
* AppGate API path/resource, without the `/admin` in the path.

`BODY`
* JSON encoded payload


## Conkolla API calls

### Globals

| Path        | Method           | Description  |
| ------------- |:-------------:| -----|
|`/settings`    | GET | Displays conkolla and runtime information.  |
|`/login` | POST GET | Login form, login JSON params, do login on a controller.|
|`/apispec` | GET | Displays the on-board apispec (might be outdated, use the linked from the menu for reference).|

### Connection specific

| Path        | Method           | Description  |
| ------------- |:-------------:| -----|
|`/agc/{connection}/conf`|GET POST| Display, change the connection settings for a controller.|
|`/agc/{connection}/headers`|GET|Download a file containing the headers for upstream calls for this connection |
|`/agc/{connection}/renewtoken`|GET| Renews the user and entitlement token for this connection. Only supported if no MFA is used and connection is set to 'Auto renew tokens' at login time.|


### Operations specific

| Path        | Method           | Description  |
| ------------- |:-------------:| -----|
|`/stats/{connection}/{label}`|GET|Retrieves stats from connected controller(s). Displaying single stats: `/stats/{connection}/`, where connection is a reference for the connection. Display stats of all connected controllers: `/stats/0/`. Display the stats for a certain label: `/stats/0/{label}`.|
|`/revoke/{connection}/{label}`|GET|Revokes the tokens for the conneciton. Revoking single connection: `/revoke/{connection}/`, where connection is a reference for the connection. Revoke all connected controllers: `/revoke/0/`. Revoke for all controllers under a certain label: `/revoke/0/{label}`.|
|`/forget/{connection}/{label}`|GET|Revokes and removes the connection. Revoking & remove single connection: `/revoke/{connection}/`, where connection is a reference for the connection. Revoke & remove all connected controllers: `/revoke/0/`. Revoke & remove for all controllers under a certain label: `/revoke/0/{label}`.|





