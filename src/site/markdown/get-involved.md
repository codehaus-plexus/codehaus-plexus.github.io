---
title: Getting Involved
author: 
  - Rahul Thakur
  - Olivier Lamy
---

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

# Getting Involved

Contributions are welcome. The full guide lives with the code: [CONTRIBUTING.md](https://github.com/codehaus-plexus/.github/blob/master/CONTRIBUTING.md). The short version is below. 

## Where to ask

[GitHub issues](https://github.com/codehaus-plexus), on the repository concerned. That is where the maintainers are. There is no Plexus mailing list — if you find one referenced in older documentation on this site, it is defunct. 

Each project on the [overview page](./index.html) links to its own repository. If you aren't sure which one owns the behaviour you're seeing, open the issue wherever seems closest and we'll move it. 

## Building

`mvn verify`. No profile or local setup is required. CI builds across JDK 8, 21 and 25 on Linux, Windows and macOS. 

## Code style

Formatting is enforced by the build, via [Spotless](https://github.com/diffplug/spotless) and [palantir-java-format](https://github.com/palantir/palantir-java-format). Don't hand-format, and don't reformat code you aren't otherwise touching. Before pushing: 

```unknown
mvn spotless:apply
```

If CI fails on `spotless:check`, running that command and committing the result is the whole fix. 

This page previously offered `maven-eclipse-codestyle.xml` and `maven-idea-codestyle.xml` for import into your IDE. Those predate Spotless and no longer match what the build enforces, so they are no longer linked from here. The files themselves are still served, so any existing link to them keeps working. 

## Licence headers

New files take the standard header used by the files around them — Apache-2.0 for everything except Modello, which is MIT. Please do not update or normalise existing copyright headers, including the older Codehaus Foundation ones; they record who contributed what. 

## Pull requests

One concern per pull request, a test where the change is testable, and an explanation of _why_ in the description. These libraries are consumed transitively across most of the Maven plugin ecosystem, so changes to public API need discussion in an issue first — deprecate rather than remove. 

## Security

Please do not open a public issue for a vulnerability. See [SECURITY.md](https://github.com/codehaus-plexus/.github/blob/master/SECURITY.md). 

