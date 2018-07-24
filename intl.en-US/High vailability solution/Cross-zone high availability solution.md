# Cross-zone high availability solution {#concept_n4y_dkv_k2b .concept}

For an enterprise, whether or not their businesses are on the cloud, service stability and continuity are of critical importance. To reduce the impact of force majeure factors on normal service operations, you must work to improve the availability and disaster tolerance of products. Although your product may already be highly-available, you cannot ignore the important task of creating highly-available and disaster-tolerant services.

To improve service availability and disaster tolerance, many users take advantage of these cloud products: ECS, Server Load Balancer, RDS, and OSS.

## Zone {#section_dqc_nkv_k2b .section}

[Zones](https://www.alibabacloud.com/help/doc-detail/40654.htm) are physical areas with independent power grids and networks in the same region. The network latency is lower for ECS instances within the same zone. The network latency is lower for ECS instances within the same zone.

Intranet communication is available across different zones in the same region, and fault isolation can be achieved between zones. Whether to deploy ECS instances in the same zone depends on the requirements for disaster tolerance capabilities and network latency.

-   If your applications require high disaster tolerance capabilities, you are advised to deploy your ECS instances in different zones of the same region.
-   If your applications require low network latency between instances, you are advised to create your ECS instances in the same zone.

In the Region List, you can view the number of zones in each region.  Alternatively, you can use the Region List API in OpenAPI Explorer to view a list of all zones.

## Introduction {#section_rgv_5kv_k2b .section}

**ECS**

[Elastic Compute Service \(ECS\)](https://www.alibabacloud.com/help/doc-detail/25367.htm) is a basic cloud computing service provided by Alibaba Cloud.  An ECS instance is a virtual computing environment which includes a CPU, memory, operating system, disks, bandwidth, and other basic server components. It is the operating entity presented to each user.

You can create any number of ECS instances at any time according to your business needs, without having to purchase hardware in advance. As your business grows, you can resize the disks and increase the bandwidth of your ECS instances. You can release the resources when you do not need them, so that you can save your cost.

ECS instances themselves do not have high availability and disaster tolerance functions. These are realized in architecture construction.

**SLB**

 [Server Load Balancer](https://www.alibabacloud.com/help/doc-detail/27539.htm) is a traffic distribution control service that distributes traffic to multiple backend ECS instances based on the forwarding policies.  SLB expands application service capabilities and enhances application availability.

Server Load Balancer sets a virtual service address to virtualize ECS instances into an application service pool featuring high performance and high availability. It distributes requests from clients to the ECS instances in the ECS pool based on forwarding rules.

The following two features allow Server Load Balancer to improve the availability and disaster tolerance of servers:

-   The Server Load Balancer service is deployed in clusters. Each cluster has a certain number of nodes to avoid SPOF risks. This means that the Server Load Balancer service is not affected if one or several node servers go offline.

    The Layer-4 Server Load Balancer \(LVS\) service, Layer-7 Server Load Balancer \(Tengine\) service, control system, and other key components in the Server Load Balancer system are all deployed in clusters to improve their scalability and availability.

-   Currently, most Server Load Balancer instances are multi-zone instances, with master and backup instances located in the data centers of different zones in the same city. If the data center in which the master instance is located experiences a fault, services can quickly fail over to the backup instance. This provides disaster tolerance and high availability of services. Click [here](https://www.alibabacloud.com/help/doc-detail/27654.htm) for information on the distribution of multiple zones in each region.


**RDS**

[Relational Database Service \(RDS\)](https://www.alibabacloud.com/help/doc-detail/26092.htm)  is a stable, reliable, and elastically scalable online database service.  Based on Alibaba Cloud's distributed file system and high-performance storage, RDS supports the MySQL, SQL Server, PostgreSQL, and PPAS \(Postgre Plus Advanced Server, a database highly compatible with Oracle\) engines. It provides a complete set of solutions for disaster tolerance, backup, recovery, monitoring, migration, and other functions to eliminate database operation and maintenance difficulties.

-   For information on the single-host basic version of RDS, please click [here](https://www.alibabacloud.com/help/doc-detail/48980.htm).
-   In the dual-host high-availability version of RDS, master and backup instances are located in the same zone. If the master instance encounters a fault, it will fail over to the backup instance, thereby providing high availability and disaster tolerance.
-   - In multi-zone RDS, master and backup instances are located in different zones.
-   - You can use DTS to synchronize and migrate data between RDS instances.

**OSS**

[Object Storage Service \(OSS\)](https://www.alibabacloud.com/help/doc-detail/31817.htm) is a massive, secure, cost-effective, and highly reliable cloud storage service provided by Alibaba Cloud.  You can upload and download data for any application, anytime, and anywhere by calling APIs, and perform simple data management operations on the web console. OSS can store any type of files and is therefore suitable for various websites, development enterprises, and developers. You are only billed for the capacity you actually use, so you can focus on your core business.

Files are chunked for storage. By default, three copies of each chunk are saved on ChunkServer nodes in different racks. In the Pangu cluster, up to one Master and two Chunkservers can go offline without affecting services, while multiple KVServers and WS servers can go offline.

The following section presents the architecture and construction process of service high availability and disaster tolerance in detail.

## Multi-zone Server Load Balancer + ECS instances in different zones {#section_lqc_nkv_k2b .section}

As shown in the following figure, ECS instances are bound to different zones under a Server Load Balancer instance. This way, when Zone A works normally, user access traffic will follow the path of the blue solid line shown in the figure. When a fault occurs in Zone A, user access traffic will be distributed to the path of the black dotted line. This prevents a fault in a single zone from causing service unavailability, and reduces latency by selecting zones between different products.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6962_en-US.png)

The construction process for this architecture is described in the following steps.

1.  Log on to the Alibaba Cloud console, select **Server Load Balancer**, and click **Create Server Load Balancer** in the upper right corner.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6965_en-US.png)

    We use the region North China 2 as an example and purchase a multi-zone instance, with primary zone B and backup zone A.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6970_en-US.png)

2.  Create ECS instances in both the primary and backup zones of Server Load Balancer.

    We create a test instance in zones A and B of the North China 2 region. In this example, we use the default security group and choose VPC instances with a 1-core 1 GB memory CentOS 7.2 configuration.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6972_en-US.png)

3.  Create listeners and add backend servers.
    1.  In the load balancing console interface, locate the instance you created, and click administration.
    2.  Click **Backend Server** and select Excluded Servers. Then, find the corresponding instances and click **Add**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6973_en-US.png)

    3.  After completing the process, you can view the corresponding ECS instances and their weights in the Included Servers interface.
    4.  Click the Listener bar on the left and select **Add Listener**.  In this example, we use the layer-4 TCP mode, set the listener port as port 80, set the backend forwarding port as port 80, and use the default weighted round robin method. We also enable session persistence and use the default 1,000 second time-out time.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6976_en-US.png)

    5.  Set the health check to TCP mode and check the backend port 80.
    6.  After completing these steps, you can view the added listener and its status on the Listener page.

        **Note:** For subsequent users, you only need to deploy the relevant service on the ECS instance and listen to port 80. Then, resolve the domain name to the Server Load Balancer’s public IP address, so the Server Load Balancer can forward requests to backend ECS instances and provide service.


## Multi-zone Server Load Balancer + ECS instances in different zones + highly-available RDS {#section_dwg_klv_k2b .section}

The multi-zone RDS architecture is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6977_en-US.png)

For regions without multi-zone RDS, you can create an RDS instance in each zone, with the backup zone used as the backup database. This database is synchronized with the RDS instance in the primary zone.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6978_en-US.png)

The construction process for a multi-zone RDS architecture is as follows:

1.  After deploying multi-zone Server Load Balancer and ECS instances in different zones, purchase RDS instances.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6980_en-US.png)

2.  Select a region that supports multi-zone RDS, as shown in the following figure.
3.  After purchasing the RDS instance, you can view it on the console.

    Likewise, you can view the RDS high availability information and switch between master and backup instances on the console, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6982_en-US.png)


The following section gives an example of RDS construction in different zones.

1.  Purchase dual-host high-availability RDS instances respectively in zone A and zone B.
2.  Create a DTS synchronization job.

## High availability - remote disaster tolerance {#section_owg_klv_k2b .section}

When there are multiple zones in a single city and an environment is deployed in a remote region as well, the resulting architecture greatly increases service availability and achieves the goal of remote disaster tolerance.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15079/6989_en-US.png)

**Note:** The ultimate service access region can be achieved by configuring the DNS resolution. At the same time, RDS uses DTS synchronization.

