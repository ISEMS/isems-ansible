# ISEMS Ansible

This repository contains ansible configuration management code for easier deployment of the ISEMS app(s).

There are two configurations: `isems.yml` for local Raspberry-PI based deployments (this is what you 
most probably want to use) and `public.yml` which is used for the demot at `app.isems.de`.

You can edit these files or use them as a template for your configuration.

The shipped-configuration contains one enctypted variable (`sentry_dsn`). This is for integration with
getsentry.io and contains our secret key. To decrypt it you need the corresponding password. To be 
prompted for it run:
```
 ansible-playbook --vault-id @prompt isems.yml -i hosts
```

For your installation you can simply remove the variable completely if you do not need sentry or
replace it with your DSN key.


Generally, to deploy to a local Raspberry PI, edit the IP address in `hosts`, remove the `sentry_dsn` variable 
from `isems.yml` (also remove the `sync` task from there since you do not want to sync to a public instance) and run:
```
 ansible-playbook isems.yml -i hosts
```
