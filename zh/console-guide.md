## Network > DNS Plus > 控制台使用指南

这里介绍在控制台中管理DNS Zone及记录集合的方法。

## 管理DNS Zone

在菜单的**DNS Plus**界面中可管理DNS Zone。

![image_01_20190604](https://static.toastoven.net/prod_dnsplus/image_01_20190604.png)

### 创建DNS Zone

![image_02_20190604](https://static.toastoven.net/prod_dnsplus/image_02_20190604.png)

1.DNS Zone作为记录集合的容器，是与DNS服务的主机的相关域名区域，单击**创建DNS Zone**按钮创建。

2.输入**DNS Zone名**与**说明**后单击**确认**按钮。  

	- 在**DNS Zone名**中以[FQDN(fully qualified domain name)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入用户的域名或下级域名。
	- **DNS Zone名**在DNS服务器中应唯一。
	- 可按照DNS服务器数创建相同的**DNS Zone名**。DNS服务器为3台。
	- 创建完成后，应为域设置默认创建的NS记录集合的名称服务器信息。默认创建的记录集合可在[管理记录集合](./console-guide/#_1)中确认。
		- 以新注册的域名创建时，应在域名注册机构将名称服务器信息设置为相应名称服务器。
		- 以运营中域名的下级域名创建时，应在运营中的域名中以下级域名及相应名称服务器创建NS记录集合。

### 修改DNS Zone

![image_03_20190604](https://static.toastoven.net/prod_dnsplus/image_03_20190604.png)

1.选择要修改的DNS Zone后单击**修改DNS Zone**按钮。

2.修改**说明**后单击**确认**按钮。

### 删除DNS Zone

![image_04_20190604](https://static.toastoven.net/prod_dnsplus/image_04_20190604.png)

1.选择所有要删除的DNS Zone后单击**删除DNS Zone**按钮。

2.单击**确认**按钮。DNS Zone中所有记录集合也将删除，所以需要一定时间。

## 管理记录集合

![image_05_20190604](https://static.toastoven.net/prod_dnsplus/image_05_20190604.png)

在菜单的**DNS Plus**界面中可管理选择的DNS Zone的记录集合。

- 可按照记录集合类型创建与**DNS Zone名**或下级名相关的记录集合。
- 默认创建**DNS Zone名**的SOA与NS记录集合，但无法修改及删除。
- SOA的记录集合无法创建/修改/删除，NS记录集合无法以**DNS Zone名**创建/修改/删除。


### 创建记录集合

![image_06_20190604](https://static.toastoven.net/prod_dnsplus/image_06_20190604.png)

1.记录集合为服务的主机信息，单击**创建记录集合**按钮创建。

2.输入记录集合信息。

	- 记录集合名：作为要服务的主机名，以[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)输入，使其可成为**DNS Zone名**或下级名。
	- 记录集合类型：作为主机的类型，根据使用目的选择类型。
	- TTL(秒)：以Time to live显示数据的有效期。以秒为单位输入名称服务器中记录集合信息的更新周期。单击右侧按钮，可简单地输入。

3.根据记录集合类型输入记录信息。

	- 应根据记录集合类型输入不同的记录值。按照创建界面下端说明输入。
	- 根据记录集合类型，可输入多个记录。单击界面右侧**+**按钮，可添加记录，单击添加的记录的**-**按钮，可删除记录。

4.设置完成后单击**确认**按钮。

### 修改记录集合

![image_07_20190604](https://static.toastoven.net/prod_dnsplus/image_07_20190604.png)

1.选择要修改的记录集合后单击**修改记录集合**按钮。

2.**记录集合名**与**记录集合类型**无法修改，**TTL(秒)**与**记录值**可修改。

3.设置完成后单击**确认**按钮。

### 删除记录集合

![image_08_20190604](https://static.toastoven.net/prod_dnsplus/image_08_20190604.png)

1.选择所有要删除的记录集合后单击**删除记录集合**按钮。

2.单击**确认**按钮。

### 统计记录集合

![image_09_20190604](https://static.toastoven.net/prod_dnsplus/image_09_20190604.png)

1.选择要确认的记录集合后单击**统计记录集合**按钮。

2.可确认记录集合最近一周的查询信息及平均响应时间。


### 查询及查看列表

![image_10_20190604](https://static.toastoven.net/prod_dnsplus/image_10_20190604.png)

1.利用**类型筛选器**可仅查看选择的记录集合类型。

2.输入搜索词后单击**Enter Key**或**搜索**，可利用记录集合名搜索。  

	- 搜索包含搜索词的所有值。
