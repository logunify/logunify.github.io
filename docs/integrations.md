# Integration

Only bigquery integration is supported at the moment and we are actively working on other integrations, please check our [roadmap](/roadmap) for more information.

The backend service authenticates with BigQuery using service accounts. Please follow this [doc](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) to create a service account and download the service account key. Make sure the service account has following permissions or the `bigquery-data-editor` role.

```
  "bigquery.tables.create",
  "bigquery.tables.createIndex",
  "bigquery.tables.createSnapshot",
  "bigquery.tables.delete",
  "bigquery.tables.deleteIndex",
  "bigquery.tables.export",
  "bigquery.tables.get",
  "bigquery.tables.getData",
  "bigquery.tables.getIamPolicy",
  "bigquery.tables.list",
  "bigquery.tables.restoreSnapshot",
  "bigquery.tables.update",
  "bigquery.tables.updateData",
  "bigquery.tables.updateTag",
```

There are three ways to pass the credentials to config the access key, using key file, key string, or `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

- Key file: mount your key json file into the docker image, and set the `key` field in the config file as path to your key file.
- Key string: set the `key` field in the config file as the string of your json key.
- Environment variable: if the key field is not being set in the config file, the backend service will use `GOOGLE_APPLICATION_CREDENTIALS` environment variable to search for the service account key file.
