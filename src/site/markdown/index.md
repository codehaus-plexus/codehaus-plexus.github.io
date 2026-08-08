 -----
 Plexus Overview
 -----
 Brett Porter
 -----
 2015-09-13
 -----

Overview

  Codehaus Plexus is a collection of small Java libraries that {{{https://maven.apache.org/}Apache Maven}} and
  its plugins are built on: archive handling, compiler abstraction, file I/O, XML, string interpolation,
  classloader management and code generation.

  If you write Maven plugins you almost certainly depend on several of these already, usually transitively.

  Each project has its own site, linked below, with Javadoc and reports. Sources and issues are on
  {{{https://github.com/codehaus-plexus}GitHub}}.

* A note on the name

  Plexus originally had two halves: an IoC container, and a set of components written for it.
  <<The container is retired.>> Maven moved to {{{https://www.eclipse.org/sisu/}Eclipse Sisu}} and
  {{{https://jcp.org/en/jsr/detail?id=330}JSR-330}} years ago, and nothing listed below needs a Plexus
  container to run -- the components are ordinary JSR-330 beans.

  What remains, and what this project is now, is the second half: the libraries below. The container
  documentation is still on this site, under <Old IoC Documentation>, because a great deal of writing
  from that era links to it. See {{{./ref/feature-comparison.html}Feature Comparison}} for how Plexus
  related to other IoC ecosystems of the time.

Libraries

  * {{{./plexus-utils/}plexus-utils}} -- utilities for strings, files, command lines and process execution.

  * {{{./plexus-xml/}plexus-xml}} -- XML classes split out of <plexus-utils> 4 (<<<Xpp3Dom>>> and friends).

  * {{{./plexus-io/}plexus-io}} -- file and resource abstractions, selectors and mappers.

  * {{{./plexus-interpolation/}plexus-interpolation}} -- resolves expressions in the <<<$\{...\}>>> style;
    the engine behind POM interpolation.

  * {{{./plexus-classworlds/}plexus-classworlds}} -- classloader management; how Maven isolates plugins
    from itself.

  []

Components

  * {{{./plexus-archiver/}plexus-archiver}} -- one API over zip, jar, tar and their compressed variants.

  * {{{./plexus-compiler/}plexus-compiler}} -- one API over javac, ECJ, AspectJ and others; used by
    <maven-compiler-plugin>.

  * {{{./plexus-languages/}plexus-languages}} -- reads <<<module-info>>> and splits the classpath from
    the module path (<plexus-java>).

  * {{{./plexus-sec-dispatcher/}plexus-sec-dispatcher}} -- encrypts and decrypts passwords in
    <<<settings.xml>>>. See the
    {{{https://maven.apache.org/guides/mini/guide-encryption-4.html}Maven encryption guide}} for usage.

  * {{{./plexus-resources/}plexus-resources}} -- reads a resource from the filesystem, the classpath or a URL.

  * {{{./plexus-velocity/}plexus-velocity}} -- Apache Velocity integration.

  * {{{./plexus-i18n/}plexus-i18n}} -- resource bundle lookup for localised messages.

  * {{{./plexus-interactivity/}plexus-interactivity}} -- prompts the user on the console.

  * {{{./plexus-build-api/}plexus-build-api}} -- lets plugins report file changes to an incremental
    build, such as m2e.

  []

Tooling

  * {{{./modello/}Modello}} -- generates Java classes, readers and writers, XSD and documentation from a
    single model file. MIT licensed; everything else here is Apache-2.0.

  * {{{./plexus-testing/}plexus-testing}} -- JUnit 5 support for testing JSR-330 components.

  * {{{./plexus-pom/}plexus}} -- the parent POM every project here inherits.

  []

Retired

  These are archived and receive no fixes, including security fixes. Don't start anything new on them.

  * {{{./plexus-containers/}plexus-containers}} -- use {{{https://www.eclipse.org/sisu/}Eclipse Sisu}}
    with JSR-330 annotations.

  * {{{https://github.com/codehaus-plexus/plexus-cipher}plexus-cipher}} -- absorbed into
    {{{./plexus-sec-dispatcher/}plexus-sec-dispatcher}} 4.x.

  * {{{./plexus-digest/}plexus-digest}} -- use <<<java.security.MessageDigest>>>, or Commons Codec.

  * {{{https://github.com/codehaus-plexus/plexus-cli}plexus-cli}} -- use Commons CLI or picocli.

  * {{{https://github.com/codehaus-plexus/plexus-component-factories}plexus-component-factories}} and
    {{{https://github.com/codehaus-plexus/plexus-maven-plugin}plexus-maven-plugin}} -- container concerns
    that no longer exist under Sisu.

  * {{{https://github.com/codehaus-plexus/plexus-swizzle}plexus-swizzle}} -- no replacement.

  * {{{https://github.com/codehaus-plexus/plexus-components}plexus-components}} -- split into the
    individual repositories above.

  []

Contributing

  Issues and pull requests are welcome on each repository. See
  {{{https://github.com/codehaus-plexus/.github/blob/master/CONTRIBUTING.md}CONTRIBUTING}} for building,
  the Java baseline and code formatting.

  To report a security vulnerability, please follow
  {{{https://github.com/codehaus-plexus/.github/blob/master/SECURITY.md}SECURITY}} rather than opening a
  public issue.
