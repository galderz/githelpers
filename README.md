# githelpers 

This is a set of scripts that help perform some common tasks
interacting with [Infinispan](http://www.infinispan.org)'s [git repository](http://github.com/infinispan/infinispan) setup, often
causing contributors to fork an upstream repository and work
off the fork, while keeping in sync with the upstream repo.

Scripts also exist to help project admins pull in contributions
via pull requests.

While these scripts have been written for Infinispan, they can
easily be adapted for use with other projects as well, that
follow similar processes.

## contributors

This directory contains scripts commonly used by contributors
to Infinispan.

### remove_topic_branch

This script removes a topic branch after it has been pulled into
upstream.  This should be run on a _local_ clone of your fork,
and it removes the topic branch on your local clone _as well as_
your remote fork.  

E.g.,

``$ remove_topic_branch t_ISPN-12345``

### sync_with_upstream

This script synchronizes your release branches (master and 4.2.x) 
with upstream, pulling in new updates.  Also, if you have any topic 
branches, these too will be rebased accordingly so that your topic
branches are up to date.  It _must_ be run in the directory containing
a local clone of your fork.

 * This script _guesses_ which release branch your topic branches
are based off, and rebases accordingly.  The guess is based on scanning
commit histories for common points.  _This is experimental!_
 * For topic branches to be rebased, your topic branch name _must_ start
 with ``t_``.  Edit the script to change this.
 
E.g.,

``$ sync_with_upstream``

## project_admins

This contains additional scripts used by project admins - folks
with push privileges on the upstream repo - and would be used
to supplement the scripts in ``contributors``.

### pull_fast_forward

This script handles a pull request, and _must_ be run in a _local_ clone of _upstream_.  (*Not* a clone of your personal fork!)  It takes in some parameters:

 * URL or remote repository name
 * Topic branch name on remote repo
 * Release branch to merge into
 
By default, if the topic branch _cannot_ be fast-forwarded, the script
aborts and instructs the admin to contact the contributor to 
sync with upstream and re-submit the pull request.

This behaviour can be overridden by passing in the -f flag at the end.

E.g.,

``$ pull_fast_forward https://maniksurtani@github.com/maniksurtani/infinispan.git t_ISPN-1234 master``

``$ pull_fast_forward vlads_repo t_ISPN-1234 master -f``

### update_release_branches

This script, when run in a local clone of upstream, will pull down changes from 
upstream onto the release branches so you are in sync with other project admins.

E.g.,

``$ update_release_branches``

Name specific branches to update:

``$ update_release_branches 4.2.x master``

# Installation

Very simple - copy the lot to ``${HOME}/bin``.  The scripts are all 
written in [BASH][] or [Python][] and should 
be executable (``chmod 755 *.sh``).
 
[BASH]: http://en.wikipedia.org/wiki/Bash_(Unix_shell) "BASH"
[Python]: http://www.python.org "Python"