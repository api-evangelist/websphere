# IBM WebSphere (websphere)

IBM WebSphere is a family of enterprise software products that provide middleware and application server capabilities for building, deploying, and managing enterprise applications.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/websphere/refs/heads/main/apis.yml)

## Tags

Application Server, Cloud Native, Enterprise Java, J2EE, Microservices, Middleware

## Timestamps

- **Created:** 2024-01-15
- **Modified:** 2026-04-18

## APIs

### WebSphere Application Server Admin API
REST API for administrative operations on WebSphere Application Server including deployment, configuration, and monitoring. Starting with version 9.0.0.1, Swagger documentation can be discovered and exposed for deployed RESTful endpoints.

**Human URL:** [https://www.ibm.com/docs/en/was](https://www.ibm.com/docs/en/was)

#### Tags

Administration, Configuration, Deployment, Monitoring

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was/9.0.5?topic=overview-administrative-rest-api)
- [OpenAPI](openapi/websphere-admin-rest-api.yml)
- [Authentication](https://www.ibm.com/docs/en/was/9.0.5?topic=api-authentication-authorization)
- [GettingStarted](https://www.ibm.com/docs/en/was-nd/9.0.5?topic=specifications-discovering-rest-api-documentation)

### WebSphere Liberty Admin REST API
RESTful administrative API for WebSphere Liberty servers, providing configuration and runtime management capabilities.

**Human URL:** [https://www.ibm.com/docs/en/was-liberty](https://www.ibm.com/docs/en/was-liberty)

#### Tags

Administration, Configuration, Liberty, REST API

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was-liberty/base?topic=liberty-admin-rest-api)
- [OpenAPI](openapi/websphere-liberty-admin-rest-api.yml)
- [GettingStarted](https://www.ibm.com/docs/en/was-liberty/base?topic=api-getting-started)

### WebSphere Liberty REST Connector API
Secure REST-based JMX connector for remote administration of Liberty servers with MBean access and file transfer.

**Human URL:** [https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html](https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html)

#### Tags

JMX, Liberty, Remote Administration, REST Connector

#### Properties

- [Documentation](https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html)
- [OpenAPI](openapi/websphere-liberty-rest-connector-api.yml)

### WebSphere MQ REST API
REST API for IBM MQ messaging operations including sending and receiving messages, queue management, and monitoring.

**Human URL:** [https://www.ibm.com/docs/en/ibm-mq](https://www.ibm.com/docs/en/ibm-mq)

#### Tags

Integration, Messaging, Publish Subscribe, Queue Management

#### Properties

- [Documentation](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=api-rest-introduction)
- [OpenAPI](openapi/websphere-mq-rest-api.yml)
- [Authentication](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=api-rest-authentication)

### WebSphere Application Server JMX API
Java Management Extensions (JMX) API for programmatic management and monitoring of WebSphere Application Server.

**Human URL:** [https://www.ibm.com/docs/en/was](https://www.ibm.com/docs/en/was)

#### Tags

JMX, Management, MBeans, Monitoring

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was/9.0.5?topic=api-using-jmx)

### WebSphere Liberty Collective Controller REST API
API for managing Liberty collective environments, including member management, scaling, and centralized administration.

**Human URL:** [https://www.ibm.com/docs/en/was-liberty](https://www.ibm.com/docs/en/was-liberty)

#### Tags

Clustering, Collective, Liberty, Management

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was-liberty/base?topic=liberty-collective-rest-api)
- [OpenAPI](openapi/websphere-liberty-collective-controller-rest-api.yml)

### WebSphere Automation REST API
API for WebSphere Automation capabilities including vulnerability management, automated patching, and health monitoring.

**Human URL:** [https://www.ibm.com/docs/en/ws-automation](https://www.ibm.com/docs/en/ws-automation)

#### Tags

Automation, Health Monitoring, Patching, Vulnerability Management

#### Properties

- [Documentation](https://www.ibm.com/docs/en/ws-automation?topic=apis)
- [OpenAPI](openapi/websphere-automation-rest-api.yml)

### Open Liberty APIs
Open Liberty provides application programming interfaces that extend and complement Jakarta EE and MicroProfile APIs.

**Human URL:** [https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html](https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html)

#### Tags

Cloud Native, Jakarta EE, MicroProfile, Open Liberty

#### Properties

- [Documentation](https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html)
- [OpenAPI](openapi/open-liberty-apis.yml)
- [Blog](https://openliberty.io/blog/)

## Capabilities

### Shared Per-API Definitions (capabilities/shared/)

| File | API |
|------|-----|
| [admin-rest.yaml](capabilities/shared/admin-rest.yaml) | WebSphere Application Server Admin REST API |
| [liberty-admin.yaml](capabilities/shared/liberty-admin.yaml) | WebSphere Liberty Admin REST API |
| [rest-connector.yaml](capabilities/shared/rest-connector.yaml) | WebSphere Liberty REST Connector API |
| [mq-rest.yaml](capabilities/shared/mq-rest.yaml) | IBM MQ REST API |
| [collective-controller.yaml](capabilities/shared/collective-controller.yaml) | Liberty Collective Controller REST API |
| [automation.yaml](capabilities/shared/automation.yaml) | WebSphere Automation REST API |
| [open-liberty.yaml](capabilities/shared/open-liberty.yaml) | Open Liberty APIs |

### Workflow Capabilities

| File | Workflow | APIs Combined |
|------|----------|---------------|
| [server-administration.yaml](capabilities/server-administration.yaml) | Server Administration | Admin REST + Liberty Admin + REST Connector + Collective Controller |
| [messaging-and-integration.yaml](capabilities/messaging-and-integration.yaml) | Messaging and Integration | MQ REST |
| [security-and-compliance.yaml](capabilities/security-and-compliance.yaml) | Security and Compliance | Automation + Admin REST |
| [monitoring-and-observability.yaml](capabilities/monitoring-and-observability.yaml) | Monitoring and Observability | Open Liberty + Admin REST + Liberty Admin |

## Artifacts

### OpenAPI Specifications (openapi/)

- [websphere-admin-rest-api.yml](openapi/websphere-admin-rest-api.yml)
- [websphere-liberty-admin-rest-api.yml](openapi/websphere-liberty-admin-rest-api.yml)
- [websphere-liberty-rest-connector-api.yml](openapi/websphere-liberty-rest-connector-api.yml)
- [websphere-mq-rest-api.yml](openapi/websphere-mq-rest-api.yml)
- [websphere-liberty-collective-controller-rest-api.yml](openapi/websphere-liberty-collective-controller-rest-api.yml)
- [websphere-automation-rest-api.yml](openapi/websphere-automation-rest-api.yml)
- [open-liberty-apis.yml](openapi/open-liberty-apis.yml)

### Spectral Rules (rules/)

- [websphere-spectral-rules.yml](rules/websphere-spectral-rules.yml)

### JSON Schema (json-schema/)

- [application.json](json-schema/application.json)
- [cluster.json](json-schema/cluster.json)
- [message-queue.json](json-schema/message-queue.json)
- [server.json](json-schema/server.json)
- Plus 67 API-specific schema files

### JSON-LD (json-ld/)

- [context.jsonld](json-ld/context.jsonld)
- Plus 7 API-specific context files

### Examples (examples/)

- 65 example files covering all API schemas

### Vocabulary (vocabulary/)

- [websphere-vocabulary.yaml](vocabulary/websphere-vocabulary.yaml)

## Common Properties

- [Portal](https://developer.ibm.com/wasdev/)
- [Support](https://www.ibm.com/mysupport)
- [Documentation](https://www.ibm.com/docs/en/was)
- [GitHub Organization](https://github.com/WASdev)
- [Blog](https://openliberty.io/blog/)
- [StackOverflow](https://stackoverflow.com/questions/tagged/websphere)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
