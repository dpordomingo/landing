# Contribution Guidelines

As all source{d} projects, this project follows the
[source{d} Contributing Guidelines](https://github.com/src-d/guide/blob/master/engineering/documents/CONTRIBUTING.md).


## Additional Contribution Guidelines

In addition to the [source{d} Contributing Guidelines](https://github.com/src-d/guide/blob/master/engineering/documents/CONTRIBUTING.md),
this project follows the guidelines described below.


# Architecture

The landing contains 2 parts:

- The [static site](#static-site) itself: a webpack app,
- a [Go API](#api) to serve the latest blog posts and job offers.

In production, each part lives in a separated container.


## Static site

The static site is built with:
- **webpack**: `javascript` (with some `REACT` components) and `css` (from `SASS` stylesheets)
- **hugo**: for the `HTML` from `Go` templates and `YAML` data

Its source code is under:
- [`hugo` directory](hugo): for `hugo` sources:
  - `content` directory: contains one `.md` per landing page that defines each page metadata,
  - `data` directory: contains the landing variable content like titles, captions, descriptions, projects...
  - `layout` directory: contains the `HTML` templates.
- [`src` directory](src): for `js` and `sass` code,
- [`static/img` directory](static/img): for site images,

## External Dependents

There are some external applications loading resources from http://sourced.tech/ so they should be preserved between different landing versions or considered as cross-project issues.

### Salesforce Pardot Landing Pages 

- [source{d} Engine Product Demo Sign-Up](http://go.sourced.tech/engine)
- [source{d} Lookout Product Demo Sign-Up](http://go.sourced.tech/lookout)
- [source{d} Contact Us](http://go.sourced.tech/contact)
	
#### Dependent Files

- `https://sourced.tech/css/bundle.css`
- `https://sourced.tech/js/bundle.js`
- `https://sourced.tech/img/home_separator_3.png`
- `https://sourced.tech/img/logo_footer.svg`
- `https://sourced.tech/img/logos/favicon.png`

#### Linked pages
- `https://sourced.tech/engine`
- `https://sourced.tech/lookout`
- `https://sourced.tech/open-source`
- `https://sourced.tech/community`
- `https://sourced.tech/company`

## API

The API serves (a cached version of):
- the 3 latest blog posts tagged as `technical`, and the 3 latest blog posts tagged as `culture` for the home page,
- all the opened positions at Lever,

Its source code is under [`api` directory](api).


# Development

## Requirements

You should already have [Go installed](https://golang.org/doc/install#install), and properly [configured the $GOPATH](https://github.com/golang/go/wiki/SettingGOPATH)
```shell
go version; # prints your go version
go env GOPATH; # prints your $GOPATH path
```

The project must be under the `$GOPATH`, following the Go import conventions, what means you can install and cd to its directory running:
```shell
go get github.com/src-d/landing/...
cd $GOPATH/src/github.com/src-d/landing
```

You also need [Yarn v1 installed](https://yarnpkg.com/en/docs/install)

```shell
yarn --version; # prints your Yarn version
```

## Install and build

You need to satisfy all [project requirements](#requirements), and then to run:

```shell
make build
```


## Development and running the landing locally

You need to satisfy all [project requirements](#requirements), and then to run:

```shell
LANDING_URL=//localhost PORT=8081 make serve
```
It runs everything you need to get the site working at [http://localhost:8081](http://localhost:8081)

Alternatively, you can start hugo, the api-server and webpack in a "three window mode", just running:
```shell
make project-dependencies
LANDING_URL=//localhost PORT=8081 yarn start
```
With this command, each window runs a command, that can be also ran by you in case you need to control the output of each command or in any other special case:
* `yarn run webpack-watcher` To start webpack watcher, that will rebuild the assets when you change its sources
* `make hugo-server` To serve the landing locally using hugo server
* `yarn run api-run` To start the landing API at [http://localhost:8080](http://localhost:8080)

### Preview the documentation site

Since the Landing repository is used as a blueprint for every documentation site as served by [src-d/docs](https://github.com/src-d/docs), the Landing can be used to generate a _"documentation like"_ site; To do so, it is needed to run:
```shell
make develop-documentation
```
And then, go to [http://localhost:8081](http://localhost:8081)

To rollback the changes, and see the landing as usual, just run:
```shell
make develop-documentation-destroy
```


## Configuration

The following envars are available for API configuration

envar | default *
- | -
`ADDR` | `:8080`
`FEED_BASE_URL` | `http://blog.sourced.tech/json/`
`POSITIONS_BASE_URL` | `https://api.lever.co/v0/postings/sourced?mode=json`

&ast; The default values are defined by [api/config/config.go](https://github.com/src-d/landing/blob/master/api/config/config.go)
