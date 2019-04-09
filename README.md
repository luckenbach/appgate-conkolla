Conkolla is a HTTP API Gateway (aka proxy) for interacting with the AppGate API. The name is a portmanteau for `Controller` and `kolla`, translated from a discovered rune stone in Gothenburg in Sweden meaning: "to look at the controller".

## Introcuction
The AppGate API can be learned and discovered in different ways. One of the common ways is to use the developer tools in a web browser while browsing through the AppGate admin UI or use existing tools such as postman or prism. 

However while the CRE team has been working with integrations and different use cases we continously build on a utility which eventually become the API Gateway, and we believe this can be useful for the community working woth AppGate outside the company. From an educational standpoint it allows you to:

* Discover the API against a real system.
* Allows you to model or craft API requests and proper responses.
* Let's you retrieve tokens when you have not yet covered the auth process in your script.
* Can be used as a API gateway connecting to many AppGate controllers at the same time.
* Gives you discovery with a Web based UI.
* Gives you all the scripting flexibility to use the http rest API (JSON).
* Simply administrate your AppGate through it.

## Quick start
### Get conkolla
Conkolla (64bit) runs on macOS, Windows and linux. Download the binaries from the latest release:
* [the latest release](https://github.com/Cyxtera/appgate-conkolla/releases/latest)

Or for docker:
```shell
docker pull mar8x/conkolla:latest
docker run -p 4433:4433 mar8x/conkolla:latest
```

### Run conkolla
By default conkolla serves on `https://localhos:4433`. Different serving options are available, check with `conkolla -h` for more information.

#### Windows
Open a cmd line or powershell, then type `conkolla.exe`. 

#### Linux & macOS
```shell
$ chmod +x conkolla
$ ./conkolla
```
#### Docker
```shell
$ docker run -p 4433:4433 mar8x/conkolla:latest
```
*Example output:*
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








