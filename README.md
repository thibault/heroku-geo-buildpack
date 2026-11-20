Heroku Buildpack: Geo
=====================

Heroku Buildpack Geo is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
installs the Geo/GIS libraries [GDAL](https://gdal.org/), [GEOS](https://libgeos.org/) and [PROJ](https://proj.org/)

It can be used to get [GeoDjango](https://docs.djangoproject.com/en/stable/ref/contrib/gis/) or [RGeo](https://github.com/rgeo/rgeo) running on Heroku.

Usage
-----

This buildpack is designed to be used in combination with other buildpacks by using Heroku's [multiple buildpack support](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app).

Ensure that Heroku Buildpack Geo is the first buildpack on your list of buildpacks:

```
$ heroku buildpacks
=== Buildpack URLs
1. https://github.com/heroku/heroku-geo-buildpack.git
2. heroku/python
```

Default Versions
----------------

The buildpack will install the following versions by default *for new apps*:

- GDAL: `3.12.3`
- GEOS: `3.14.1`
- PROJ: `9.8.1`

Note: *Existing apps* that don't specify an explicit version will continue to use the
version used by the last successful build (unless the
[build cache is cleared](https://help.heroku.com/18PI5RSY/how-do-i-clear-the-build-cache)).

You can change the version of each library that will be installed by setting the
`GDAL_VERSION`, `GEOS_VERSION` or `PROJ_VERSION` config variables.

To install versions built elsewhere, set the `GEO_ARCHIVES_URL` config variable to the
address of a host that serves the archives as `<url>/<stack>/<LIBRARY>/<LIBRARY>-<version>.tar.gz`,
for example `<url>/heroku-24/GDAL/GDAL-3.13.3.tar.gz`. Scalingo stacks download the archives
of the Heroku stack with the same number.

To build an archive, for example GDAL for heroku-24:

```
$ make buildenv STACK_VERSION=24
$ docker run --rm -it -v "${PWD}/upload:/tmp/upload" geo-buildenv-24 ./gdal.sh X.Y.Z
```

The archive lands in `upload/heroku-24/GDAL/GDAL-X.Y.Z.tar.gz`. To serve it from this
repository, attach it to the release tagged `heroku-24/GDAL`.

Available Versions
------------------

- GDAL:
  - `3.9.3`
  - `3.10.3`
  - `3.11.5`
  - `3.12.3`
- GEOS:
  - `3.10.7`
  - `3.11.5`
  - `3.12.3`
  - `3.13.1`
  - `3.14.1`
- PROJ:
  - `9.7.1`
  - `9.8.1`

Migrating from heroku/python GEO support
----------------------------------------

If you were previously using the undocumented `BUILD_WITH_GEO_LIBRARIES` functionality of the official [Heroku Python Buildpack](https://github.com/heroku/heroku-buildpack-python) here are instructions for changing to this buildpack:

1. You have to completely remove the `BUILD_WITH_GEO_LIBRARIES` config variable like so - `heroku config:unset BUILD_WITH_GEO_LIBRARIES`
2. You should consider flushing your applications build cache by following these instructions - https://help.heroku.com/18PI5RSY/how-do-i-clear-the-build-cache
