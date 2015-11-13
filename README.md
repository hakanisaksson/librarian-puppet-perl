# NAME

**librarian-puppet-perl** - Fetch and update puppet modules from git to Directory Environments configuration.

# SYNOPSIS

librarian-puppet-perl &lt;options> action

**actions:**

    clean = remove cache and modules
    convert = convert librarian-puppet file to YAML
    fetch = fetch git repos but don't update modules
    list = list modules and versions
    update = update/install modules

# OPTIONS

- **--debug**

    print debugging info

- **--environment**

    limit environment, by default this script acts on all environments found

- **--help**

    print the help text

- **--man**

    print the complete documentation

- **--test**

    run in test mode only

- **--verbose**

    print mode information

# DESCRIPTION

**librarian-puppet-perl** Update puppet modules from selected Git repositories.
Repositories need to be listed in a YAML-format textfile called Puppetfile.yaml
librarian-puppet-perl is a replacement for librarian-puppet that is simpler and more robust. It does not pend on specific ruby version, it's made in Perl. Expects that you tag your versions properly in Git, but works even if you don't. Does not depend on the module containing a Puppetfile or metadata.json to display version information about the module. It does not automatically download module dependencies.
Uses YAML format for configuration files instead of ruby format that breaks if you forget a comma.
Maintains a local cache and uses rsync to update the actual configuration to minimize the time when the filesystem is changed.
Expects puppet to be configured with Directory Environments, but would work with a single environment too.
Any sub-directory with a Puppetfile.yaml is considered to be a Puppet Environment.
This script is inspired by puppet-librarian by rodjek, but a total rewrite that uses rsync to ensure changes to modules have minimal impact on a running environment. It also don't have a crap ton of gem dependencies.

Puppetfile.yaml format example:

    FORGE: 'ssh://git.repo.com/git/puppet/modules'
    MODULES:
    - mod: base
      git: git://git.repo.com/git/puppet/modules/base.git
      ref: development
    - mod: ldap
      ref: 1.0.0
    - mod: ntp

**Installation**
    Ensure that perl-YAML is installed
    Create Puppetfile.yaml
    Run librarian-puppet-perl update

**Configuration**
   librarian-puppet-perl creates a file librarian-puppet-perl.yaml in the directory where it is runnig if it does not exist. Edit the file to change defaults. Defaults can be overridden on the commandline.

**Example librarian-puppet-perl.yaml**:

   ---
   AUTOCLEAN: 1
   DEBUG: 0
   LOGDIR: /var/log/puppet
   MODULEDIR: /etc/puppet/environments
   REPOLIST: Puppetfile.yaml
   RSYNC: rsync -a --delete --exclude .git
   TEST: 0
   TMPDIR: /etc/puppet/environments/.librarian-puppet-perl
