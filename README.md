Ansible Role Squid
==================

Installs latest squid on ubuntu 16.04 & CentOs 7

Example Playbook
----------------

install server and config apt / yum:

    - hosts: squid-servers
      roles:
         - shelleg.squid
         
just config clients:

    - hosts: squid-servers
      roles:
         - shelleg.squid
           squid_server_mode: false
           squid_proxy_port: 3148                # your custom proxy port
           proxy_server: <your proxy host or ip> # do not specify port !
           
Default vars
------------
```
squid_server_mode: true # install server
squid_yum: true         # configure yum proxy & disable fast mirror
squid_apt: true         # configure apt proxy
squid_proxy_port: 8080  # the defult sqiod listen port

```


License
-------

MIT

Author Information
------------------

Haggai Philip Zagury -> Tikal Knowledge
