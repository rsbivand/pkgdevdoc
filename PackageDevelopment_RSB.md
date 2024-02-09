---
editor_options: 
  markdown: 
    wrap: 72
---

## Is a package development CTV possible?

### Introduction

[partly in new #Introduction, skip codetools as more general than pkg
dev (still mention later), WRE versioning moved to #Maintenance]

~~R ships with a lot of functionality, distributed via base
(`r pkg("utils")`, `r pkg("tools")`) and recommended
(`r pkg("codetools")`) packages for managing and developing packages,
following the [Writing R
Extensions](https://cran.r-project.org/doc/manuals/R-exts.html) (WRE)
manual; additional functionality is provided by contributed packages.
WRE defines what is accepted by standard R methods and it is the
defining manual of package development.~~ ~~WRE is versioned, as are all
[manuals](https://cran.r-project.org/manuals.html) addressing
R-released, R-patched, and R-devel, and changes do occur as R itself
develops, and as the software components on which R is built themselves
evolve.~~

[partly in new #Introduction, reference rather than rephrase WRE/CRAN
policies]

~~This Task View, unlike most task views, is not only a curated listing
of contributed packages available from the [Comprehensive R Archive
Network](https://cran.r-project.org) (CRAN) that may be helpful in
package development, but also places those packages in the context of
the crucial role played by packages and package development in the
central software architecture of R itself. Consequently, before
contributed CRAN packages that may assist package developers are
presented, this document will rehearse selected elements of the context
of package development, which differ in many ways from the context of
other interpreted language communities.~~

### Packages

[this section replaced by references to WRE in relevant subsections]

~~WRE defines (p. 2, PDF rendering of R-devel (pre-4.4) version) a
package as: "a directory of files which extend R, a source package (the
master files of a package), or a tarball containing the files of a
source package, or an installed package, the result of running R CMD
INSTALL on a source package." Further: "On some platforms (notably macOS
and Windows) there are also binary packages, a zip file or tarball
containing the files of an installed package which can be unpacked
rather than installing from sources."~~

~~Installed packages are kept in one or more libraries (p. 2): "A
directory into which packages are installed, e.g. /usr/lib/R/library: in
that sense it is sometimes referred to as a library directory or library
tree (since the library is a directory which contains packages as
directories, which themselves contain directories)."~~

~~Operations on packages include (p. 2):~~

-   ~~"The most common is installation which takes a source package and
    installs it in a library using `R CMD INSTALL` or
    `utils::install.packages`."~~

-   ~~"Source packages can be built. This involves taking a source
    directory and creating a tarball ready for distribution, including
    cleaning it up and creating PDF/HTML documentation from any
    vignettes it may contain."~~

-   ~~"Source packages (and most often tarballs) can be checked, when a
    test installation is done and tested (including running its
    examples); also, the contents of the package are tested in various
    ways for consistency and portability."~~

~~Package structure (p. 3) is defined as: "The sources of an R package
consist of a subdirectory containing the files DESCRIPTION and
NAMESPACE, and the subdirectories R, data, demo, exec, inst, man, po,
src, tests, tools and vignettes (some of which can be missing, but which
should not be empty). The package subdirectory may also contain files
INDEX, configure, cleanup, LICENSE, LICENCE and NEWS."~~

### Package Development

[skip: people searching for ctv likely know this already]

~~Many R packages have been created and are actively maintained by user
groups in organisations or corporations, and are only used within those
settings. Some of the suggestions made here will also be relevant for
them, even though the authors do not intend to submit them to CRAN. Some
such packages, by groups or single authors, are made available from for
example [GitHub](https://github.com), as supplementary material to
journal publications, of from [Zenodo](https://zenodo.org/).~~

[skip: better in "code development" tv, also important for reproducible
research]

~~Use of a [version control
system](https://en.wikipedia.org/wiki/Version_control) in package
development is essential, to be able to track changes as the text files
making up the nascent package. Even if all developers at first think
that learning and using such a system is an unnecessary burden ("I keep
off-site backups, shouldn't that suffice?"), the benefits accrue over
time, especially as users of the production package suggest
enhancements.~~

[skip: following paragraphs covered in WRE and not focused on packages
supporting development - we don't give all the system requirements for
packages in other task views]

~~Users of Windows wishing to develop packages need to install
[Rtools](https://cran.r-project.org/bin/windows/Rtools/) for the version
of R under which development is taking place, consulting versioned
guides ([See for R 4.3
here](https://cran.r-project.org/bin/windows/base/howto-R-4.3.html)).
[MikTeX](https://miktex.org/) (with basic packages and inconsolata) is
needed to build PDF-format vignettes and documentation.
[Pandoc](https://pandoc.org/) may also be needed; both MikTeX and Pandoc
should be made available to the Rtools console.~~

~~Similarly for macOS, software described in
[tools](https://mac.r-project.org/tools/) should be available, at least
`Xcode`, the specified Fortran compiler, LaTex and probably Pandoc.~~

~~Users of other `"unix"` systems will typically be able to update their
existing systems by installing the relevant development bundle for R on
their platform: `R-core-devel` for
[Fedora](https://cran.r-project.org/bin/linux/fedora/), `r-base-dev` for
[Debian](https://cran.r-project.org/bin/linux/debian/) and
[Ubuntu](https://cran.r-project.org/bin/linux/ubuntu/fullREADME.html);
these typically depend on the software components needed to build, check
and install source packages.~~

~~Should the package being developed contain compiled code linking to
software external to R, those software components should be named in the
`DESCRIPTION` file in the `SystemRequirements` field (WRE, p. 6). If
those components are already provided in Rtools for Windows (typically
in, for example, `C:/rtools43/x86_64-w64-mingw32.static.posix/lib`), or
in R-macOS [recipes](https://github.com/R-macos/recipes), adding a
simple `Makevars` file (WRE p. 25) to the package `src` directory should
suffice. Since contributed packages including compiled code and built
for Windows or macOS are statically linked, that is the installed
package dynamically linked library (shared object) only links to R over
and above the operating system and is otherwise self-contained, the
external components need to be built with the same build train as R
itself.~~

~~On `"unix"` systems other than macOS, dynamic linking of shared
objects is used by default, and typically a `configure` file is used to
create `src/Makevars` before package installation (WRE p. 20, often a
`configure` file built with `autoconf`). Configure arguments may also be
used to point to the headers and library files, etc., of external
software components and possibly their shared metadata files.~~

~~It is also possible to bundle the source code of small external
software components in the R package under development, but then the
package author will need to take responsibility for maintaining this
code as well as the package; this approach is often used for `jar` Java
components.~~

[important links moved to #links]

~~Many R packages over and above those available from CRAN are curated
and made available for source and binary installation by
[Bioconductor](https://www.bioconductor.org/), which provides clear
[guidelines for package
development](https://contributions.bioconductor.org/). Here, packages
available from CRAN and Bioconductor will be termed contributed
packages, because they not only use base R, but also aim to contribute
to the extension of the R ecosystem through these two providers.~~

~~In addition, [ROpenSci](https://ropensci.org/) offers a framework
which is described as community-driven learning, including peer reviews
of draft packages. The organisation also hosts a range of packages for
enhancements to aspects of package development, such as interaction with
git repositories. Some journals, such as [The R
Journal](http://journal.r-project.org/) or [Journal of Statistical
Software](https://www.jstatsoft.org), require publication of packages on
CRAN or Bioconductor for article submissions.~~

[important links moved to #Links; or #Maintenance]

~~Should package developers choose CRAN as their chosen method of
distribution, they should be aware that they need to adhere to [CRAN
Repository
Policy](https://cran.r-project.org/web/packages/policies.html) over and
above WRE. This document also evolves over time (tracked by this
[repository](https://github.com/eddelbuettel/crp)).~~ ~~Package
developers are further supported by the
[R-package-devel](https://stat.ethz.ch/mailman/listinfo/r-package-devel)
mailing list; it is instructive to subscribe and read digested postings
as they often indicate that many package developers share issues, such
as controlling the number of processor cores being used for
computation.~~

[skip: essentially said already]

~~Many useful tools and R packages themselves have been created to ease
or improve package development. This Task View focuses on these R
packages. Packages used for formatting or making package development
more "user-friendly" might also appear in other Tasks Views. A lot of
functionality within this Task View is present in the `r pkg("tools")`
package or other packages always available when R is installed, in these
cases it is mentioned at the top of the section.~~

### Development of packages to be submitted to CRAN or Bioconductor

[following paragraphs condensed to intro of #Maintenance section, with
important references extracted]

~~Package developers should be aware that their packages should be
maintained (as long as reasonably possible) if distributed in
repositories like CRAN or Bioconductor. Making your software available
as an R package is obviously beneficial for yourself and for users of
your work. While your chosen licence may assert that the software is
provided as-is, submitting to Bioconductor or CRAN further implies a
responsibility to maintain your work. Bioconductor has a twice-yearly
release cycle, where packages failing to conform are deprecated for the
next release. Reasons may include lack of adherence to policy, failure
to fix bugs, or bouncing maintainer e-mail addresses.~~

~~In the case of CRAN, the relationship is even closer, as the corpus of
CRAN contributed packages (all 20,000 or so of them) is used to test
changes to the development version of R. Some changes are discarded when
the fallout among contributed packages is too severe, other changes are
implemented gradually by introducing first check notes, then warnings,
and finally errors in package checks. Once a published contributed
package starts accruing extra notes, but more seriously warnings, CRAN
team will first suggest updates, but if the maintainer is unresponsive,
will set a tighter deadline for fixing problems.~~

~~One such change was to flip `stringsAsFactors` from `TRUE` to `FALSE`
on reading data, as discussed in a
[blog](https://blog.r-project.org/2020/02/16/stringsasfactors/index.html),
another was the problem of [conditions of length greater than
one](https://blog.r-project.org/2018/10/12/conditions-of-length-greater-than-one/index.html):
`if (a)` when `length(a)` could be greater than one, the outcome was
unclear, but by custom had taken `a[1]`. Both of these changes to R
itself were discussed in the [R development blog
series](https://blog.r-project.org/), which is a further very useful
source of information.~~

~~Discussions on the
[R-devel](https://stat.ethz.ch/mailman/listinfo/r-devel) mailing list
can also illuminate forthcoming changes in R itself. They also often
touch on other aspects of the support ecosystem, especially changes
related to new compiler versions, and specific issues with new versions
of operating systems. For R itself, these are handled by R core, but
developers of packages using compiled code (C, C++, Fortran, etc.) may
well find it helpful to follow discussions as they occur.
Platform-specific changes in build trains (`gcc` or `llvm/clang` based),
and forecasts of impending sharpening of default arguments (such as
`C++` namespaces) lead to quite frequent tightening of check
requirements, because R is conservatively backward-compatible by design
(changes should not change workflow outcomes unless bugs have been
confirmed, or have been extensively advertised). Listings of environment
variables used in `R CMD check` may be found the the [Tools
chapter](https://cran.r-project.org/doc/manuals/r-devel/R-ints.html#Tools)
of the R Internals manual.~~

[this moved to #continuous-integration]

~~The maintenance work required might be due to changes in the
computational environment, as in package dependencies, in the repository
policies or in R. Some of these can be detected by diligent use of
continuous integration, such as GitHub Actions, which mimics the local,
manual use of installing a development version of R, updating packages,
and running `R CMD check --as-cran` on the package(s) in question. For
hints on setting up GitHub Actions, the `check-r-package` action is
among [many useful suggestions](https://github.com/r-lib/actions).~~

[these moved to #checking-a-package]

~~Liberal use may also be made of the [R
Win-builder](https://win-builder.r-project.org/) once a source package
has been built, and/or of the very similar [macOS
builder](https://mac.r-project.org/macbuilder/submit.html).~~ ~~For some
years, the builders offered by [R-hub](https://builder.r-hub.io/) were
up-to-date; check the list of builders returned by `rhub::platforms`
from the `r pkg("rhub")` package.~~

[ this moved to #debugging-issues-found-in-package-checks]

~~R-hub also offers a
[selection](https://github.com/r-hub/rhub-linux-builders) of Docker
containers using similar builders. The
[Rocker](https://rocker-project.org/images/) project offers stacks of
system images that can assist in pinning down both simple and complex
problems encountered when checking packages, aain using Docker
containers. These images are actively maintained and include versioned
images catering for clusters of packages in the R ecosystem using shared
infrastructure, such as "Tidyverse".~~

[following paragraphs condensed to intro of #Maintenance section, with
important references extracted]

~~Naturally, all packages deserve to be actively maintained,
irrespective of whether they have been submitted to CRAN or
Bioconductor, are only used internally in an institution or corporation,
or support published work. This requires continued effort from the
maintainer, rewarded often by growth in usage and efficiency. The tools
used for maintaing packages published on CRAN or Bioconductor can and
should also be used for the maintenance anf further development of these
other packages and their associated workflows. In institutional
settings, it is highly advisable to budget for continued development and
maintenance.~~

~~Should a package previously published on CRAN fail to be updated in
time, it will be "archived", that is all its source packages remain in
their folder in the
[Archive](https://cran.r-project.org/src/contrib/Archive), but are no
longer offered for download and installation or used for checking
changes in R itself. If the maintainer belatedly submits an updated
package, it will be scrutinised as if it was a new submission, but on
success will return to regular production. Maintainers can pre-emptively
ask CRAN team to archive a package, maintainers can recruit a new
maintainer who will take over the packages, or if no replacement is
available, a package may be
[orphaned](https://cran.r-project.org/src/contrib/Orphaned/README).~~

### Package Dependencies

[move to #Dependency management]

~~At first submission to CRAN, no other packages depend on new package
`A`. It will almost certainly itself declare dependencies, most likely
on a version of R greater than or equal to a given level (often because
newer compression algorithms are used when reducing the size of stored
data objects). Dependencies on packages, say `B` and `C`, may also be
versioned, these are known as forward dependencies, `A` depends on `B`
and `C` - and `C` may itself depend on `D`.~~

[ moved to #keeping-up-to-date. skip note on unit test this more of a
howto]

~~This means that non-contributed packages are not isolated from changes
in subsequent versions of R, or of packages on which the non-contributed
package depends. For this reason, testing release candidates of R on
critical workflows, and tracking the development versions of crucial
upstream dependencies is always prudent.~~ Likewise, writing unit tests
that assume that all upstream components are fixed for ever is tricky,
as a change in object structure could signal not an error but rather a
fixed bug or an enhancement.

[ following sections moved to #dependency-management]

~~As the package attracts users, some may contribute their own CRAN
packages, or update existing CRAN packages to utilize functionality in
the new package. This creates reverse dependencies, say `Z` and `Y`
depend on `A`. The CRAN package list includes the forward dependencies
declared by packages. Dependency graphs may be created using
`tools::package_dependencies`, based on a package database like that
returned by `utils::available.packages`. These can be used to list and
then check reverse dependencies that may be impacted as your package
evolves.~~

~~Note also that WRE (and CRAN) permit dependencies on Bioconductor
packages, and, when using the `Additional_repositories` field in the
`DESCRIPTION` file, to other repositories such as those using
`r pkg("drat")`, or for example INLA by
<https://www.r-inla.org/download-install>. Reverse dependencies back to
CRAN packages from Bioconductor packages may be found by updating the
package database repository list from `utils::available.packages` before
running `tools::package_dependencies`.~~

[replace following sections with ref to WRE]

~~WRE (p. 6) describes the fields in the `DESCRIPTION` file specifying
dependencies: `Depends`, `Imports`, `LinkingTo`, `Suggests` and
`Enhances`. Strong dependencies are those that must be installed for the
packages depending on them to function, that is the first three listed
(definition in `?tools::package_dependencies`).~~

~~Packages listed as `Depends` are loaded and attached before the
package in which they are listed (loaded means that their namespaces are
available and may be accessed in code using the double colon operator
`::`, and objects may be imported in the `NAMESPACE` file, while
attached means that the packages are available as though `library` or
`require` had been used). WRE notes (p. 11): "Field 'Depends' should
nowadays be used rarely, only for packages which are intended to be put
on the search path to make their facilities available to the end user
(and not to the package itself) ..."~~

~~Packages listed as `Imports` are loaded but not attached, so that
their namespaces can be cherry-picked in the `NAMESPACE` file. This is
important if you consider that bulk-importing the namespaces of many
packages will also import the namespaces of their strong forward
dependencies, so that the search path used by the R engine grows in
length and it takes longer to find functions during execution.~~

~~The `LinkingTo` field is used when C/C++ code in the package links to
headers for compilation of compiled (C, C++, Fortran, etc.) code in the
specified package - `r pkg("Rcpp")` has very many such reverse
dependencies as would be expected.~~

~~The `Suggests` and `Enhances` fields are weak dependencies, as they
are not required to be present for the package to perform its core
functions. If a package is listed as `Suggests`, any use in code,
examples, vignettes, etc. must be conditional, that is the code using
the suggested package can be avoided using for example:
`if (require(, quietly = TRUE))` or
`if (requireNamespace(, quietly = TRUE))`.~~

[moved to #checking-a-package]

~~WRE notes (p. 13): "On most systems, `R CMD check` can be run with
only those packages declared in 'Depends' and 'Imports' by setting
environment variable `_R_CHECK_DEPENDS_ONLY_=true`, whereas setting
`_R_CHECK_SUGGESTS_ONLY_=true` also allows suggested packages, but not
those in 'Enhances' nor those not mentioned in the `DESCRIPTION` file.
It is recommended that a package is checked with each of these set, as
well as with neither."~~

~~Usefully, `_R_CHECK_SUGGESTS_ONLY_` lets us check packages in a
constricted setting (R Internals, p. 57, PDF rendering of R-devel
(pre-4.4) version): "If set to a true value, running examples, tests and
vignettes is done with a temporary library directory populated by all
the Depends/Imports/Suggests packages. (As exceptions, packages in a
'VignetteBuilder' field are always made available.) Default: false (but
true for CRAN submission checks: some of the regular checks use true and
some use false)."~~

[this one less important if follow advice above]

~~Another important environmental variable set for checking is
`_R_CHECK_FORCE_SUGGESTS_` (R Internals, p. 47): "If true, give an error
if suggested packages are not available. Default: true (but false for
CRAN submission checks)."~~

[in #dependency-management]

~~A further possible source of difficulties that are hard to debug is
where a package itself has no direct dependency relationship with
another package, but where they are related through an intermediate
package. `tools::package_dependencies` has some options to handle these
settings, but such problems do place responsibility on maintainers to
review the check results of packages they maintain regularly, and to
`utils::update.packages` themselves when trying out modifications to
their own packages. Keeping copies of the output of `utils::sessionInfo`
or equivalently `sessioninfo::session_info` is helpful in tracking
changes caused by upstream updates.~~

### 

[following replaced with new intro]

~~Packages listed here automate some processes, or establish a process
to be used but they themselves might be(come) stale, be updated or
archived.~~

~~In this situation, users might need to update their package. It might
require using fewer tools, updating the tools used or reuse any process
established to comply with the R extension manual and repositories
policies (if the package is shared in a repository).~~

~~Packages included here are neither meant to endorse the "best"
packages for a given task, nor are they listed because they comply with
current or past CRAN policy. Some packages are related to
repositories.~~

~~If you think that some package is missing from the Task View, please
file an issue in the GitHub repository or contact the maintainer(s).~~

Package creation

[now #Initializing-a-package]

~~These packages provide some functionality to create the structure
needed for a package: The DESCRIPTION file, the NAMESPACE, licence, R
code and documentation. A template is provided by
tools::package.skeleton.~~

Checking and building packages

[ now #Checking-a-package, following essentially covered by main
intro/intro to this section/maintenance section]

~~Building a package is done via R CMD build which can be later checked
with R CMD check. Repositories and maintainers determine what is
considered a failure. For example: CRAN uses non-default environmental
variables and procedures to enforce behaviour in the packages. Before
distributing a package, the maintainer should make efforts to reach the
desired levels of quality (be those self-imposed or validated
externally: the repository, a third entity, the institution where it is
developed...).~~

[in #keeping-up-to-date]

~~Due to the amount of work it requires to review the high quality
standards of packages in repositories, they can be archived without
warning. It is the responsibility of the maintainer to be aware of
possible problems. tools::testInstalledPackage to check installed
packages.~~

Writing package documentation

[in #writing-package-documentation]

~~Packages must be documented via R's format: R documentation.
Functions, objects and other elements can have a help page. In addition
there are vignettes to show more complex uses of a package.~~

~~These packages provide some functionality to annotate and document a
package like transforming from one format to the R documentation format.
R provides functions such as tools::Sweave. A special mention for
tools::prompt which provides a template for writing a function and
tools::promptData which provides a template for an object.~~

Package namespaces

[in #dependency-management]

~~Packages must have a namespace, which controls what is imported and
exported from the package. Some tools that help with the documentation
help also handling the namespaces, others are more focused on
dependencies tradeoffs. tools::package_depencencies and
tools::check_packages_in_dir~~

Internationalisation

[in #locastation or #text-quality-checks]

~~Packages might be addressed to people using a different language and
locales. These packages help with the process, so package maintainers
can write for them. tools::checkPoFile to check translations~~ and
tools::aspell to check typos

Tidying and profiling R code

[in #code-quality-checks: limit scope to package checks, not general
code tools]

~~Package source code is often looked upon by users to understand the
functionality and how it is done. At the same time maintainers often
find that it is easier to maintain a package with more readable code.
These packages provide some rules and helpful functionality.~~

~~Maintainers might be interested in optimising the code, so that users
spend less time waiting for it to finish. The following packages provide
functions to measure, compare and optimise code. To compare the time
required to execute some code R provides utils::Rprof.~~
