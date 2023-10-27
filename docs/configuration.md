# Configuration

Configuration has three sections: server configs, schema configs and auth configs.

Below is sample config:

```properties
# Server Configs
# More configs: https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.server
server.port       = 8081

# Schema configs
# Location of schemas
schema.location   = sample_schemas
# Name of the organization, used for package name in the generated code
schema.org-name   = logunify

# Security configs
# Comma separated api keys
# Type of authentication, supported values are "basic" and "none"
security.auth-type = none
# Comma separated api keys, required if auth-type is "basic"
# security.basic-auth.api-keys = key1,key2
```

## Server Configs
Configs for running the server. LogUnify service is built on top of the springboot
framework, more config properties can be found in [sprint boot server config properties].(https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.server)

## Schema Configs
Configs for schemas and codegen:
* `schema.location` (required): path to the schema, if you use the `service.sh` script to start the service,
it will automatically config that for you based on the path_to_schema you provided via `-s` option.
* `schema.org-name` (required): name of the organization, this is used as part of the package name for some languages. For 
instance in java the package name would be `com.[org-name][project-name]`.

## Security Configs
All configs for security:
* `security.auth-type` (required): type of authentication to enable. Currently, the supported values are `none` and `basic`.
* `security.basic-auth.api-key` (required if `security.auth-type` is set to basic): a comma separated list of api keys used for basic auth. 
Api keys should be set under the `X-Auth-Token` header in requests, for instance: `curl -X GET -H "X-Auth-Token: key1"  'localhost:8081/api/info/schemas'`. 
