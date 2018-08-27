[![Travis](https://img.shields.io/travis/contentascode/docsmith.svg)](https://travis-ci.org/contentascode/docsmith) [![npm](https://img.shields.io/npm/dt/docsmith.svg)](https://www.npmjs.com/package/docsmith) [![Code Climate](https://img.shields.io/codeclimate/github/contentascode/docsmith.svg)](https://codeclimate.com/github/contentascode/docsmith) [![Code Climate](https://img.shields.io/codeclimate/coverage/github/contentascode/docsmith.svg)](https://codeclimate.com/github/contentascode/docsmith/coverage) [![GitHub license](https://img.shields.io/github/license/contentascode/docsmith.svg)](https://github.com/contentascode/docsmith/blob/master/LICENSE)

# This is in the very early stages of development!

# docsmith (part of [content as code](http://iilab.github.io/contentascode))

Content as Code aims to make it easy to start a website in just a few steps but also build complex content publishing pipelines. It develops best practices to managing content workflows inspired from how code is managed in large collaborative software engineering projects.

**docsmith** implements the [Content as Code](https://contentascode.io) framework using metal*smith*, pan*doc* and *doc*ker microservice deployment.

<!-- MarkdownTOC -->

- [Getting Started](#getting-started)
  - [Use your own domain name](#use-your-own-domain-name)
  - [Collaboration](#collaboration)
  - [Checking Links](#checking-links)
  - [Choose a theme](#choose-a-theme)
  - [What next?](#what-next)
  - [Modularity](#modularity)
  - [Docsmith CLI tool](#docsmith-cli-tool)
    - [init](#init)
    - [install](#install)
    - [build](#build)
    - [serve](#serve)
    - [publish](#publish)
    - [load / update / save](#load--update--save)
  - [Configuration](#configuration)
  - [build](#build-1)
- [API](#api)
  - [source](#source)
    - [github](#github)
  - [build](#build-2)
  - [validate](#validate)
  - [editor](#editor)
  - [generate](#generate)
    - [jekyll](#jekyll)
    - [metalsmith](#metalsmith)
  - [publish](#publish-1)
    - [github-pages](#github-pages)
- [Dependencies](#dependencies)
- [Dev dependencies](#dev-dependencies)

<!-- /MarkdownTOC -->


## Getting Started

First make sure you have the docsmith dependencies installed:
 - You'll [need node and npm](https://nodejs.org/en/download/) (version 4.x - version 5 is not yet supported)
 - You'll also [need pandoc](http://pandoc.org/installing.html) (version 1.12 or higher)

The simplest way to get started is to fork one of our sample repos on github:
 - https://github.com/contentascode/blog: A template for a simple blog
 - https://github.com/contentascode/wiki: A template for a wiki
 - https://github.com/contentascode/doc: A template for software documentaiton
 - https://github.com/contentascode/site: A template for a simple website

You can have these templates up and running simply by [activating Github pages on your fork](link). This will make the site available after a few minutes on http://USER.github.io/REPO where ORG is the name of the user or organisation which forked the repo, and REPO is the name of the REPO (which should be blog or site, unless you renamed it.)

Wow, that was easy and fast. What can I do next?

### Use your own domain name

You can make your new site available on the domain name of your choice. If you have procured a domain name you can simply create a CNAME file at the base of this repository and point your DNS following these instructions.

### Collaboration

The Content as Code workflow uses best practices to allow different people to contribute to your content project, in the way that large software engineering projects in the open source world do it.

The sample repos use Github (soon Prose) to help with content editing. By default, all pages of your new website have built in links to

### Checking Links

You can go to the next level by using another free online service (Travis CI) to build your site. This will provide you with additional features such as using validations. For instance to check that links are working or that your content is generally well formatted and readable. You can activate travis by going to travis-ci.com signing up and activating travis for your USER/REPO project.

Is that it? Yes, now you'll receive emails with notifications each time there is a problem with your website build, like a broken link or possible other problems with your site.

### Choose a theme

You can change our default website layout and theme if you have activated Travis CI in the previous step. In that case, you can edit the `_content.yml` file and change the theme to another content as code enabled jekyll theme we support.

### What next?

We're planning to implement many more features with a content as code approach for instance doing diagrams, interactive content, or integration with and migration from your existing content management system.

If you want to go deeper into **docsmith** and content as code, you can also help us develop the command line tool we have started designing below which will help more advanced users with more sophisticated content management needs. It is based on a modular approach

## Modularity

This table aims to represent which components affect which stages of the content as code framework.
 - Key          : means that this component is a key component for this stage.
 - Change       : means that this component modifies or applies to this stage.
 - + component : means that this component works better with the specified component.
 - = component : means that this component only works with the specified component.
 - - component : means that this component doesn't work with the specified component.


| Component \ Stages |  Source  | Author | Build | Generate |  Integrate  | Collaborate | Translate |  Publish   |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| github             | Key      |        |       |          | + travis    |             |           | + gh-pages |
| gitlab             | Key      |        |       |          | + gitlab-ci |             |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| prose              | + github | Key    |       | + jekyll |             |             |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| validate           |          |        |       |          | Change      |             |           |            |
| validate links     |          |        |       |          | Change      |             |           |            |
| validate style     |          |        |       |          | Change      |             |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| grunt              |          |        | Key   |          |             |             |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| jekyll             |          |        |       | Key      |             |             |           |            |
| metalsmith         |          |        |       | Key      |             |             |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| travis             |          |        |       |          | Key         |             |           |            |
| gitlab-ci          | - github |        |       |          | Key         |             |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| gitlab issues      |          |        |       |          |             | Key         |           |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| transifex          |          |        |       |          |             |             | Key       |            |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|
| gh-pages           | + github |        |       |          |             |             |           | Key        |
|--------------------|----------|--------|-------|----------|-------------|-------------|-----------|------------|



## Docsmith CLI tool

 - ```npm install -g docsmith``` : installs content CLI tool and the `content` executable.

### init

 - ```content init```: Or ```content init <template>``` like ```content init blog```, ```content init doc``` or ```content init wiki```.

### install

 - ```content install``` : in a repo would read content.yml and create package.json, metalsmith.json and docker-compose.json and run necessary installations (npm install,...).

 - ```content install validate``` : Install default plugin suite in the integration stage (linkchecker) or the one specified in the `_content.yml` file.

 - ```content install validate style```: Install specific plugin (and updates the yml file)

### build

 - ```content build```: Build the content locally.

### serve

 - ```content serve```: Serve the content locally. (Should probably open a console and allow to `pull`,`update` or `pull` within it)

### publish

 - ```content publish```: Publishes the content (and configure and/or deploy needed microservices).

### load / update / save

 - ```content load/pull``` : Get local content updates
 - ```content load/pull <version>```: Get specific version.
 - ```content update```: Get remote content dependency updates.
 - ```content save/push``` : Push content source updates and advertise changes.
 - ```content save/push <version>``` : Push content source updates and advertise changes.

## Configuration

Example ```_content.yml```:

```yaml

implementation: 'docsmith'  # Which implementation of content as code?

#
# Source
#
#   Source repositories which contain the sources for the current project.
#   Note: These are not upstream dependencies which will be managed with metadata inside
#   and alongside the source files.
#

source:
  github:                   # Defaults to github could also be gitlab or a local folder. Gollum for a wiki?
    owner: iilab
    repo: contentascode
    transform: ''           # There could be some type of processing when query APIs or scraping...
    path: '.'               # Binds to authoring path.

#
# Author
#
#   This is where the content is edited, manipulated and so on. Different authoring environment will have
#   different capabilities (for instance for validation without a server round-trip or workflow aspects...).
#   See the lib/components.js file for a first attempt at modeling these capabilities.
#

author:                  # What is the content authoring environment? Could be prose, realms,...
  - type: 'local'           # Maybe editor plugins could be proposed for desktop based edition.
  - type: 'github'
  - type: 'prose'

translate:
  - type: 'transifex'

#
# Generate
#   The static site generator used.

generate:
  metalsmith:
    config: metalsmith.json

#
# Integrate
#
#   These are the tools used to prepare and validate content and give feedback to authors and editors,
#   including presenting staging or testing environments/artifacts of various versions that are being worked on.
#   Note: Maybe this should be included in the publish component.

integrate:                  # Defaults to empty. Can be travis, or gitlab-ci
  local:
    build: 'npm'               # Which tool is orchestrating local integration tests.
    validate:               # Defaults to empty. List of validation scripts, for instance links,...
      - 'links'
  travis:
    branch: 'versions/*'
    build: 'npm'           # Which tool is orchestrating integration
    validate:
      - 'links'
  shared:                 # Shared component for isomorphic validations?
    validate:
      * 'links'

#
# Publish
#
#   These are the various channels where published versions will be available from.
#

publish:
  - type: 'github-pages'
    url: 'http://iilab.github.io/contentascode'
    build: 'jekyll-github-pages' # Probably useless as its part of capabilities.
    branch: 'gh-pages'
  - type: 'scp'
    files: 'ssh://server.example.org/:/var/www/my_site'
    url: 'https://www.example.org'

#
# Service
#
#   These are various additional services that are linked to various features that are useful
#   for
#

service:
  discuss: 'github'  # Discussion threads, comments, wiki style discuss page...
  review: 'github'          # Line based review like github code comments, could be gitlab...
  stats: 'piwik'

```

 - ```content components``` : display list of available modules.
 - ```content source``` : display current source settings.
 - ```content source gitlab``` : changes repo to gitlab.
 - ```content build travis-ci``` : changes build system to travis ci.
 - ```content validate links``` : Adds link validator module.

## build

In the simplest case the build stage uses the github-pages component which depend on an external build system. It can also depend on other external build systems like travis, or use a more custom build with a custom static website generator, or in more complex cases it can mean assembling various components to for instance generate various outputs or orchestrate deployment of microservices. In that sense the build system could also include configuration management with ansible or docker compose.

Depending on the chosen build system, different targets will be available for the `publish` command to enable `content publish android` or `content publish pdf`.

# API

Each component of the docsmith build pipeline passes to the next component a context (as this is managed via metalsmith) which is the current state of the file structure:
 - metadata
 - files

## source

 - pull: get the latest (or default) version from the content repo.
 - update: pulls dependencies (through git submodules or a package management approach or another yet to be developed approach)
 - version: get a specific version (github version tag or branch or ). From docsmith's perspective all versions are really branches that can be worked on a later merged onto other versions aiming to pick the best merge strategy based on heuristics.
 - push: pushes local changes to content repo.

Configures the build pipeline (for now metalsmith) with the proper and desired source content version. This should allow to wrap dependency management (include with git submodules if wanted).

### github

 * This module connects docsmith to a github repository as a content source.
 * Configuration options
     - `repo`:
         + `url` : (Required) URL of repo.
         + `branch`: (Optional - Defaults to master)

## build

## validate

 - validate: runs validations on the current context and exits with a descriptive validation error

## editor

 - url: ? This could link to a prose instance, it could possibly be deployable as an unhosted app or a desktop app.
 - callbacks? Maybe some glue configuration will be necessary to link things like
     + edit
     + fork
     + commit
     + ...

## generate

Methods:
 - generate
 - watch

### jekyll

### metalsmith

Configuration:
 - templating engine
 - templates

## publish

### github-pages

 * Configuration options
     - `source`
       +

# Dependencies

 - Libgit2 (`brew install libgit2` on OSX)
 - Nodegit
 - pandoc v1.12+

# Dev dependencies
 - Cucumber-js
 -

