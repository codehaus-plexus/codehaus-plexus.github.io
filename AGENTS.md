# Agent guide for codehaus-plexus.github.io

This repository is the source of the Codehaus Plexus organization site at
<https://codehaus-plexus.github.io/> (`org.codehaus.plexus:plexus-site`, `pom` packaging). It has no
Java code and no test suite. Pages are Markdown under `src/site/markdown`: the overview
(`index.md`), `get-involved.md`, and the retired Plexus container documentation under `guides/` and
`ref/`. `src/site/site.xml` defines the menu, and `apt-old/`, `src/site/xdoc` and `src/site/old`
hold older material that is not rendered. The published URLs of the container guides are linked
from a lot of external writing: do not move, rename or delete a page, and do not change its
headings. The container tutorials are short notes that point to
`guides/migrating-from-plexus-container.md`; put new migration advice there, not in the old pages.

## Build and test

- Run `mvn site` to render the site into `target/site`. The reports configured in the POM are the
  `maven-project-info-reports-plugin` team, issue-management, licenses and scm reports.
- The site is the organization page, so `maven-scm-publish-plugin` publishes it to the `master`
  branch (`scmBranch` in the POM), not to `gh-pages`. `mvn site-deploy` does that; the
  `site.yml` workflow runs the same publish through the shared `codehaus-plexus/.github` site
  workflow, and its `dry-run` input builds without committing. Publishing steps are in
  `RELEASING.md` of that shared repository.
- `.github/workflows/maven.yml` calls the shared `codehaus-plexus/.github` build workflow (the
  default `mvn verify javadoc:javadoc` on the shared JDK and operating system matrix) and then the
  shared deploy workflow.
- Run `mvn spotless:apply` on JDK 17 to 21 before pushing; it also formats Markdown.

## Documentation updates

A change that users or contributors can notice ships with the documentation it affects, in the
same pull request. A refactor with no visible effect needs none.

- A library gets a new site, is retired, or changes its one-line description: the matching entry in
  `src/site/markdown/index.md`, under Libraries, Components, Tooling or Retired.
- A page is added or removed: the menu in `src/site/site.xml`.
- The way the site is built or published: `README.md` and the Build and test section of this file.
- A new convention, build gate or fixture: this file.

