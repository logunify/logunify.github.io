# Schema

A schema definition has three sections, the metadata, the schema, and the outputs.

The structure of a schema definition looks like below:

```yaml
# Metadata
projectName: project_name
# Schema
schema:
  name: schema_name
  [optional]description:
  enums:
    - name:
      values:
  fields:
    - name:
      type:
      [optional]description:
# Outputs
outputs:
  - name:
    type:
    gcpProject:
    dataset:
    table:
    key:
```

## Metadata

One required fields in the metadata section, `projectName`. The `(projectName, schemaName)` pair serves as a unique identifier for the schema.
Also, the schema could have an optional `description` to represent its purpose.

## Schema

The schema section defines the list of fields in the event. We support most of the common types that data warehouse supports, including

- string
- int64 / int32
- boolean
- double / float
- enum
- array of primitive types
- map of primitive types

Also, each field has an optional description which will help improve its clarity and usability.

Examples of primitive type fields:

```yaml
- name: userId
  type: string
- name: numClicks
  type: int32
- name: price
  type: double
- name: primeMember
  type: boolean
```

In order to use enum, you'll need to define enums first like

```yaml
- name: Event
  values:
    - ADD_TO_CART
    - REMOVE_FROM_CART
    - CHECKOUT
```

then you can use enum like other types

```yaml
- name: event
  type: enum<Event>
```

You can use primitive types as key / values in an array or map

```yaml
- name: products
  type: array<int32>
- name: shoppingCart
  type: map<int32, int32>
```

## Outputs

The output section defines the destination of the events. Right now only BigQuery output is allowed and other data warehouse integrations are under active development, please check our [Roadmap](/roadmap) page for more information.

To define a bigquery output, you need to add the following section to your yaml.

```yaml
outputs:
  - name: eventName
    type: BigQuery
    gcpProject: [gcpProjectName]
    dataset: [bqDataset]
    table: [bqTable]
    key: [service account key string | key file]
```

The backend service will automatically create the table in the bigquery dataset using the schema defined in this yaml file.
Please read [Integration](/integrations.md) doc on more details about the integration setting.

