# verdaccio configuration example

This repo provides basic information on how to deploy [Verdaccio](https://verdaccio.org) in a controlled environment, behind nginx (which handles the TLS negotiation), with a single user and with the registration disabled. 

This configuration is based on several tutorials available on the Internet, several questions and answers on Stackoverflow and also the [official documentation of Verdaccio](https://verdaccio.org/docs/what-is-verdaccio/)

## Configuration & Installation

1. Download this repo
2. Create a `htpasswd` file with your desired username and password and save it in the `conf` directory replacing the existing `htpasswd` file.
3. Adjust the `conf/config.yaml` file with your desired configuration. It will work as is (it is the default configuration), you will only need to adjust the last line the `url_prefix: /npm/` line. This will signal Verdaccio that it will be served from https://some-domain.com/npm/. If your Verdaccio will be served from the canonical domain (https://some-domain.com) you will not need any `url_prefix` line in your configuration file.
4. Open the `docker-compose.yml` file and adjust the environment variable `VERDACCIO_PUBLIC_URL` to point to the canonical domain where your Verdaccio instance will be served. Adjust also the port where it will listen. By default it will listen on port 8080 and it will be directed to the Verdaccio's internal 4873 port. Run `docker-compose up -d` to start the service.
5. Open the `nginx.conf` file, adjust your domain name, the TLS private key and certificates' path and the port number where Verdaccio is listening. Enable this configuration file in your nginx server and restart it.


This configuration will create a fully private Verdaccio site, where all the downloads require authentication. **Even downloading public packages not hosted by Verdaccio but proxied from the public npm registry will require authentication**.

## Working with Verdaccio

To publish something in your registry, you will need to login with your username and password:

```bash
npm login --registry=https://yourserver.com/npm/
```

This step will save your credentials in the `~/.npmrc` file with a line like this:

```
//yourserver.com/npm/:_authToken=<REDACTED_TOKEN>
```

You can use the <REDACTED TOKEN> in a CI pipeline to automate the publishing of packages into your Verdaccio instance.


And then just publish your packages there:

```
npm publish --registry=https//yourserver.com/npm/
```


## Troubleshooting

Please check the [official documentation of Verdaccio](https://verdaccio.org/docs/what-is-verdaccio/) for more information.

If you have any questions, please contact us adding a new Issue on the [Github repository](https://github.com/codesyntax/verdaccio-configuration-example)

You will find [some background of our use of Verdaccio in our blog](https://www.codesyntax.com/en/blog/).

