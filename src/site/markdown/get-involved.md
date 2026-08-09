<?xml version="1.0" encoding="UTF-8"?>
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->
<document xmlns="http://maven.apache.org/XDOC/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/XDOC/2.0 https://maven.apache.org/xsd/xdoc-2.0.xsd">
  <properties>
    <title>Getting Involved</title>
    <author>Rahul Thakur</author>
    <author>Olivier Lamy</author>
  </properties>

  <body>
    <section name="Getting Involved">

      <p>
        Contributions are welcome. The full guide lives with the code:
        <a href="https://github.com/codehaus-plexus/.github/blob/master/CONTRIBUTING.md">CONTRIBUTING.md</a>.
        The short version is below.
      </p>

      <subsection name="Where to ask">
        <p>
          <a href="https://github.com/codehaus-plexus">GitHub issues</a>, on the repository concerned.
          That is where the maintainers are. There is no Plexus mailing list — if you find one referenced
          in older documentation on this site, it is defunct.
        </p>
        <p>
          Each project on the <a href="./index.html">overview page</a> links to its own repository. If you
          aren't sure which one owns the behaviour you're seeing, open the issue wherever seems closest
          and we'll move it.
        </p>
      </subsection>

      <subsection name="Building">
        <p>
          <code>mvn verify</code>. No profile or local setup is required. CI builds across JDK 8, 21 and 25
          on Linux, Windows and macOS.
        </p>
      </subsection>

      <subsection name="Code style">
        <p>
          Formatting is enforced by the build, via
          <a href="https://github.com/diffplug/spotless">Spotless</a> and
          <a href="https://github.com/palantir/palantir-java-format">palantir-java-format</a>. Don't hand-format,
          and don't reformat code you aren't otherwise touching. Before pushing:
        </p>
        <source>mvn spotless:apply</source>
        <p>
          If CI fails on <code>spotless:check</code>, running that command and committing the result is the
          whole fix.
        </p>
        <p>
          This page previously offered <code>maven-eclipse-codestyle.xml</code> and
          <code>maven-idea-codestyle.xml</code> for import into your IDE. Those predate Spotless and no longer
          match what the build enforces, so they are no longer linked from here. The files themselves are still
          served, so any existing link to them keeps working.
        </p>
      </subsection>

      <subsection name="Licence headers">
        <p>
          New files take the standard header used by the files around them — Apache-2.0 for everything except
          Modello, which is MIT. Please do not update or normalise existing copyright headers, including the
          older Codehaus Foundation ones; they record who contributed what.
        </p>
      </subsection>

      <subsection name="Pull requests">
        <p>
          One concern per pull request, a test where the change is testable, and an explanation of <i>why</i> in
          the description. These libraries are consumed transitively across most of the Maven plugin ecosystem,
          so changes to public API need discussion in an issue first — deprecate rather than remove.
        </p>
      </subsection>

      <subsection name="Security">
        <p>
          Please do not open a public issue for a vulnerability. See
          <a href="https://github.com/codehaus-plexus/.github/blob/master/SECURITY.md">SECURITY.md</a>.
        </p>
      </subsection>

    </section>
  </body>
</document>
