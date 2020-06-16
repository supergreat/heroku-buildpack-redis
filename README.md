# Heroku buildpack: stunnel

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
allows an application to use an [stunnel](http://stunnel.org) to connect securely to
SSL/TLS servers. It is based off the Heroku Redis buildpack and it is meant to be used in
conjunction with other buildpacks.

## Usage

Set this buildpack as your initial buildpack with:

```console
$ heroku buildpacks:add -i 1 supergreat/stunnel
```

Confirm that you are using this buildpack as well as your language buildpack like so:

```console
$ heroku buildpacks
=== frozen-potato-95352 Buildpack URLs
1. https://github.com/supergreat/heroku-buildpack-stunnel.git
2. heroku/python
```

Next, for each process that should connect to a TLS server securely, you will need to
preface the command in your `Procfile` with `bin/start-stunnel`. In this example, we want
the `web` process to use a secure connection to a TLS server:

```
$ cat Procfile
web: bin/start-stunnel python wsgi.py
```

We're then ready to deploy to Heroku with an encrypted connection between the dynos and our
TLS server:

```
$ git push heroku master
```

## Configuration

The buildpack will install and configure stunnel to connect to all URLs specified as a list
in the  `STUNNEL_URLS` config var:

```
$ heroku config:add STUNNEL_URLS="CACHE_URL SESSION_STORE_URL"
```

### Stunnel settings

Some settings are configurable through app config vars at runtime:

- ``STUNNEL_ENABLED``: Default to true, enable or disable stunnel.
- ``STUNNEL_LOGLEVEL``: Default is `notice`, set to `info` or `debug` for more verbose log output.

## Using the edge version of the buildpack

The `supergreat/stunnel` buildpack points to the latest stable version of the buildpack published in the [Buildpack Registry](https://devcenter.heroku.com/articles/buildpack-registry). To use the latest version of the buildpack (the code in this repository), run the following command:

```
$ heroku buildpacks:add https://github.com/supergreat/heroku-buildpack-stunnel
```
