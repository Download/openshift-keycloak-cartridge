# WildFly with Keycloak on MySQL Cartridge

This cartridge installs a WildFly 10 instance (modeled after [openshift-wildfly-cartridge](https://github.com/openshift-cartridges/openshift-wildfly-cartridge)) 
and adds Keycloak to it (modeled after [openshift-keycloak-cartridge](https://github.com/keycloak/openshift-keycloak-cartridge)).

## Changes
This cartridge changes a few things compared to the `openshift-keycloak-cartridge`:

* Use the connected MySQL database, not the built-in H2 database.
* Use a distributed cache i.s.o. a local cache

This should make this cartridge fully scaleable on OpenShift. You can include your own app 
just like you would do with the regular WildFly cartridge. 

> Do be aware that this cartridge connects to *the same database* as your regular app. 
This means the Keycloak tables will be added to the database. Personally I think this is an
advantage because it allows my app to query the users tables easily. However if you rather 
keep the Keycloak tables separate, make a new schema for keycloak and alter the config for 
`KeycloakDS` in `standalone.xml` to use the keycloak schema i.s.o the default schema.

## Installation

You can create a new application using this cartridge with the following command:

```sh
rhc app create myapp http://cartreflect-claytondev.rhcloud.com/github/download/wildfly-keycloak-mysql-cartridge mysql-5.5 -s
```

This will create a new application with two connected gears. The web gear will run 
WildFly 10.1.0.Final with Keycloak 2.5.1.Final embedded. MySQL 5.5 will be installed 
on the second gear and Keycloak will add it's tables and data to the MySQL database.

> It will take a few minutes to build, so be patient.
