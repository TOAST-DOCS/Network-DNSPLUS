## Network > DNS Plus > Console User Guide

This Console User Guide describes how to manage DNS Zones and record sets in the console.

## Manage DNS Zone

You can manage the DNS Zone on the **DNS Plus** screen of the menu.

![image_01_20190604](https://static.toastoven.net/prod_dnsplus/image_01_20190604.png)

### Create DNS Zone

![image_02_20190604](https://static.toastoven.net/prod_dnsplus/image_02_20190604.png)

1. The DNS Zone is a container of the record sets, which means a domain area for the host where the DNS provides service. Click the **Create DNS zone** button to create one.

2. Enter the **DNS Zone name** and the **Description** and then click the **OK** button.  

- In the **DNS Zone name**, type the user-owned domain or the sub domain as follows: [FQDN(fully qualified domain name)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name).
- Its **DNS Zone name** must be unique within the DNS server.
- The same **DNS Zone name** can be created as many as the number of DNS servers. There are three DNS servers.
- After it has been created, set the name server information of the default NS record set in the domain. For the default record set, see [Manage Record Set](./console-guide/#manage-record-set).
	- If it is created as a newly-registered domain, set the name server information to the corresponding name server in the domain registrar.
	- If it is created as a sub domain of the domain currently in operation, set the NS record set using the sub domain name and the corresponding name server in the domain being operated.

### Updating DNS Zone

![image_03_20190604](https://static.toastoven.net/prod_dnsplus/image_03_20190604.png)

1. Select the DNS Zone to modify and then click the **Update DNS Zone** button.

2. Modify the **Description** and then click the **OK** button.

### Delete DNS Zone

![image_04_20190604](https://static.toastoven.net/prod_dnsplus/image_04_20190604.png)

1. Select all DNS Zones to delete and then click the **Delete DNS Zone** button.

2. Click the **OK** button. All record sets within the DNS Zone will be deleted and it takes some time to finish the job.

## Manage Record set

![image_05_20190604](https://static.toastoven.net/prod_dnsplus/image_05_20190604.png)

Manage record sets of the DNS Zone selected on the **DNS Plus** screen of the menu.

- Create the record sets for the **DNS Zone name** or the sub name by record set type.
- SOA and NS record sets of the **DNS Zone name** are created by default and cannot be modified or deleted.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.


### Create Record set

![image_06_20190604](https://static.toastoven.net/prod_dnsplus/image_06_20190604.png)

1. The record set is the information of the host served. Click the **Create record set** button to create it.

2. Enter the record set information.

- Record set name: Enter the **DNS Zone name** or the sub name as the host name to serve in the [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) type.
- Record set type: This is a type of host which can be selected according to the purpose of use.
- TTL(sec) Time to live (TTL) refers to the valid period of data. Enter the renew cycle of the record set information in seconds in the name server. Right-click for easy entry.

3. Enter record information appropriate for the record set type.

- A different record value must be entered depending on the record set type. Enter the record value according to the description displayed at the bottom of the screen.
- The record value must be different by record set type. Click the **+** button on the right side of the screen to add a record. Click the **-** button next to the added record to remove it.

4. After completing the setup, click the **OK** button.

### Modify Record set

![image_07_20190604](https://static.toastoven.net/prod_dnsplus/image_07_20190604.png)

1. Select a record set to modify and then click the **Modify Record Set** button.

2. **Record set name** and **record set type** cannot be modified. However, **TTL(sec)** and **record value** can be modified.

3. After completing the setup, click the **OK** button.

### Delete Record set

![image_08_20190604](https://static.toastoven.net/prod_dnsplus/image_08_20190604.png)

1. Select the record set to delete and then click the **Delete Record Set** button.

2. Click the **OK** button.

### Record set statistics

![image_09_20190604](https://static.toastoven.net/prod_dnsplus/image_09_20190604.png)

1. Select the record set to view and then click the **Record Set Statistics** button.

2. You can view the query information and average response time of the record set for the last seven days.


### Look up and view list

![image_10_20190604](https://static.toastoven.net/prod_dnsplus/image_10_20190604.png)

1. View only the record set types selected with **Type Filter**.

2. Type the keyword and click the **Enter Key** or the **Search** to search for the record set name.  

- All values which include the keyword are searched.

