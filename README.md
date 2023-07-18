# U-Bahn API

## Install software

- node 12.x+
- npm 6.x+
- docker
- elasticsearch 7.7+
- PostgreSQL

## Configuration

Configuration for the application is at config/default.js and config/production.js. The following parameters can be set in config files or in env variables:

- LOG_LEVEL: the log level
- PORT: the server port
- API_BASE_URL: the api base url, default to 'http://127.0.0.1:3001/api/1.0'
- AUTOMATED_TESTING_NAME_PREFIX: the prefix for the records from db, default to 'POSTMANE2E-',
- CASCADE_PAUSE_MS: how many milliseconds to pause between deleting records during cascade delete (default to 1000)
- AUTH_SECRET: TC Authentication secret
- VALID_ISSUERS: valid issuers for TC authentication
- PAGE_SIZE: the default pagination limit
- MAX_PAGE_SIZE: the maximum pagination size
- API_VERSION: the API version
- DB_NAME: the database name
- DB_USERNAME: the database username
- DB_PASSWORD: the database password
- DB_HOST: the database port
- DB_PORT: the database port
- AUTH0_URL: Auth0 URL, used to get TC M2M token
- AUTH0_AUDIENCE: Auth0 audience, used to get TC M2M token
- TOKEN_CACHE_TIME: Auth0 token cache time, used to get TC M2M token
- AUTH0_CLIENT_ID: Auth0 client id, used to get TC M2M token
- AUTH0_CLIENT_SECRET: Auth0 client secret, used to get TC M2M token
- AUTH0_PROXY_SERVER_URL: Proxy Auth0 URL, used to get TC M2M token
- BUSAPI_URL: Topcoder Bus API URL
- KAFKA_ERROR_TOPIC: The error topic at which bus api will publish any errors
- KAFKA_MESSAGE_ORIGINATOR: The originator value for the kafka messages
- UBAHN_CREATE_TOPIC: Kafka topic for create message
- UBAHN_UPDATE_TOPIC: Kafka topic for update message
- UBAHN_DELETE_TOPIC: Kafka topic for delete message
- UBAHN_AGGREGATE_TOPIC: Kafka topic that is used to combine all create, update and delete message(s)
- ES_HOST: Elasticsearch host
- ES.DOCUMENTS: Elasticsearch index, type and id mapping for resources.
- ATTRIBUTE_GROUP_PIPELINE_ID: The pipeline id for enrichment with attribute group. Default is `attributegroup-pipeline`
- SKILL_PROVIDER_PIPELINE_ID: The pipeline id for enrichment with skill provider. Default is `skillprovider-pipeline`
- USER_PIPELINE_ID: The pipeline id for enrichment of user details. Default is `user-pipeline`
- ATTRIBUTE_GROUP_ENRICH_POLICYNAME: The enrich policy for attribute group. Default is `attributegroup-policy`
- SKILL_PROVIDER_ENRICH_POLICYNAME: The enrich policy for skill provider. Default is `skillprovider-policy`
- ROLE_ENRICH_POLICYNAME: The enrich policy for role. Default is `role-policy`
- ACHIEVEMENT_PROVIDER_ENRICH_POLICYNAME: The enrich policy for achievement provider. Default is `achievementprovider-policy`
- SKILL_ENRICH_POLICYNAME: The enrich policy for skill. Default is `skill-policy`
- ATTRIBUTE_ENRICH_POLICYNAME: The enrich policy for skill. Default is `attribute-policy`
- ELASTICCLOUD_ID: The elastic cloud id, if your elasticsearch instance is hosted on elastic cloud. DO NOT provide a value for ES_HOST if you are using this
- ELASTICCLOUD_USERNAME: The elastic cloud username for basic authentication. Provide this only if your elasticsearch instance is hosted on elastic cloud
- ELASTICCLOUD_PASSWORD: The elastic cloud password for basic authentication. Provide this only if your elasticsearch instance is hosted on elastic cloud
- MAX_BATCH_SIZE: Restrict number of records in memory during bulk insert (Used by the db to es migration script)
- MAX_RESULT_SIZE: The Results Per Query Limits. Default is `1000` (Used by the db to es migration script)
- MAX_BULK_SIZE: The Bulk Indexing Maximum Limits. Default is `100` (Used by the db to es migration script)

For `ES.DOCUMENTS` configuration, you will find multiple other configurations below it. Each has default values that you can override using the environment variables

Configuration for testing is at `config/test.js`, only add such new configurations different from `config/default.js`
- WAIT_TIME: wait time used in test, default is 0 or 0 second (5000 stands for 5 seconds)
- AUTH_V2_URL: The auth v2 url
- AUTH_V2_CLIENT_ID: The auth v2 client id
- AUTH_V3_URL: The auth v3 url
- ADMIN_CREDENTIALS_USERNAME: The user's username with admin role
- ADMIN_CREDENTIALS_PASSWORD: The user's password with admin role
- USER_CREDENTIALS_USERNAME: The user's username with user role
- USER_CREDENTIALS_PASSWORD: The user's password with user role
- MANAGER_CREDENTIALS_USERNAME: The user's username with manager role
- MANAGER_CREDENTIALS_PASSWORD: The user's password with manager role
- COPILOT_CREDENTIALS_USERNAME: The user's username with copilot role
- COPILOT_CREDENTIALS_PASSWORD: The user's password with copilot role
- USER_ID_BY_ADMIN: the user id which is created by admin
- USER_ID_BY_TESTER: the user id which is created by a normal user
- PROVIDER_ID_BY_ADMIN: the service provider id which is created by admin
- PROVIDER_ID_BY_TESTER: the service provider id which is created by a normal user
- SKILL_ID_BY_ADMIN: the skill id which is created by admin
- SKILL_ID_BY_TESTER: the skill id which is created by a normal user
- ROLE_ID_BY_ADMIN: the role id which is created by organization
- ROLE_ID_BY_TESTER: the role id which is created by a normal user
- ORGANIZATION_ID_BY_ADMIN: the organization id which is created by admin
- ORGANIZATION_ID_BY_TESTER: the organization id which is created by a normal user
- ACHIEVEMENTS_PROVIDER_ID_BY_ADMIN: the achievement provider id which is created by admin
- ACHIEVEMENTS_PROVIDER_ID_BY_TESTER: the achievement provider id which is created by a normal user
- ATTRIBUTE_GROUP_ID_BY_ADMIN: the attribute group id which is created by admin
- ATTRIBUTE_GROUP_ID_BY_TESTER: the attribute group id which is created by a normal user
- ATTRIBUTE_ID_BY_ADMIN: the attribute id which is created by admin
- ATTRIBUTE_ID_BY_TESTER: the attribute id which is created by a normal user
- ACHIEVEMENT_ID_BY_ADMIN: the achievement id which is created by admin
- ACHIEVEMENT_ID_BY_TESTER: the achievement id which is created by a normal user
- AUTOMATED_TESTING_REPORTERS_FORMAT: The format of output for Newman Automation Testing Report.
## Local deployment

Setup your Elasticsearch instance and ensure that it is up and running.

1. Follow *Configuration* section to update config values, like database, etc ..
2. Goto *UBahn-api*, run `npm i` and `npm run lint`
3. Run the migrations - `npm run migrations up`. This will create the database, the tables.
4. Then run `npm run insert-data` and insert mock data into the database.
5. You will then run `npm run migrate-db-to-es` to migrate the data to elasticsearch from the database
6. Startup server `node app.js` or `npm run start`

## Migrations

Migrations are located under the `./scripts/db/` folder. Run `npm run migrations up` and `npm run migrations down` to execute the migrations or remove the earlier ones

## Import data from QLDB
 
Make sure `QLDB_NAME`, `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` are set in your environment

Run `npm run migrate-qldb-to-pg` to import data from qldb. 

## Import data from S3 to QLDB

Make sure `BUCKET_NAME`, `QLDB_NAME`, `AWS_REGION`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` are set in your environment

Run `npm run import-s3-data` to import data from s3 to qldb. 

## Local Deployment with Docker

Make sure all config values are right, and you can run on local successfully, then run below commands

1. Navigate to the directory `docker`

2. Rename the file `sample.env` to `.env`

3. Set the required AUTH0 configurations, DB configurations and ElasticSearch host in the file `.env`

4. Once that is done, run the following command

    ```bash
    docker-compose up
    ```

5. When you are running the application for the first time, It will take some time initially to download the image and install the dependencies

You can also head into `docker-pgsql-es` folder and run `docker-compose up -d` to have docker instances of pgsql and elasticsearch to use with the api

## Running tests

### Configuration
Test configuration is at `config/test.js`. You don't need to change them.

The following test parameters can be set in config file or in env variables:

- WAIT_TIME: wait time
- AUTH_V2_URL: The auth v2 url
- AUTH_V2_CLIENT_ID: The auth v2 client id
- AUTH_V3_URL: The auth v3 url
- ADMIN_CREDENTIALS_USERNAME: The user's username with admin role
- ADMIN_CREDENTIALS_PASSWORD: The user's password with admin role
- USER_CREDENTIALS_USERNAME: The user's username with user role
- USER_CREDENTIALS_PASSWORD: The user's password with user role
- COPILOT_CREDENTIALS_USERNAME: The user's username with copilot role
- COPILOT_CREDENTIALS_PASSWORD: The user's password with copilot role
- MANAGER_CREDENTIALS_USERNAME: The user's username with manager role
- MANAGER_CREDENTIALS_PASSWORD: The user's password with manager role

### Prepare

- Start Postgres.
- Create DynamoDB tables.
- Start Local ElasticSearch.
- Create ElasticSearch index.
- Various config parameters should be properly set.

### Running E2E tests with Postman

#### `Start` the app server before running e2e tests. You may need to set the env variables by calling `source env.sh` before calling `NODE_ENV=test npm start`.

- Make sure the db and es are properly started
```bash
  $ cd u-bahn-api

    # NOTE:
    # if tables and data already exist, please run first

    # $ npm run delete-data

    # to drop data and tables

    # Then re-initialize the es server and the database. (Note, You need to drop the `SequelizeMeta` table manually.)

  $ npm run migrations up            # to create table
  $ npm run insert-postman-data      # to insert the testing data for postman
  $ npm run migrate-db-to-es         # to index the db data to the ES
```

To run postman e2e tests.

```bash
npm run test:newman
```

To clear the testing data from postman e2e tests.

```bash
npm run test:newman:clear
```

## Running tests in CI
- TBD

## Verification

Refer to the verification document `Verification.md`.