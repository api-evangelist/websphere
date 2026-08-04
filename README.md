# IBM WebSphere (websphere)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

IBM WebSphere is a family of enterprise software products that provide middleware and application server capabilities for building, deploying, and managing enterprise applications.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/websphere/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/websphere/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Application Server
- Cloud Native
- Enterprise Java
- J2EE
- Microservices
- Middleware

## Timestamps

- **Created:** 2024-01-15
- **Modified:** 2026-05-19

## APIs

### WebSphere Application Server Admin API

REST API for administrative operations on WebSphere Application Server including deployment, configuration, and monitoring. Starting with version 9.0.0.1, Swagger documentation can be discovered and exposed for deployed RESTful endpoints.

- **Human URL:** [https://www.ibm.com/docs/en/was](https://www.ibm.com/docs/en/was)
- **Base URL:** `https://localhost:9443/ibm/api`

#### Tags

- Administration
- Configuration
- Deployment
- Monitoring

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was/9.0.5?topic=overview-administrative-rest-api)
- [OpenAPI](https://www.ibm.com/docs/en/was/9.0.5?topic=api-openapi-specification) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/websphere-admin-rest-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/websphere-admin-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-admin-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Authentication](https://www.ibm.com/docs/en/was/9.0.5?topic=api-authentication-authorization)
- [Getting Started](https://www.ibm.com/docs/en/was-nd/9.0.5?topic=specifications-discovering-rest-api-documentation)
- [API Reference](https://www.ibm.com/docs/en/was/9.0.5?topic=javadoc-apis-application-programming-interfaces)
- [JSON Schema](json-schema/application.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/server.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/cluster.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### WebSphere Liberty Admin REST API

RESTful administrative API for WebSphere Liberty servers, providing configuration and runtime management capabilities. The Admin REST Connector enables secure remote access through HTTPS calls or Java clients.

- **Human URL:** [https://www.ibm.com/docs/en/was-liberty](https://www.ibm.com/docs/en/was-liberty)
- **Base URL:** `https://localhost:9443/ibm/api`

#### Tags

- Administration
- Configuration
- Liberty
- REST API

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was-liberty/base?topic=liberty-admin-rest-api)
- [OpenAPI](openapi/websphere-liberty-admin-rest-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/websphere-liberty-admin-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-liberty-admin-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Getting Started](https://www.ibm.com/docs/en/was-liberty/base?topic=api-getting-started)
- [API Reference](https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html)
- [Documentation](https://www.ibm.com/docs/en/was-liberty/base?topic=center-setting-up-admin)
- [JSON Schema](json-schema/application.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/server.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### WebSphere Liberty REST Connector API

Secure REST-based JMX connector for remote administration of Liberty servers. Provides file transfer and JMX MBean access through HTTPS endpoints, requiring TLS for confidential communication.

- **Human URL:** [https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html](https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html)
- **Base URL:** `https://localhost:9443/IBMJMXConnectorREST/api`

#### Tags

- JMX
- Liberty
- Remote Administration
- REST Connector

#### Properties

- [Documentation](https://openliberty.io/docs/latest/reference/feature/restConnector-2.0.html)
- [OpenAPI](openapi/websphere-liberty-rest-connector-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/websphere-liberty-rest-connector-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-liberty-rest-connector-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [API Reference](https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html)
- [Documentation](https://openliberty.io/docs/latest/configuring-jmx-connection.html)
- [Documentation](https://www.ibm.com/docs/en/was-liberty/core?topic=features-admin-rest-connector-20)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### WebSphere MQ REST API

REST API for IBM MQ messaging operations including sending and receiving messages, queue management, and monitoring. Supports both administration and messaging endpoints for point-to-point and publish-subscribe patterns.

- **Human URL:** [https://www.ibm.com/docs/en/ibm-mq](https://www.ibm.com/docs/en/ibm-mq)
- **Base URL:** `https://localhost:9443/ibmmq/rest/v2`

#### Tags

- Integration
- Messaging
- Publish Subscribe
- Queue Management

#### Properties

- [Documentation](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=api-rest-introduction)
- [OpenAPI](openapi/websphere-mq-rest-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/websphere-mq-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-mq-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [API Reference](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=api-rest-reference)
- [Authentication](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=api-rest-authentication)
- [Getting Started](https://developer.ibm.com/tutorials/mq-develop-mq-rest-api/)
- [Documentation](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=mq-messaging-using-rest-api)
- [JSON Schema](json-schema/message-queue.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### WebSphere Application Server JMX API

Java Management Extensions (JMX) API for programmatic management and monitoring of WebSphere Application Server. Provides MBean access for server configuration, performance monitoring, and resource management.

- **Human URL:** [https://www.ibm.com/docs/en/was](https://www.ibm.com/docs/en/was)
- **Base URL:** `service:jmx:rmi:///jndi/rmi://localhost:2809/jmxrmi`

#### Tags

- JMX
- Management
- MBeans
- Monitoring

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was/9.0.5?topic=api-using-jmx)
- [API Reference](https://www.ibm.com/docs/en/was/9.0.5?topic=reference-javadoc)
- [Documentation](https://www.ibm.com/docs/en/was/9.0.5?topic=api-jmx-examples)
- [JSON Schema](json-schema/server.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Postman Collection](collections/open-liberty-apis.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/open-liberty-apis.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/websphere-admin-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-admin-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/websphere-automation-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-automation-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/websphere-liberty-admin-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-liberty-admin-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/websphere-liberty-collective-controller-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-liberty-collective-controller-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/websphere-liberty-rest-connector-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-liberty-rest-connector-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/websphere-mq-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-mq-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### WebSphere Liberty Collective Controller REST API

API for managing Liberty collective environments, including member management, scaling, and centralized administration. Supports subscription-based notifications for API changes across collective members.

- **Human URL:** [https://www.ibm.com/docs/en/was-liberty](https://www.ibm.com/docs/en/was-liberty)
- **Base URL:** `https://localhost:9443/ibm/api/collective`

#### Tags

- Clustering
- Collective
- Liberty
- Management

#### Properties

- [Documentation](https://www.ibm.com/docs/en/was-liberty/base?topic=liberty-collective-rest-api)
- [OpenAPI](openapi/websphere-liberty-collective-controller-rest-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/websphere-liberty-collective-controller-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-liberty-collective-controller-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [API Reference](https://www.ibm.com/docs/en/was-liberty/base?topic=api-collective-reference)
- [Documentation](https://www.ibm.com/docs/en/was-liberty/base?topic=deploying-applications-in-liberty)
- [JSON Schema](json-schema/cluster.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/server.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### WebSphere Automation REST API

API for WebSphere Automation capabilities including vulnerability management, automated patching, and health monitoring. Accessible through the Swagger UI on OpenShift Container Platform deployments.

- **Human URL:** [https://www.ibm.com/docs/en/ws-automation](https://www.ibm.com/docs/en/ws-automation)
- **Base URL:** `https://automation-api.example.com/v1`

#### Tags

- Automation
- Health Monitoring
- Patching
- Vulnerability Management

#### Properties

- [Documentation](https://www.ibm.com/docs/en/ws-automation?topic=apis)
- [OpenAPI](openapi/websphere-automation-rest-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/websphere-automation-rest-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/websphere-automation-rest-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [API Reference](https://www.ibm.com/docs/en/ws-automation?topic=reference-rest-api)
- [Documentation](https://www.ibm.com/docs/en/ws-automation?topic=viewing-rest-api)
- [JSON Schema](json-schema/server.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Open Liberty APIs

Open Liberty provides application programming interfaces that extend and complement Jakarta EE and MicroProfile APIs. Includes APIs for security, admin connectors, batch processing, messaging, and more.

- **Human URL:** [https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html](https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html)
- **Base URL:** `https://localhost:9443/ibm/api`

#### Tags

- Cloud Native
- Jakarta EE
- MicroProfile
- Open Liberty

#### Properties

- [Documentation](https://openliberty.io/docs/latest/reference/api/open-liberty-apis.html)
- [OpenAPI](openapi/open-liberty-apis.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/open-liberty-apis.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/open-liberty-apis.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [API Reference](https://openliberty.io/docs/latest/documentation-openapi.html)
- [Getting Started](https://openliberty.io/guides/rest-intro.html)
- [Documentation](https://openliberty.io/docs/latest/microprofile.html)
- [Blog](https://openliberty.io/blog/)
- [JSON Schema](json-schema/application.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/server.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

## Common Properties

- [Arazzo Workflows](arazzo/) — [Arazzo Specification](https://spec.openapis.org/arazzo/latest.html)
- [Portal](https://developer.ibm.com/wasdev/)
- [Support](https://www.ibm.com/mysupport)
- [Documentation](https://www.ibm.com/docs/en/was)
- [Getting Started](https://www.ibm.com/support/pages/websphere-liberty-developers)
- [Changelog](https://www.ibm.com/support/pages/recommended-updates-websphere-application-server)
- [Pricing](https://www.ibm.com/products/websphere-hybrid-edition/pricing)
- [Terms of Service](https://www.ibm.com/legal/terms)
- [Privacy Policy](https://www.ibm.com/us-en/privacy)
- [Status Page](https://cloud.ibm.com/status)
- [Blog](https://openliberty.io/blog/)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/websphere)
- [Sign Up](https://cloud.ibm.com/registration)
- [Console](https://www.ibm.com/products/liberty)
- [GitHub Organization](https://github.com/WASdev)
- [SDK](https://github.com/WASdev/sample.batch.bonuspayout)
- [Features](https://www.ibm.com/products/websphere-application-server)
- [Use Cases](https://www.ibm.com/products/websphere-application-server)
- [Integrations](https://www.ibm.com/docs/en/was)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
