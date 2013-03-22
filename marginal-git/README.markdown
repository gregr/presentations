# marginal progress beyond the basics of git #

DISCLAIMER: I have no idea what I'm talking about.

---
## commit structure ##

* directed acyclic graph
  * **`git log --graph --oneline --decorate --all`**
  * [example branching model][branching-model]

* branches as named pointers
  * point a branch to something new: **`git help reset`**
  * drop a new one at your current location: **`git branch ill-get-to-this-later`**

* tags as named constant pointers plus metadata
  * by the way, you should probably annotate (-a) your tags
  * **`git help describe`**
  * [lightweight vs. annotated tags][annotated-tags]

* **`git reflog`**
  * even if you point a branch at something new, the commits formerly pointed to still exist (until garbage collected)

---
## incorporating upstream changes ##

* pull may not be the preferred default for synching your work with upstream changes
  * tip: fetch first so that you can review changes before incorporating your work

* suggestion: rebasing is usually cleaner than merging when incorporating upstream commits into a work in progress
  * **warning**: try not to rebase when working with published history; it messes everyone else up

* [about pull --rebase][pull-rebase]
* pull vs. fetch and merge (or rebase)
  * [fetch and merge, don't pull][no-pull]
  * [Linus Torvalds on pulling and rebasing][linus-rebase]

---
## merge vs. rebase ##

### graph structure ###

      A0-A1-A2  master
        \
        B3-B4-B5  *feature

  **`git merge master`**

      A0-A1-A2-C6  *feature
        \      /
        B3-B4-B5

  **`git rebase master`**

      A0-A1-A2  master
              \
              B3'-B4'-B5'  *feature

### conflict resolution ###

* with merge
  * single: one (possibly large) conflict to resolve
  * easy to revert a merged set of commits
* with rebase
  * incremental: deal with conflicts on a commit-by-commit basis
  * harder to revert commits incorporated by rebasing

improved conflict markers:  **`git config --global merge.conflictstyle diff3`**

moral: due to revert difficulty, only use rebase when individual commits are consistent independent of their successors

### interactive rebase ###

commit history is communication: keep it clean

* reorder commits to logically group ideas and show coherent progression
* condense spurious details and mistakes
* split commits up into logically independent chunks
  * **`git add -p`**
  * **`git reset -p`**
* improve commit messages after the fact

---
## it's distributed ##

* remotes
  * transporting new commits to test site (submodule in another project)

---
## bisect example ##

* start, bad, good, reset
* visualize, log?
* btw, bisect enters merges

---
## fugitive ##

* [fugitive vim plugin webcasts][vimcasts] (search text for 'fugitive')
  * be sure not to miss these: status, diff, grep, blame, exploring commit objects/history
* programmable editors are awesome

---
## git configuration ##

**`git config -l`**

if you're interested in pretty graph logs, note the log aliases

---
## parting advice ##

options with implicit but dramatic effects are dangerous: do not make them your defaults!

* examples:
  * **commit --all**
  * **push --all**

[linus-rebase]: http://www.mail-archive.com/dri-devel@lists.sourceforge.net/msg39091.html
[no-pull]: http://longair.net/blog/2009/04/16/git-fetch-and-merge/
[annotated-tags]: http://stackoverflow.com/questions/4971746/why-should-i-care-about-lightweight-vs-annotated-tags
[branching-model]: http://nvie.com/posts/a-successful-git-branching-model/
[pull-rebase]: http://stackoverflow.com/questions/6284887/whats-the-difference-between-git-fetch-then-git-rebase-and-git-pull-reb/11531502#11531502
[vimcasts]: http://vimcasts.org/episodes/archive
