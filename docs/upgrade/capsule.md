---
layout: documentation
title: Capsule Upgrade
sidebar: sidebars/documentation.html
---

# Capsule Upgrade

Katello supports upgrades from version 2.0.  For users transitioning from 1.4, please see - [Transition Guide](/docs/installation/2.0-transition.html).

## Step 1 - Operating System

Ensure your operating system is fully up-to-date:

```
# yum -y update
```

## Step 2 - Repositories

Update the Foreman and Katello release packages:

  * RHEL6 / CentOS 6:

```
  # yum update -y http://fedorapeople.org/groups/katello/releases/yum/nightly/katello/RHEL/6Server/x86_64/katello-repos-latest.rpm
  # yum update -y http://yum.theforeman.org/nightly/el6/x86_64/foreman-release.rpm
```

  * RHEL7 / CentOS 7:

```
  # yum update -y http://fedorapeople.org/groups/katello/releases/yum/nightly/katello/RHEL/7Server/x86_64/katello-repos-latest.rpm
  # yum update -y http://yum.theforeman.org/nightly/el7/x86_64/foreman-release.rpm
```

## Step 3 - Update Packages

Clean the yum cache

```
# yum clean all
```

Update packages:

```
# yum update -y
```

## Step 4 - Regenerate Certificates

On the Katello server, regenerate the certificates tarball for your capsule:

```
# capsule-certs-generate --capsule-fqdn "mycapsule.example.com"\
                         --certs-tar    "~/mycapsule.example.com-certs.tar"
```

And copy them to your capsule:

```
# scp ~/mycapsule.example.com-certs.tar mycapsule.example.com:
```

## Step 5 - Run Installer

The installer with the --upgrade flag will run the right database migrations for all component services, as well as adjusting the configuration to reflect what's new in Katello {{ site.version }}

```
# capsule-installer --upgrade --certs-tar ~/mycapsule.example.com-certs.tar\
                    --certs-update-all --regenerate --deploy
```

**Congratulations! You have now successfully upgraded your Capsule to {% if site.version %}{{ site.version }} For a rundown of what was added, please see [release notes](/docs/{{ site.version }}/release_notes/release_notes.html).{% else %}the latest nightly{% endif %}!**

If for any reason, the above steps failed, please review /var/log/capsule-installer/capsule-installer.log -- if any of the "Upgrade step" tasks failed, you may try to run them manaully below to aid in troubleshooting.

## Manual Steps

**Pulp**

```
# sudo -u apache pulp-manage-db
```

