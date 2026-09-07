<!-- machine_translated: true -->

<!-- pre-align:aligned sig=464bf6032da9 -->

<a id="network-dns-plus-release-notes"></a>
## Network > DNS Plus > Release Notes { #network-dns-plus-release-notes }

<a id="april-14-2026"></a>
### April 14, 2026 { #april-14-2026 }

<a id="april-14-2026-added-features"></a>
#### Added Features

* Added API v2.0
    * Added support for User Access Key tokens.

<a id="november-25-2025"></a>
### November 25, 2025 { #november-25-2025 }

<a id="november-25-2025-feature-updates"></a>
#### Feature Updates

*  Changed the maximum length of record values for TXT record set type from 255 bytes to 4,096 bytes.
{% if "gov" not in build_flags %}

<a id="april-29-2025"></a>
### April 29, 2025 { #april-29-2025 }

<a id="april-29-2025-feature-updates"></a>
#### Feature Updates

*  Changed the minimum value of the Record Set TTL from 1 to 10.
{% endif %}

<a id="may-28-2024"></a>
### May 28, 2024 { #may-28-2024 }

<a id="may-28-2024-added-features"></a>
#### Added Features

* Added the feature to set the header for health check requests, health check interval, maximum response latency (timeout), and maximum number of retries in GSLB health checks.

<a id="march-12-2024"></a>
### March 12, 2024 { #march-12-2024 }

<a id="march-12-2024-feature-updates"></a>
#### Feature Updates

* Stopped support for the SPF record set type. You can use the TXT record set type instead.
    * For more information, see [[RFC 7208#section-14.1]](https://datatracker.ietf.org/doc/html/rfc7208#section-14.1).
{% if "gov" in build_flags %}

<a id="december-07-2021"></a>
### December 07, 2021 { #december-07-2021 }

<a id="december-07-2021-added-features"></a>
#### Added Features

##### DNS Plus

* Added the Create multiple record sets feature.


<a id="november-03-2020"></a>
### November 03, 2020 { #november-03-2020 }

<a id="november-03-2020-feature-updates"></a>
#### Feature Updates

* Improved the service so that the record set type can be modified when modifying a record set.


<a id="april-07-2020"></a>
### April 7, 2020 { #april-07-2020 }

<a id="april-07-2020-release-of-a-new-product"></a>
#### Release of a New Product

* DNS Plus is a service that provides domain management features and reliable load balancing of server traffic.
* You can configure and manage domains easily with DNS (Domain Name System).
* With GSLB (global server load balancing), you can configure endpoint servers with DR (disaster recovery), random load balancing, or global load balancing according to the routing rule.
{% else %}
<a id="august-24-2021"></a>
### August 24, 2021 { #august-24-2021 }

<a id="august-24-2021-added-features"></a>
#### Added Features

* Added the Create multiple record sets feature.


<a id="september-22-2020"></a>
### September 22, 2020 { #september-22-2020 }

<a id="september-22-2020-feature-updates"></a>
#### Feature Updates

* Improved the service so that the record set type can be modified when modifying a record set.

<a id="december-24-2019"></a>
### December 24, 2019 { #december-24-2019 }

<a id="december-24-2019-added-features"></a>
#### Added Features

* Added the GSLB (global server load balancing) feature that allows reliable load balancing of traffic of an endpoint server.
* The created GSLB domain can be configured with DR (disaster recovery), random load balancing, or global load balancing according to the routing rule.
* Pool is a component that groups endpoint servers, which is the smallest unit to which a routing rule can be applied.
* Supports reliable services by periodically performing health checks on the endpoint servers included in the pool. Health check supports HTTP, HTTPS, and TCP.

<a id="december-24-2019-feature-updates"></a>
#### Feature Updates

* Made improvements so that, when creating or modifying record sets, users can enter the CNAME record set type by selecting from their own GSLB domains.


<a id="august-27-2019"></a>
### August 27, 2019 { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### Feature Updates

* Added the maximum number of record sets that can be created. You can create up to 5,000 record sets per DNS Zone.
* Made a modification so that, when querying record set statistics, query for the CNAME record set type retrieves the A record set type and the AAAA record set type as well.


<a id="june-25-2019"></a>
### June 25, 2019 { #june-25-2019 }

<a id="june-25-2019-release-of-a-new-product"></a>
#### Release of a New Product

* DNS Plus is a service that provides domain management features.
* It allows you to configure a DNS server easily.
{%- endif %}
