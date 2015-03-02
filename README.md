Couchbase Sync Gateway Ansible Role
===================================

This is a role to install [Couchbase Sync Gateway][sync-gateway] with [Ansible][] scripts.
You can combine it with Couchbase's [server install scripts][ansible-server] found on GitHub.


Configuration
-------------

The default settings install version 1.0.3 and configure sync gateway to talk to a local [Couchbase Server][couchbase-server] installation on the default ports against a `sync_gateway` bucket.
_Nginx_ will be installed to reverse-proxy requests to sync gateway.
The special **GUEST** user will be enabled on the _public_ channel.
You can change these settings by redefining any of the following variables:

    couchbase_sync_gateway_package: couchbase-sync-gateway-community_1.0.3_x86_64.deb
    couchbase_sync_gateway_url: http://packages.couchbase.com/releases/couchbase-sync-gateway/1.0.3/{{ couchbase_sync_gateway_package }}
    couchbase_sync_gateway_sha256: ""
    
    couchbase_sync_gateway_user:
      name: cbsync
      home: /home/cbsync
    couchbase_sync_gateway_port: 4984
    couchbase_sync_gateway_admin_port: 4985
    couchbase_sync_gateway_settings: couchbase-sync-gateway.json
    couchbase_sync_gateway_users:
      - name: GUEST
        admin_channels:
          - public
    
    # Nginx
    couchbase_sync_gateway_nginx_server: localhost
    couchbase_sync_gateway_nginx_port: 4999
    
    # Server
    couchbase_server_bucket_name: sync_gateway
    couchbase_server_admin_port: 8091

The file `couchbase-sync-gateway.json.j2` is the configuration file the gateway will use when starting, you can provide your own if you so desire.
Furthermore, the file `couchbase-sync-gateway.js` contains the sync function, you can supply your own to specify your sync function.


Running
-------

After installation and configuration, a script named `run_gateway.sh` is placed in the specified user's home directory (`cbsync` by default).
You probably want to setup _supervisor_ or similar to take care of launching your Sync Gateway.



[sync-gateway]: http://developer.couchbase.com/mobile/develop/guides/sync-gateway/
[ansible]: http://docs.ansible.com
[ansible-server]: https://github.com/couchbaselabs/ansible-couchbase-server
[couchbase-server]: http://www.couchbase.com/nosql-databases/couchbase-server
