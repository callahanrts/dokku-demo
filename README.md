# Dokku Demo

## Setup

### Dependencies
- Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
- Install [Vagrant](https://www.vagrantup.com/docs/installation/)

### Host Machine Setup
1. Add `192.168.33.10 microservices.local` to your `/etc/hosts`
1. Clone this repository
1. Start the vm: `vagrant up`
1. SSH  into the vm: `vagrant ssh`

### Guest Machine Setup

1. Install [Dokku](http://dokku.viewdocs.io/dokku/getting-started/installation/)
    - `wget https://raw.githubusercontent.com/dokku/dokku/v0.7.1/bootstrap.sh`
    - `sudo DOKKU_TAG=v0.7.1 bash bootstrap.sh`

1. Navigate to [microservices.local](http://microservices.local/)
1. Copy your public key (output of `cat ~/.ssh/id_rsa.pub`) to the Dokku Setup.
1. Change the hostname to `microservices.local`
1. Check `use virtualhost naming for apps`
1. Finish Setup

## Test Out a Sample App

_Follow the instructions of the [deploy tutorial](http://dokku.viewdocs.io/dokku/deployment/application-deployment/) or:_

##### Guest (Dokku) Machine

```bash
$ dokku apps:create ruby-rails-sample
$ sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git
$ dokku postgres:create rails-database
$ dokku postgres:link rails-database ruby-rails-sample
```

##### Host Machine
`/etc/hosts` doesn't support wildcards so add `192.168.33.10 ruby-rails-sample.microservices.local`
to your `/etc/hosts`. In production, we would point `*.microservices.com`, or similar, to our server.
```bash
$ cd ruby-rails-sample
$ git remote add dokku dokku@microservices.local:ruby-rails-sample
$ git push dokku master
```
Navigate to [http://ruby-rails-sample.microservices.local](http://ruby-rails-sample.microservices.local)
