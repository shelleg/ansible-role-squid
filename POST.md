---
layout: post
published: true
title: Speed up your deveopment cycle(s) with Squid caching server
author: hagzag
permalink: devops/squid-4-speed
---
## Problem

Developing infrastructure as code requires downloading 100's of Mbytes
and some time Gigabytes of data just to get going.

## Solution

Run a local cache of all the packages you need for development
(especially when you are building a multi-platform solution).
The cool thing here is that with under a minute from the time the
machine is booted you have a "lazy cacher" which only downloads what you
actually consume.

As almost everything I am doing nowadays is done via
[Ansible](http://www.ansible.com), iv'e developed a [small
role](https://github.com/shelleg/ansible-role-squid) to do just that !
Install squid on either Ubuntu 16.04 or CentOs 7.

## Setting up squid on Centos / Ubuntu

```python
git clone git@github.com:shelleg/ansible-role-squid.git 
cd ansible-role-squid
```
- running ubuntu flavored squid -> `vagrant provision squid-ubuntu`
- running centos flavored squid -> `vagrant provision squid-centos`

## apt/yum caching included ("batteries included" :))...

The role has both yum & apt proxying support see the
`squid-config-client.yml` tasks file, which will default to `127.0.0.1`
as the proxy host unless you pass the `proxy_server` ansible var in your
playbook.
So if you were to setup proxy and wish to configure a different project
to use it you should have a playbook like the following example:
    
```python
- hosts: custom-jenkins
  become: true
  vars:
    squid_server_mode: false            # Don't install proxy it's on
another machine ...
    proxy_server: "172.16.1.99"         # Any other IP it may reside on
...
    noproxy_hosts:                      # Hosts you do not want to proxy
...
      - '127.0.0.1'
      - '172.16.1.*'
  roles:
    - role: squid                       # This role
    - role: oracle-java
    - role: epel
    - role: git
    - role: ansible-role-custom-jenkins

```

**Please note:** The `squid-config.client.yml` will most defintelly work
with a different proxy implementation which means you could potentially
use this role to setup your own non-squid or other squid proxy
implementation.
(I plan to add system wide proxy, wger, curl support in the near
future).

## Caveats

I set the Squid cach to "5000" MB - if you are planning on using this in
productio you may consider adding more space in addition to add some
ACL's to squid configuration (plan to do those if we move into
production with this!)

## Before I conclude -> OMG !!! what sould happen with vagrant destroy
-f ? hang on !

vagrant destroy -f is brutal and might set you back ... there is 2
things you should do in order to prevent that:

1. Install the `vagrant-protect` plugin `vagrant install
   vagrant-protect` which is already configured in the included Vagrant
file:
```ruby
   if Vagrant.has_plugin?("vagrant-protect")
          srv.protect.enabled = servers["protected"]
   end
```
2. Add the protected boolean in `servers.yml` see line #9 in the
   servers.yml which is also emphesized below
    
```python
    - name: squid-centos
      box: centos/7
      ram: 768
      ip: 172.16.1.140
      # when used for development I do not want to accedentelly remove
it ...
      # hence I am using vagrant protect plugin
(https://github.com/ryuzee/vagrant-protect)
      protected: true
```

As always feedbak (good|bad) is welcome.
[Issues](https://github.com/shelleg/ansible-role-squid/issues) are
welcome [here](https://github.com/shelleg/ansible-role-squid/issues)
& you can always comment of this post of course ... I will be more than
happy to hear this helped you out.

Until Next time ...

Haggai Philip

