# Prod Deployment of Postgres 13 plus [pgbouncer](https://www.pgbouncer.org/)

Based on [NERSC's spin-recipes](https://github.com/NERSC/spin-recipes/tree/master/postgres)

## How To Update in Spin
See: [https://docs.nersc.gov/services/spin/rancher1/getting_started/lesson-3/#ex
ample-4-upgrade-the-image-used-in-a-stack](https://docs.nersc.gov/services/spin/
rancher1/getting_started/lesson-3/#example-4-upgrade-the-image-used-in-a-stack)

* Log into Cori
* module load spin
* export RANCHER_ENVIRONMENT=prod-cattle
* git clone this repo and make changes to docker-compose as needed
* rancher up --upgrade
* If all looks good, confirm the upgrade
rancher up --upgrade --confirm-upgrade


## Updates

* April 2021 adding pgbouncer support and moved to postgres 13
* March 2021 Added explicit setting of max_connections = 500 as part of start up config


### postgres

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
