# Ansible Role - Yum Client
This Ansible Role is used for configuring the .repo files in /etc/yum.repos.d to allow CentOS/RHEL clients to install/update from custom yum repositories.

Useful in disconnected environments with no internet access

---- 

### TODO
* Do gpgcheck properly 

### Known Issues
* After running a yum update on a fresh CentOS install, the default .repo files are recreated
    * This is solved by rerunning some code in the update.yml task to remove .repo files and recreate the .repo template again.

----
## Role Variables

Configure this role to purge ALL .repo files in /etc/yum.repos.d directory. This is pretty useful if you want all of your CentOS servers to have clean directories to make it easier to manage your repo files.

```
# Default: false
yum_repo_remove_old_repo_files: true
```

Configure this role to perform a yum update, this will update ALL packages on the CentOS server.
```
# Default: false
yum_update_packages: true
```

Configure the name and directory for the .repo files to be in
```
yum_repo_file_name: rhel.repo

# This will create /etc/yum.repos.d/rhel.repo
```

Configure the standard CentOS repository URLS
```
yum_repo_host_url: https://yum.example.com/centos
# That will become https://yum.example.com/centos/$releasever/os/$basearch/ for example

# Enable/disable which ones you want
# Defaults:
yum_repo_enable_base: 1
yum_repo_enable_updates: 1
yum_repo_enable_extras: 0
yum_repo_enable_centosplus: 0
yum_repo_enable_epel: 0
```

Configure the EPEL URL
```
epel_repo_host_url: https://epel.example.com/epel

# Which will become https://epel.example.com/epel/$releasever/$basearch/
```

Configure whether or not to enable gpgchecks
```
# Default is 0 (disabled)
yum_enable_gpgcheck: 1
```

Configure extra custom repos, for example Docker-CE
```
yum_repo_custom_repos: []
  - name: docker-ce
    description: Docker CE Stable - $basearch
    base_url:  "https://docker.example.com/docker-ce/centos/$releasever/$basearch/stable"
    enabled: 1
    gpgcheck: 0
```

Configure Yum-cron
```
# Enable or Disable, default is disabled
yum_cron_enabled: true

#  What kind of update to use:
# default                            = yum upgrade
# security                           = yum --security upgrade
# security-severity:Critical         = yum --sec-severity=Critical upgrade
# minimal                            = yum --bugfix update-minimal
# minimal-security                   = yum --security update-minimal
# minimal-security-severity:Critical =  --sec-severity=Critical update-minimal
yum_cron_update_cmd: default

# Whether a message should be emitted when updates are available,
# were downloaded, or applied
yum_cron_update_messages: "yes"

# Whether updates should be downloaded when they are available.
yum_cron_download_updates: "yes"

# Whether updates should be applied when they are available.  Note
# that download_updates must also be yes for the update to be applied.
yum_cron_apply_updates: "yes"
```

----
