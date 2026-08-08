---
title: Plexus Overview
author: Brett Porter
date: 2015-09-13
---

# Overview

Codehaus Plexus is a collection of small Java libraries that [Apache Maven](https://maven.apache.org/) and its plugins are built on: archive handling, compiler abstraction, file I/O, XML, string interpolation, classloader management and code generation.

If you write Maven plugins you almost certainly depend on several of these already, usually transitively.

Each project has its own site, linked below, with Javadoc and reports. Sources and issues are on [GitHub](https://github.com/codehaus-plexus).

## A note on the name

Plexus originally had two halves: an IoC container, and a set of components written for it. **The container is retired.** Maven moved to [Eclipse Sisu](https://www.eclipse.org/sisu/) and [JSR-330](https://jcp.org/en/jsr/detail?id\=330) years ago, and nothing listed below needs a Plexus container to run -- the components are ordinary JSR-330 beans.

What remains, and what this project is now, is the second half: the libraries below. The container documentation is still on this site, under _Old IoC Documentation_, because a great deal of writing from that era links to it. See [Feature Comparison](./ref/feature-comparison.html) for how Plexus related to other IoC ecosystems of the time.

# Libraries

- [plexus-utils](./plexus-utils/) -- utilities for strings, files, command lines and process execution.

- [plexus-xml](./plexus-xml/) -- XML classes split out of _plexus-utils_ 4 (`Xpp3Dom` and friends).

- [plexus-io](./plexus-io/) -- file and resource abstractions, selectors and mappers.

- [plexus-interpolation](./plexus-interpolation/) -- resolves expressions in the `${...}` style; the engine behind POM interpolation.

- [plexus-classworlds](./plexus-classworlds/) -- classloader management; how Maven isolates plugins from itself.

# Components

- [plexus-archiver](./plexus-archiver/) -- one API over zip, jar, tar and their compressed variants.

- [plexus-compiler](./plexus-compiler/) -- one API over javac, ECJ, AspectJ and others; used by _maven-compiler-plugin_.

- [plexus-languages](./plexus-languages/) -- reads `module-info` and splits the classpath from the module path (_plexus-java_).

- [plexus-sec-dispatcher](./plexus-sec-dispatcher/) -- encrypts and decrypts passwords in `settings.xml`. See the [Maven encryption guide](https://maven.apache.org/guides/mini/guide-encryption-4.html) for usage.

- [plexus-resources](./plexus-resources/) -- reads a resource from the filesystem, the classpath or a URL.

- [plexus-velocity](./plexus-velocity/) -- Apache Velocity integration.

- [plexus-i18n](./plexus-i18n/) -- resource bundle lookup for localised messages.

- [plexus-interactivity](./plexus-interactivity/) -- prompts the user on the console.

- [plexus-build-api](./plexus-build-api/) -- lets plugins report file changes to an incremental build, such as m2e.

# Tooling

- [Modello](./modello/) -- generates Java classes, readers and writers, XSD and documentation from a single model file. MIT licensed; everything else here is Apache-2.0.

- [plexus-testing](./plexus-testing/) -- JUnit 5 support for testing JSR-330 components.

- [plexus](./plexus-pom/) -- the parent POM every project here inherits.

# Retired

These are archived and receive no fixes, including security fixes. Don't start anything new on them.

- [plexus-containers](./plexus-containers/) -- use [Eclipse Sisu](https://www.eclipse.org/sisu/) with JSR-330 annotations.

- [plexus-cipher](https://github.com/codehaus-plexus/plexus-cipher) -- absorbed into [plexus-sec-dispatcher](./plexus-sec-dispatcher/) 4.x.

- [plexus-digest](./plexus-digest/) -- use `java.security.MessageDigest`, or Commons Codec.

- [plexus-cli](https://github.com/codehaus-plexus/plexus-cli) -- use Commons CLI or picocli.

- [plexus-component-factories](https://github.com/codehaus-plexus/plexus-component-factories) and [plexus-maven-plugin](https://github.com/codehaus-plexus/plexus-maven-plugin) -- container concerns that no longer exist under Sisu.

- [plexus-swizzle](https://github.com/codehaus-plexus/plexus-swizzle) -- no replacement.

- [plexus-components](https://github.com/codehaus-plexus/plexus-components) -- split into the individual repositories above.

# Contributing

Issues and pull requests are welcome on each repository. See [CONTRIBUTING](https://github.com/codehaus-plexus/.github/blob/master/CONTRIBUTING.md) for building, the Java baseline and code formatting.

To report a security vulnerability, please follow [SECURITY](https://github.com/codehaus-plexus/.github/blob/master/SECURITY.md) rather than opening a public issue.
