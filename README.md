Ansible Role Squid
==================

Installs latest squid on ubuntu 16.04 & CentOs 7

Example Playbook
----------------

install server only:

    - hosts: squid-servers
      roles:
         - shelleg.squid
         
config clients -> see [ansible-role-proxy-client]()

    - hosts: squid-servers
      roles:
         - shelleg.squid
           squid_server_mode: false
           squid_proxy_port: 3148                # your custom proxy port
           
Default vars
------------
```
squid_server_mode: true # install server
squid_proxy_port: 8080  # the defult sqiod listen port

```


License
-------

MIT

Author Information
------------------

Haggai Philip Zagury -> Tikal Knowledge
