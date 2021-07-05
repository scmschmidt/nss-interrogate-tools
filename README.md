# nss-interrogate-tools
Tools to interrogate NSS modules.

Lately we disussed among colleagues how to find out if a user is in the local database, LDAP, AD or what ever configured database.
It seems that it is not possible to retrieve this information with current tools, so I tried to figure out a way.
As a result the first draft of two tools:

```
    getuser USERNAME NSSMODULE
```

The script checks if a given user is in a given database (NSS).<br>
It mounts an individual nsswitch.conf in a shell with an unshared mount namespace, where only the given NSS module is listed for 'passwd:'

Example:

```
    # ./getuser root compat
    uid=0(root) gid=0(root) Gruppen=0(root)
```

and

```
    finduser USERNAME
```

The script tries to figure out which of the configured NSS modules can resolve the given user.<br>
 It iterates through all configured passwd modules in nsswitch.conf and for each entry it mounts an individual nsswitch.conf in a shell with an unshared mount namespace to check if the user can be found.

Example:

```
    # ./finduser root
    compat:  uid=0(root) gid=0(root) Gruppen=0(root)

```
 
Both tools could need a bit polishing, but in principle they work.

