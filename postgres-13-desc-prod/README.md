# Prod Deployment of Postgres 13 plus [pgbouncer](https://www.pgbouncer.org/)

Based on [NERSC's spin-recipes](https://github.com/NERSC/spin-recipes/tree/master/postgres)

## How To Update the Postgres Deployment in Spin
See: [https://docs.nersc.gov/services/spin/rancher1/getting_started/lesson-3/#example-4-upgrade-the-image-used-in-a-stack](https://docs.nersc.gov/services/spin/rancher1/getting_started/lesson-3/#example-4-upgrade-the-image-used-in-a-stack)

* Log into Cori
* `module load spin`
* `export RANCHER_ENVIRONMENT=prod-cattle`
* git clone this repo and make changes to docker-compose as needed
* `rancher up --upgrade`
* If all looks good, confirm the upgrade
`rancher up --upgrade --confirm-upgrade`


## Deployment updates

* April 2021 adding pgbouncer support and moved to postgres 13
* March 2021 Added explicit setting of max_connections = 500 as part of start up config

## Initial Set up

* `pg_dump` on postgres-desc-dev to download Jim's initial tests using Postgres 12.4
   -  `shifter --image=postgres:13-alpine pg_dump -h db.postgres-desc-dev.dev-cattle.stable.spin.nersc.org -U gen3_devel -p 5432 -d desc_dm_gen3_spin -f ./mydump.sql`
* Stood up the Postgres 13 service on RANCHER_ENVIRONMENT=prod-cattle without pgbouncer service
  - comment out the service in the docker-compose and rancher-compose yamls and enable port 5432 for the db service 
* Set up desc_dm_gen3_spin DB and enable btree_gist  
  - `CREATE EXTENSION IF NOT EXISTS btree_gist;`
* Dump data into new DB: 
  - `shifter --image=postgres:13-alpine psql -h db.postgres-13-desc-prod.prod-cattle.stable.spin.nersc.org -U gen3_devel -p 5432 -d desc_dm_gen3_spin -f ./mydump.sql`
* Now upgrade the deployment after modifying the yamls to use pgbouncer`


### postgres -- Notes from NERSC's original spin-recipes github repo

This recipe shows a working example of Postgres on Spin using the official managed image
with an automated backup routine.

Features of this recipe:
  * Persistent storage on Rancher NFS.
  * Login credentials on Rancher Secrets.
  * Optional direct access vi SQL monitor from within NERSC networks.
  * Custom configuration options supplied through runtime arguments, avoiding the
    need to create (and maintain) a custom image.
  * Health checking and automatic rescheduling in the event of problems.
  * Enhanced security through a reduced set of capabilities.

To use this recipe, incorporate it into your `docker-compose.yml` and `rancher-compose.yml`,
substituting the appropriate values as described in the comments.
