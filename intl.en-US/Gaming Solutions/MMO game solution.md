# MMO game solution {#concept_u1l_4tt_j2b .concept}

## Industry overview {#section_st5_wrp_k2b .section}

As the gaming industry expands, casual games and card and board games cannot keep up with gamers' demand for high-quality games. Meanwhile, the rapid increase in mobile terminal hardware configurations allows them to run MMO RPG \(Massively Multiplayer Online Role Playing Game\), MOBA \(Multiplayer Online Battle Arena\), and other medium-to-heavyweight games. In this context, more and more MMO games are released.

MMO client products are now entering the mobile game product market. This means that cloud services are tasked with more and more support responsibilities as the backend servers for the games. A reasonable platform architecture is the foundation that ensures stable system operation and business development.

Currently, the homogenization of game products is a serious problem. Game vendors are gradually coming back down to earth after an era of explosive growth. In the future, they must attempt to create higher-quality and globalized games. Only the top vendors and the most powerful newcomers will survive the current upheaval in the industry. Going forward, MMO games will continue to be an extremely important part of the high-quality game market.

## Technical challenges { .section}

-   Large bandwidth and high package volume

    MMO games generally wish to provide the largest field of vision possible, and movement and combat are the core gameplay aspects, which requires mutual real-time visibility between gamers on the same screen. Therefore, a large volume of movement and combat packets must be broadcast within a field of vision.

    In this case, MMO game servers produce a massive amount of communication packets when many gamers are playing simultaneously. Therefore, the access layers of MMO game servers require ample network bandwidth and high network packet throughput.

-   Resource auto scaling

    Mobile and webpage-based MMO games are generally light games with time fragmentation. As a result, the industry requires maximum conservation and utilization of game server resources in order to efficiently achieve server activation and combination for MMO game servers. This is especially true for webpage-based games.

-   High computing power

    For MMO web games, game planners hope to use strong interaction between gamers to attract new gamers. Therefore, they must increase the number of concurrent gamers in individual zones as much as possible. The maximum number of concurrent gamers in individual zones is generally required to be in the thousands. Therefore, MMO web games require strong interaction and validation as well as high game server computing capability.

-   Nearby access

    MMO games generally adopt nearby deployment models based on zone and server division and in multiple centers across different regions. This gives gamers nearby game servers, ensuring smooth gameplay and enhancing the gaming experience.


## Why Alibaba Cloud? { .section}

-   Top-level infrastructure

    -   Alibaba Cloud has self-built 5A data centers and 16 major IDCs around the world, with BGP exclusive bandwidth, 1000 gigabit optical fiber access, and over 1200 CDN nodes worldwide.
    -   Our solutions leverage Alibaba Cloud’s global network of data centers and Express Connect service to easily implement global server sharing architectures. This forms a global network, with one console able to deploy resources in China and abroad in mere seconds.
-   Comprehensive products for support

    -   Alibaba Cloud's comprehensive product lines meet the needs of different business scenarios.
    -   Our wide range of instance types provide the instance specifications and performance metrics required in various technical scenarios.
-   High frequency instances

    Alibaba Cloud provides instances with various frequencies that are able to satisfy the different CPU computing power needs of various scenarios.

    -   Our solutions meet the computing speed requirements of complex game logics.
    -   Our solutions meet the single-core processing speed requirements of old client game architectures.
-   High network throughput capacity

    Alibaba Cloud provides high network throughput ECS instances with a throughput capacity of over one million PPS to meet the needs of various high packet throughput scenarios.

    -   Our solutions provide the PPS capacity required for message broadcast scenarios for multiple gamers on a single screen.
    -   Our solutions provide the intranet throughput capacity required for public data reading.
-   Stable computing power assurance

    Alibaba Cloud provides dedicated instances to ensure the continuous and stable output of computing power.

    -   Our solutions provide the CPU computing power stability required for high load scenarios.
    -   Our solutions provide the computing speed assurance required by CPU-intensive scenarios.
-   Nearby access worldwide

    Alibaba Cloud has data centers around the world that provide nearby access for gamers and meet the needs of global server sharing infrastructures.

    -   Our solutions ensure a good access experience for gamers sharing servers around the world.
    -   Our solutions solve the last mile problem for gamer access.
-   High quality BGP network

    Alibaba Cloud's multi-line BGP networks ensure gamers experience high-quality network access, solving network access problems for gamers on different carriers.

-   Professional security protection capabilities

    Alibaba Cloud draws on the security protection experience accumulated by the Alibaba Group over more than a decade to provide comprehensive security protection solutions that extend from clients, the network layer, and the application layer to the infrastructure layer.

    -   We provide defenses against DDoS, CC, and various other types of cyberattacks.
    -   We prevent application layer attacks and penetration problems that affect login, payment, and other core business platforms.
    -   We use big data analysis to predict security risks.

        Alibaba Cloud's Anti-DDoS Service Pro is the most powerful game security protection system in the industry. It can easily defend against DDoS attacks of hundreds of gigabytes and various application layer attacks.

-   Efficient O&M support system

    -   Alibaba Cloud's wide range of product APIs greatly increase customer's O&M flexibility and efficiency.
    -   Our comprehensive monitoring and alert system effectively reduces the difficulty of O&M monitoring.
-   High-quality service experience

    -   We provide a unified presales and aftersales service system worldwide along with local service support.
    -   Our professional technical support service ensures rapid response and efficient problem resolution.

## Business function architecture {#section_dmq_rsp_k2b .section}

The overall business process and specific business modules of MMO games are shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6920_en-US.png)

## Product architecture {#section_yqf_5sp_k2b .section}

**General reference architecture solution of MMO games**

Alibaba Cloud’s complete product lines provide comprehensive solutions for all scenarios, from game downloads and updates, game business servers, game logic servers, game database servers, and game data operations platforms to game O&M monitoring platforms.

Using rational service combinations and resource configurations, you can effectively improve O&M efficiency, enhance service experiences, and reduce total operating costs.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6922_en-US.png)

**MMO game architecture - Client games**

You can use Alibaba Cloud's different products to implement all business processes or only specific processes. An MMO client game architecture is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6923_en-US.png)

**Data interaction process**

See the figure shown in the MMO game architecture - Client games part. The following list describes the specific data interactions numbered in the figure:

1.  The client connects to the gate server to initiate a login request.
2.  The gate server forwards the login request to the login server.
3.  The login server initiates an ID data verification query to the DC server.
4.  The DC server accesses the gameDB server to perform the data query and return the results.
5.  If ID verification is successful, the login server continues to query data and returns account status data \(role, level, attributes, last login scene server, geolocation, and other information\). It also synchronizes login status and information to the center server.
6.  The center server is responsible for distributing information to the corresponding scene server. At the same time, a gamer online notification is broadcast to the gamer’s friends and the online gamer status monitor \(to control reconnection after disconnection and disconnection time-out\).
7.  After the gate server receives authentication information, it establishes a connection with the scene server, and the gamer successfully logs on to the scene server.
8.  If the gamer has public information to broadcast, a request is sent to the center server. The center server performs message packet broadcast.
9.  The scene server starts writing all user behavior to the log. At the same time, the relevant gamer data storage or query request is sent to the DC server.

**Architecture features**

MMO client game architectures have the following main features:

-   The gateway server is responsible for all network packet forwarding. Generally, the network load is concentrated here, so it has high network throughput requirements.
-   The scene server contains the game logic and is relatively dependent on CPU power and requires a certain level of network packet forwarding capabilities.
-   A single game zone serves over 10,000 gamers. The logic server generally divides gamers according to scene maps.Larger scales can be achieved by line division.
-   The DC server caches gamer data and writes it to the database asynchronously. This ensures gamer clients can rapidly read and write data. It has high availability requirements, so it must use the application layer to implement data error tolerance mechanisms.
-   The log server collects and processes all service behavior logs for a region. It has high disk write performance requirements. Generally, its function is implemented by grouping multiple servers.

## MMO game architecture - Mobile games {#section_l1l_ftp_k2b .section}

You can use Alibaba Cloud's different products to implement all business processes or only specific processes. An MMO mobile game architecture is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6924_en-US.png)

**Architecture features**

MMO mobile game architectures have the following main features:

-   Compared to client games, mobile games have less complex gameplay and shorter lifecycles. Considering this feature together with operator policies and the resource economics model, mobile games usually adopt relatively simple deployment architectures. However, a minority of MMO mobile games use the client game deployment architecture.
-   Clients generally connect directly to the game server. For a small number of games, a gateway is configured at the frontend of the game server or the gateway and game server are deployed on the same machine. Mobile games are relatively dependent on the CPU power and network packet forwarding capabilities of individual servers. A single game zone generally supports 1,000 - 5,000 online gamers.
-   A dedicated game database server can be deployed for each game server, or a single game database server can be deployed for multiple zones.

**Derivative architecture of MMO mobile games**

A detailed derivative architecture of MMO mobile games is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6927_en-US.png)

-   In the derivative architecture of MMO mobile games, Server Load Balancer and gateway servers are used to organize the multiple logic servers into a large server. This server can support more online gamers to help build a gamer ecosystem.
-   The gateway server implements gamer request scheduling and connection status monitoring.
-   Battle servers \(or PVP battlefield servers\) require greater computing power and load capabilities. These servers can be constructed with high-configuration or dedicated instances, and controlled centrally by the gateway server.

## Centralized deployment architecture of MMO web games {#section_vkw_jtp_k2b .section}

When the logic server layer and game database layer adopt a centralized deployment method, the specific architecture is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6939_en-US.png)

-   Through the web game process, content loading relies on CDN. Therefore, the game requires a certain level of CDN stability and download speed. In resource loading, large file packages are generally loaded in segments, while small files can be loaded at any time.
-   Web games generally adopt the operating policy of rapid server activation and combination, so that they can activate thousands of game zones in a short period of time and gradually combine servers as the number of active gamers in each zone falls. Using custom images and automatic server activation scripts, they can quickly create game regions and logic servers.
-   Compared to client and mobile games, web games have low computing complexity. A single game zone does not require any special levels of server computing power or network throughput. Generally, a single server is used to activate multiple zones. The computing load determines how many game zones can run on each server.

## MMO page-tour partition deployment Schema {#section_r4t_ltp_k2b .section}

When the logic server layer and game database layer adopt a divisional deployment method, the specific architecture is shown in the following figure:

## General technology solution of MMO games {#section_mbb_ntp_k2b .section}

**Elastic scaling up & down for the access layer server cluster**

-   Using Auto Scaling, you can automatically scale up and scale down your access layer server cluster. This allows you to effectively cope with boot storms, battle event traffic peaks, and other scenarios, and ensure your server cluster resources have sufficient load capabilities.
-   This solution also applies to login servers and other servers that require elastic scaling.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6943_en-US.png)


**High availability of game downloads and updates - Self-built ECS origin site**

-   A multi-level download retry mechanism ensures the high availability of downloads and updates, reducing the proportion of gamers lost during this stage.
-   By splitting the back-to-source address and direct external download address, this solution avoids potential security risks from exposed addresses and Server Load Balancer unavailability.
-   You can use rsync+inotify to synchronize files across multiple origin site servers in real time.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6944_en-US.png)

**High availability of game downloads and updates - OSS origin site**

-   A multi-level download retry mechanism ensures the high availability of downloads and updates, reducing the proportion of gamers lost during this stage.
-   By splitting the back-to-source address and direct external download address, this solution avoids potential security risks from exposed addresses.
-   By setting OSS as the CDN origin site and taking advantage of OSS’s automatic remote replication function, it further improves origin site availability and throughput.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6945_en-US.png)

**Large game file downloads & foreign back-to-source - OSS origin site**

-   By deploying origin sites in China and abroad, this solution ensures high-speed and stable back-to-source operations.
-   OSS's cross-region replication function automatically synchronizes origin site files.
-   The URL push function pushes large files to L2 nodes, accelerating the speed of the first download and reducing the number of back-to-source requests.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15077/6946_en-US.png)

## Cloud product introduction {#section_e4n_dj4_k2b .section}

**ECS **

Elastic Compute Service\(ECS\) is a basic cloud computing service provided by Alibaba Cloud. You can create any number of ECS instances at any time according to your business needs, without having to purchase hardware in advance. As your business grows, you can resize the disks and increase the bandwidth of your ECS instances. You can release the resources when you do not need them, so that you can save your cost.

An ECS instance is a virtual computing environment which includes a CPU, memory, operating system, disks, bandwidth and other basic server components. It is the operating entity presented to each user.

For more information, refer to [ECS](https://www.alibabacloud.com/help/doc-detail/25367.htm).

**RDS**

Alibaba Cloud Relational Database Service \(RDS\) is a stable and reliable online database service providing the auto scaling capacity.

Based on Alibaba Cloud's distributed file system and high-performance storage, RDS supports the MySQL, SQL Server, PostgreSQL, and PPAS \(Postgre Plus Advanced Server, a database highly compatible with Oracle\) engines. It provides a complete set of solutions for disaster tolerance, backup, recovery, monitoring, migration, and other functions.

For more information, refer to [RDS](https://www.alibabacloud.com/help/doc-detail/26092.htm).

**Redis **

ApsaraDB for Redis is compatible with open-source Redis protocol standards and provides persistent memory database services. Based on its high-reliability dual-machine hot standby architecture and seamlessly scalable cluster architecture, this service can meet the needs of businesses that require high read/write performance and flexible capacity adjustment.

Using **memory + hard disk**storage, ApsaraDB for Redis can meet your data persistence requirements, while providing high-speed data read/write capability.

For more information, refer to [Redis](https://www.alibabacloud.com/help/doc-detail/26342.htm).

**MaxCompute **

MaxCompute is a big data processing platform that processes and stores massive batch structural data to provide effective data warehousing solutions and big data modeling.  MaxCompute provides comprehensive data import solutions and a variety of typical distributed computing models, enabling you to solve the massive data computing problem in a fast way, effectively reduce costs, and ensure data security.

MaxCompute is mainly used to store and compute batches of structured data. It provides data warehouse solutions for massive volumes of data, as well as big data analysis and modeling services.

MaxCompute aims to provide a convenient means of analyzing and processing massive data volumes. You no longer need to concern yourself with the details of distributed computing. Rather, you can directly and conveniently achieve your data analysis goals.

MaxCompute is already widely used within the Alibaba Group, in applications such as data warehousing and BI analysis for large-scale Internet companies, log analysis for websites, transaction analysis for e-commerce websites, and mining of user features and interests.

For more information, refer to [MaxCompute](https://www.alibabacloud.com/help/doc-detail/27800.htm).

**CloudMonitor **

CloudMonitor is a service that monitors Alibaba Cloud resources and Internet applications. The service can be used to collect monitoring metrics for Alibaba Cloud resources, to test the availability of Internet services, and to set alarms for these metrics.

For more information, refer to [CloudMonitor](https://www.alibabacloud.com/help/doc-detail/35170.htm).

## Use cases {#section_fxn_2vp_k2b .section}

Dawn of War, a mobile fantasy MMORPG independently developed by ZPLAY, is set in a world based on European mythology. The game is rich in content, with 3D mounts, costumes and accessories, and other distinctive gameplay systems, as well as a 360° view.

This game used Alibaba Cloud's cloudification solution to form a game server architecture that can effectively avoid single point of failure, support smooth resizing, and allow for flexible deployment. The high-performance ECS delivers the high computing power required by intensive game logic. The ApsaraDB for RDS read-only instances effectively support read/write splitting for MMO games.

## Summary and outlook {#section_bnz_2vp_k2b .section}

MMO games have always received a high degree of attention from the industry. Since the days of physical IDCs to the current cloud platform era, the MMO client games have evolved into mobile and web games, presenting developers with a host of technical challenges and opportunities for innovation.

This document provides a comprehensive introduction to the MMO product architecture, discussing the general reference architecture of MMO games along with MMO client game architectures, MMO mobile game architectures, and MMO web game deployment architectures. It also describes certain general technical solutions for MMO games, and briefly discusses the cloud products used in these solutions. Although this document does not discuss every scenario and issue, we hope that it will inspire customers to learn more.

As the industry develops and technology advances, Alibaba Cloud will continue to refine its various basic services, network services, security services, and big data services, providing customers with simpler, more stable, and more complete solutions.

