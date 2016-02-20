Couchbase Sync Gateway Ansible Role
===================================

This is a role to install [Couchbase Sync Gateway][sync-gateway] with [Ansible][] scripts.
You can combine it with Couchbase's [server install scripts][ansible-server] found on GitHub.
[Here's](http://developer.couchbase.com/documentation/mobile/1.2/get-started/sync-gateway-overview/index.html) a nice overview over the Sync Gateway.


Configuration
-------------

The default settings install version 1.2 and configure sync gateway to talk to a local [Couchbase Server][couchbase-server] installation on the default ports against a `sync_gateway` bucket.
_Nginx_ will be installed to reverse-proxy requests to sync gateway.
The special **GUEST** user will be enabled on the _public_ channel.
You can change these settings by redefining any of the following variables:

    couchbase_sync_gateway_version: 1.2.0
    couchbase_sync_gateway_package: couchbase-sync-gateway-community_1.2.0-79_x86_64.deb
    couchbase_sync_gateway_port: 4984
    couchbase_sync_gateway_admin_port: 4985
    couchbase_sync_gateway_users:
      - name: GUEST
        admin_channels:
          - public
    
    # it seems this is created automatically when installing Sync Gateway 1.2, know what you do when you change these!
    couchbase_sync_gateway_user:
      name: sync_gateway
      home: /home/sync_gateway
    
    # Nginx
    couchbase_sync_gateway_nginx_server: localhost
    couchbase_sync_gateway_nginx_port: 4999
    
    # Server
    couchbase_server_url: http://localhost
    couchbase_server_admin_port: 8091
    couchbase_server_bucket_name: sync_gateway

The file `couchbase-sync-gateway.json.j2` is the configuration file the gateway will use when starting, you can provide your own if you so desire.
Furthermore, the file `couchbase-sync-gateway.js` contains the sync function, supply your own to specify your sync function.


Running
-------

It seems that with version 1.2, Sync Gateway is automatically set up to always run.
The script uses the `sync_gateway` user and uses `/home/sync_gateway/sync_gateway.json` as the config file.
This role will place your settings file at that location.

#### Outdated:

After installation and configuration, a script named `run_gateway.sh` is placed in the specified user's home directory (`sync_gateway` by default).
You probably want to setup _supervisor_ or similar to take care of launching your Sync Gateway.



[sync-gateway]: http://developer.couchbase.com/mobile/develop/guides/sync-gateway/
[ansible]: http://docs.ansible.com
[ansible-server]: https://github.com/couchbaselabs/ansible-couchbase-server
[couchbase-server]: http://www.couchbase.com/nosql-databases/couchbase-server
