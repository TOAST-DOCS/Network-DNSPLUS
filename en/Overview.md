## Network > DNS Plus > Overview

DNS Plus is a type of DNS that allows users all around the world to access their networks in a fast and reliable way. It supports Anycast network for fast and reliable access to DNS from anywhere. In addition, DNS services are directly available in the web console without separate DNS solutions or servers.

## Key features

- DNS
    - DNS services are directly available in the web console without separate DNS solutions or servers.
    - It supports Anycast network so that a high speed services can be provided in and out of the country.
    - As designed to accept a massive volume of DNS queries, it can process a huge volume of DNS queries and mitigate DDoS attacks against DNS.

- GSLB
    - DNS Plus GLSB (Global Server Load Balancing) is based on DNS service which provides load balancing of traffic to endpoints on a stable basis.
    - Provides a variety of routing rules.

    | Routing Rules | Description | Details |
    |---|---|---|
    | FAILOVER | Routes pools connected to GLSB domain in the priority order. |  |
    | RANDOM | Routes available pools connected to GLSB domain randomly. |  |
    | GEOLOCATION | Routes traffic of configured region to the corresponding connected pool.<br>Routes in the priority order, if regional setting is not available. | < Available Regions ><br>- Western North America<br>- Eastern Nort America<br>- Western Europe<br>- Eastern Europe<br>- Northern South America<br>- Southern South America<br>- Oceania<br>- Middle East<br>- Northern Africa<br>- Southern Africa<br>- India<br>- Southeast Asia<br>- Northeast Asia |

    - Endpoint grouping via pool
        - Endpoints can be grouped by pool for specific purposes.
        - Weighted value can be configured for each endpoint included in the pool.

    - Support for healthchecks for various endpoints
        - Service can be supported more stably with healthcheck execution at endpoints included in the pool, by using HTTP/HTTPS/TCP.
        - For HTTP healthchecks, port, route, expected status code, and expected response body can be configured.
        - For HTTPS healthchecks, certificate validity, port, route, expected status code, and expected response body can be configured.

## Service targets

- DNS
    - For developers whose applications and services will be provided to global customers
    - For administrators and maintenance engineers who want to ease the burden of management and maintenance of DNS servers

- GSLB
    - To provide stable load balancing of service traffic
    - To provide global load balancing of service traffic
    - To configure Disaster Recovery (DR) through load balancing

## Service terms

| Terms | Description |
|---|---|
| Domain | An address used to identify a computer on the network, which is presented in a human-recognizable format. |
| DNS Zone | A domain area of the host that DNS provides service. As a container of record sets, it specified how to route traffic from domain and subdomain. |
| Record set | Host information that DNS provides service. This is used to route traffic from domain. |
| Record set type | A resource type which shows how to route traffic from host. |
| Record value | A description of how to route the traffic from the host. The entry is determined based on the record set type (resource type) and one or more record values can be registered. |
