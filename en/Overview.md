## Network > DNS Plus > Overview

DNS Plus is a DNS that users around the world can access fast and reliably. DNS Plus supports anycast networks so that users can access DNS fast and reliably from anywhere, and allows you to provide a DNS service directly from the web console without a separate DNS solution or server.

## Key Features

- DNS
    - You can perform DNS configuration in the web console without installing a separate DNS server.
    - It supports anycast networks so that you can provide a service at high speed in Korea as well as in other countries.
    - Because it is designed to accept a high volume of DNS queries, it can process a high volume of DNS queries and also mitigate DDoS attacks targeting DNS.

- GSLB
    - DNS Plus GSLB (global server load balancing) is a service that performs reliable load balancing of traffic to endpoints based on DNS service.
    - It supports various routing rules.

    | Routing Rule | Description | Details |
    |---|---|---|
    | FAILOVER | Performs routing based on the priorities of pools connected to the GSLB domain. |  |
    | RANDOM | Performs routing by randomly selecting an available pool among pools connected to the GSLB domain. |  |
    | GEOLOCATION | Routes the traffic of the configured region to the connected pool.<br>When there is no configured region, performs routing based on the priorities of connected pools. | < Available Regions ><br>\- Western North America<br>\- Eastern North America<br>\- Western Europe<br>\- Eastern Europe<br>\- Northern South America<br>\- Southern South America<br>\- Oceania<br>\- Middle East<br>\- Northern Africa<br>\- Southern Africa<br>\- India<br>\- Southeast Asia<br>\- Northeast Asia |

    - Endpoint grouping through pool
        - You can use pools to group endpoints according to specific purposes.
        - Weights can be set for each endpoint included in the pool.

    - Supports various endpoint health check functions
        - Supports a reliable service by performing health checks of endpoints included in the pool using HTTP/HTTPS/TCP
        - For HTTP health checks, you can set the port, path, expected status code, and expected response body.
        - For HTTPS health checks, you can set the certificate validity, port, path, expected status code, and expected response body.

## Service Targets

- DNS
    - If you're developing an application provided for global users
    - When it is burdensome to manage and maintain DNS servers

- GSLB
    - If you want reliable load balancing of service traffic
    - If you want load balancing of global service traffic
    - If you want to configure disaster recovery (DR) through load balancing

## Service Terms

| Term | Description |
|---|---|
| Domain | A method of representing an address that identifies a computer on a network in a human-recognizable form. |
| DNS Zone | The domain zone of the host served by DNS. A container for record sets, which contains how to route traffic of domain and subdomain. |
| Record set | The information of host served by DNS Zone, which is how to route traffic of a domain. |
| Record set type | A resource type that indicates how to route traffic of a host. |
| Record value | Description of how to route the traffic of a host. The input content is determined by the record set type (resource type), and one or more values can be registered except for a specific type. |
| GSLB domain | An abbreviation of global server load balancing domain. It is a domain that can be used by mapping the CNAME to the service domain (record set) created by DNS Plus. |
| Pool | A component that groups endpoints, which is the smallest unit to which a routing rule is applied. Endpoints included in the pool are load balanced according to their weight. |
| Endpoint | Refers to the server that handles the actual traffic. Endpoints are available in the form of IP or domain. |
| Health check | A component that checks accessibility for endpoints belonging to a pool according to the configured value. |
