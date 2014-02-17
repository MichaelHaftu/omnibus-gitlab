# GitLab Omnibus project

This project creates full-stack platform-specific packages for
GitLab!

## Installation

After the steps below your GitLab instance should reachable over HTTP,
and have an admin user with username `root` and password `5iveL!fe`.

### Ubuntu 12.04

```
sudo apt-get install openssh-server
sudo apt-get install postfix # sendmail or exim is also OK
sudo dpkg -i gitlab-x.y.z.deb # this is the .deb you downloaded
sudo gitlab-ctl reconfigure
```

### CentOS 6.5

```
sudo yum install openssh-server
sudo yum install postfix # sendmail or exim is also OK
sudo rpm -i gitlab-x.y.z.rpm
sudo gitlab-ctl reconfigure
# Open up the firewall for HTTP and SSH
sudo lokkit -s http -s ssh
```

## How to manage an Omnibus-installed GitLab

### Start/stop GitLab

You can start, stop or restart GitLab and all of its components with the
following commands.

```shell
# Start all GitLab components
sudo gitlab-ctl start

# Stop all GitLab components
sudo gitlab-ctl stop

# Restart all GitLab components
sudo gitlab-ctl restart
```

It is also possible to start, stop or restart individual components.

```shell
sudo gitlab-ctl restart unicorn
```

### Creating the gitlab.rb configuration file

```shell
sudo mkdir -p /etc/gitlab
sudo touch /etc/gitlab/gitlab.rb
sudo chmod 600 /etc/gitlab/gitlab.rb
```

### Configuring the external URL for GitLab

In order for GitLab to display correct repository clone links to your users
it needs to know the URL under which it is reached by your users, e.g.
`http://gitlab.example.com`. Add the following line to `/etc/gitlab/gitlab.rb`:

```ruby
external_url "http://gitlab.example.com"
```

Run `sudo gitlab-ctl reconfigure` for the change to take effect.

### Invoking Rake tasks

To invoke a GitLab Rake task, use `gitlab-rake`. For example:

```shell
sudo gitlab-rake gitlab:backup:create
```

Contrary to with a traditional GitLab installation, there is no need to change
the user or the `RAILS_ENV` environment variable; this is taken care of by the
`gitlab-rake` wrapper script.

### Directory structure

Omnibus-gitlab uses four different directories.

- `/opt/gitlab` holds application code for GitLab and its dependencies.
- `/var/opt/gitlab` holds application data and configuration files that
  `gitlab-ctl reconfigure` writes to.
- `/etc/gitlab` holds configuration files for omnibus-gitlab. These are
  the only files that you should ever have to edit manually.
- `/var/log/gitlab` contains all log data generated by components of
  omnibus-gitlab.

## Building your own package

See [the separate build documentation](doc/build.md).
