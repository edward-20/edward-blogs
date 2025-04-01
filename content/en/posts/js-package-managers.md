+++
date = '2025-04-01T14:54:32+11:00'
draft = true
title = 'Js Package Managers'
+++

Here's my crash course on the available Javascript package managers for myself
and possibly for you. We consider:
* npm
* Yarn Clasic 
* Yarn Berry
* pnpm

Feature parity has been achieved. Generally you can with all the package
managers:
* edit metadata
* batch install or update all dependencies
* add, update and remove dependencies
* run scripts
* publish packages
* security audits


Differentiating factors of the major package managers:
* installation speed
* how packages are stored on disk
* support for multi-package projects (a.k.a. workspaces)
* Dependencies are stored in `node_modules` in npm and Yarn Classic. Yarn Berry
uses Plug'n'Play. pnpm has nested `node_modules` directories.
* hoisting (security implications)
* lock file formats (performance implications)


# Timeline
## npm
From Jan 2010. Prior to then, project dependencies were handled manually.

2020, Github acquired npm.
## Yarn Classic
October 2016, Facebook and Google announced through a blog to develop a new
package manager to address consistency, security and performance problems. Yet
Another Resource Negotiator.

It parallelised operations to speed up the install process. It improved DX and
introduced new concepts:
* native monorepo support
* cache-aware installs
* offline caching
* lock files

Yarn classic entered maintenance mode in 2020.

## pnpm
Version 1 was released in 2017 by Zoltan Kochan.

It addresses the redundant storage of dependencies used across projects. That is
the hoisting used to flatten their `node_modules`.

pnpm used content-addressable storage. There is a `~/.pnpm-store/` and
`node_modules` with symlinks to 


When installing dependencies with npm or Yarn Classic, all packages are hoisted
to the root of the modules directory. As a result, source code has access to
dependencies that are not added as dependencies to the project.

By default, pnpm uses symlinks to add only the direct dependencies of the
project into the root of the modules directory.

If you depend on different versions of the dependency, only the files that
differ are added to the store. For instance, if it has 100 files, and a new
version has a change in only one of those files, pnpm update will only add 1 new
file to the store, instead of cloning the entire dependency just for the
singular change.


All the files are saved in a single place on the disk. When packages are
installed, their files are hard-linked from that single place, consuming no
additional disk space. This allows you to share dependencies of the same version
across projects.

## Yarn Berry
Released in Jan 2020. PnP was a strategy to fix `node_modules`. Instead a
`.pnp.cjs` with dependency lookup tables is generated. It's better because its a
single file instead of a nested `node_modules`. Every package is stored as a zip
file in `.yarn/cache`.






## Sources
https://blog.logrocket.com/javascript-package-managers-compared/
https://www.jonathancreamer.com/inside-the-pain-of-monorepos-and-hoisting/
https://pnpm.io/motivation

