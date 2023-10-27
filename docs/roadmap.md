# Roadmap & Vision

## Our Vision

In today's business landscape, application events are almost essential for business, serving as critical sources of analytical insights and communal units for services. However, ensuring highly efficient and consistent application events can be a challenging undertaking, often necessitating the use of multiple tools such as client logging, event gateway, message broker, and data warehouse integration. Moreover, developers typically need to construct in-house integrations to connect these tools seamlessly.

As we have personally experienced the arduous and time-consuming nature of this process, we understand its challenges firsthand. Our mission is to make the development of high-quality structured application events architecture accessible to all developers. Our current objective is to help developers ship the solution that seamlessly defines, collects, and transports events into the data warehouse in hours instead of weeks or even longer.

Our long-term vision is to transform into a unified event orchestration layer that developers can utilize to consume, produce, and analyze events in a structured and unified manner, while retaining the flexibility to select individual components like specific message brokers and data warehouses according to their preferences. The scope of this unified event orchestration layer will extend beyond logging and analytics, as it can also serve as a cross-platform schema registry for event-native services, or as a layer to integrate with LLMs for analyzing events in human languages.

## Roadmap
We are expecting to launch following feature in the following months:
* Schema Registry: 
  * A unified schema registry UI and API to show all event schemas with details like active integrations, message throughput and other relevant details
  * Admin capability to manage the schemas and integration
  * Integration of the schema registry to other services like Apache Kafka, AWS Glow Registry
* Data Enrichment:
  * Canonical data collection such as device os version, device build, etc
  * Server side enrichment such as the IP country
* Events Integration: 
  * Server Side Infrastructure: WAL (Write ahead log) for events, handling of backpressure 
  * Event sinks for AWS Redshift and Snowflake
* Security: 
  * Other auth type like OAuth and potential support of role/authorization
* Codegen: 
  * Other language support: python, Go
  * Download generated code as a package that ready to be published to language specific 

## Talk with Us
We're under the active development and are open for any feature request or feedbacks. Please come and talk with us in our [slack channel](https://join.slack.com/t/logunify/shared_invite/zt-1rko7zvlf-ENDYnVZGRb3v9FdoOBZp1A)!
