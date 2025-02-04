
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Useful git pre-commit hooks for R related projects

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/lorenzwalthert/pre-commit-hooks.svg?branch=master)](https://travis-ci.org/lorenzwalthert/pre-commit-hooks)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

A collection of git pre-commit hooks to use with
[pre-commit.com](https://pre-commit.com). Currently, we have:

  - `style-files`: A hook to style files with
    [styler](https://styler.r-lib.org). Only commit code corresponding
    to the tidyverse style guide. NOTE: devel version of styler strongly
    suggested. Install via `remotes::install_github('r-lib/styler')`.
  - `readme-rmd-rendered`: Make sure `README.Rmd` hasn’t been edited
    more recently than `README.md`, i.e. remind you to render the `.Rmd`
    to `.md` before committing.
  - `parsable-R`: Checks if your `.R` files are “valid” R code.
  - `no-browser-statement`: Guarantees you that you don’t accidentally
    commit code with a `browser()` statement in it.
  - `roxygenize`: A hook to run `roxygen2::roxygenize()`. Makes sure you
    commit your `.Rd` changes with the source changes.
  - `use-tidy-description`: A hook to run
    `usethis::use_tidy_description()` to ensure dependencies are ordered
    alphabetically and fields are in standard order.

To add a pre-commit hook to your project, install pre-commit as
described in the [official documentation](https://pre-commit.com/#intro)
and make sure the executable `pre-commit` is in a place that is on your
`$PATH`.

If you installed pre-commit, you can add it to a specific project by
adding a `.pre-commit-config.yaml` file that has a structure like this:

``` yaml
repos:
-   repo: https://github.com/lorenzwalthert/pre-commit-hooks
    rev: v0.0.0.9008
    hooks: 
    -   id: style-files
    -   id: parsable-R
    -   id: no-browser-statement
    -   id: readme-rmd-rendered
    -   id: roxygenize
    -   id: use-tidy-description
```

If you want to see the file `.pre-commit-config.yaml` in RStudio, you
have to enable “Show Hidden Files” in the *Files* Pane of RStudio under
*More*.

Next, run `pre-commit install` in your repo and you are done. The next
time you run `git commit`, the hooks listed in your
`.pre-commit-config.yaml` will get executed before the commit. When any
file is changed due to running a hook, the commit will fail. You can
inspect the changes introduced by the hook and if satisfied, you can
attempt to commit again. If all hooks pass, the commit is made. You can
also [temporarily disable
hooks](https://pre-commit.com/#temporarily-disabling-hooks). If you
succeed, it should look like this:

![](man/figs/screenshot.png)<!-- -->

You can also add other hooks from other repos, by extending the
`.pre-commit-config.yaml` file, e.g. like this:

``` yaml
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v1.2.3
    hooks: 
    -   id: check-added-large-files
```

To update the hook revisions, just run `pre-commit autoupdate`.

# why using hooks?

The goal of pre-commit hooks is to improve the quality of commits. This
is achieved by making sure your commits meet some (formal) requirements,
e.g:

  - that they comply to a certain coding style (with the hook
    `style-files`).
  - that you commit derivatives such as `README.md` or `.Rd` files with
    their source instead of spreading them over multiple commits.
  - and so on.

As all changes enter a repository history with a commit, we believe many
checks should be performed at that point, and not only later on a CI
service. For example, creating auto-commits at a CI service for styling
code creates unnecessary extra commits, as styling can be checked at the
time of committing and is relatively inexpensive.

# why using the pre-commit framework?

Implementing hooks in a framework such as
[pre-commit.com](https://pre-commit.com) has multiple benefits compared
to use simple bash scripts in `.git/hooks`:

  - easily use hooks other people wrote, in bash, R, python and other
    languages. There is a wealth of useful hooks available, most listed
    [here](https://pre-commit.com/hooks.html). For example,
    `check-added-large-files` prevents you from committing big files,
    other hooks validate json or yaml files and so on.
  - No need to worry about dependencies, testing, different versions of
    hooks, file filtering for specific hooks etc. It’s handled by
    pre-commit.
  - Hooks are maintained in *one* place, and you just need a
    `.pre-commit-config.yaml` file. No need to c/p hooks from one
    project to another.

You have an idea for a hook? Please file an issue.
