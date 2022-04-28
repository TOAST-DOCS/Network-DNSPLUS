## Network > DNS Plus > Release Notes

### August 24, 2021

#### Added Features

* Added the Create multiple record sets feature.


### September 22, 2020

#### Feature Updates

* Improved the service so that the record set type can be modified when modifying a record set.


### December 24, 2019

#### Added Features

* Added the GSLB (global server load balancing) feature that allows reliable load balancing of traffic of an endpoint server.
* The created GSLB domain can be configured with DR (disaster recovery), random load balancing, or global load balancing according to the routing rule.
* Pool is a component that groups endpoint servers, which is the smallest unit to which a routing rule can be applied.
* Supports reliable services by periodically performing health checks on the endpoint servers included in the pool. Health check supports HTTP, HTTPS, and TCP.

#### Feature Updates

* Made improvements so that, when creating or modifying record sets, users can enter the CNAME record set type by selecting from their own GSLB domains.


### August 27, 2019

#### Feature Updates

* Added the maximum number of record sets that can be created. You can create up to 5,000 record sets per DNS Zone.
* Made a modification so that, when querying record set statistics, query for the CNAME record set type retrieves the A record set type and the AAAA record set type as well.


### June 25, 2019

#### Release of a New Product

* DNS Plus is a service that provides domain management features.
* It allows you to configure a DNS server easily.