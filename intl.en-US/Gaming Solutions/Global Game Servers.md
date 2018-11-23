# Global Game Servers {#concept_d1y_h4s_l2b .concept}

## Industry Overview {#section_wmc_11k_l2b .section}

The game industry is evolving each day at a magnificent speed. Along with which, the international consumer base is growing manifold. Hence, to enhance players’ experience becomes the prime objective of all game industries. A player with a high latency internet connection may show slow responses, and a disconnection in a flash can render all players offline. Therefore, reliable servers, real-time data analysis and low-latency are essential for high-quality contents and outstanding player experience, which makes gaming a natural fit to run on cloud. Moreover, gaming companies no longer need to estimate the game servers they will need or even to make additional purchases in advance, for cloud computing offers flexible and scalable infrastructure following the online player number.

Mobile gaming is now the largest segment. With mobile technologies developed enough to handle medium-to-heavy workload games, many console gaming companies have entered the mobile segment.

The trends of mobile gaming toward innovation, specification, heavy workload, VR/AR, return of classic copyright, going international with global servers make cloud computing the best choice for mobile gaming companies to obtain reliable backend and suitable architecture supports for their business.

In 2016, Supercell launched a new game - Clash Royale, which laid the benchmark of the global server architecture in the gaming industry. In China, Clash of Kings, a game developed by Elex Technology, also achieved remarkable success. Following the success ladder of these games, many mobile gaming companies choose global server architecture as a solution to go global.

## Technical Challenges {#section_efv_b1k_l2b .section}

-   Architecture design

    The key challenge to the Global Server architecture for mobile games is how to design and deploy the transport layer, business layer, and data layer to fit different game genres.

-   Network latency

    Latency is inevitable during data transmission. But how to reduce latency and minimize its effect on players’ visual experience is critical to guarantee the seamless access and fair competition of players across the world.

-   Data read and write

    Efficient data read and write, with data consistency.

-   Resource management

    Centralized and efficient game O&M and resource management.


## Why Alibaba Cloud {#section_ctb_d1k_l2b .section}

-   1\) unified management of resources

    Alibaba Cloud provides features that match games with global players and mass data with ease.

    -   Data centers with high compute power across multiple regions worldwide.
    -   One account for maintenance and resource management.
    -   Centralized presale and aftersales service system along with the localized support.
-   Sustained and secure availability

    Alibaba Cloud provides stable and low-latency network to guarantee global players seamless access and fair competition.

    -   Dedicated and consistent network connection by using Express Connect to link data centers across the world.
    -   High network quality and low latency level specified in SLAs.
-   Comprehensive suite of products

    Alibaba Cloud offers properly designed architectures and flexible deployment plans to fulfill the business logic of different genres of games.

    -   Reliable global serverless architecture and customizable deployment plans.
    -   Customer-based solutions for network latency, data consistency, and other technical difficulties.

## Business Logic Architecture {#section_g24_21k_l2b .section}

The following architecture is applicable to browser games, mobile games, and console games. It is constructed by two main modules: client services and operation services.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668017052_en-US.png)

## Global Server Game Acceleration {#section_cd1_g1k_l2b .section}

The architecture of Global Server Game Acceleration \(or Global Acceleration\) is built with the centralized game servers deployed in a single region, and edge locations deployed worldwide with the accelerated public network access. Players are geographically routed to the closest edge locations through Express Connect. This aids to minimize the latency, ensures high-quality performance and availability and to realize the global acceleration.

Currently, the prime pain area of global server games is the unfair gaming experience caused by different network latencies of players located in different regions. The technical complications such as price and quality variations of dedicated network connection from the third-parties, the high O&M costs involved in the worldwide edge location deployment, complex proxies are to name a few.

**Mobile Accelerator**

Game developers may encounter the following problems:

-   Slow app installation and launch
-   Slow game loading
-   High latency
-   Slow interaction among the carriers like China Mobile, Unicom, and Telecom
-   Chaotic carrier IP libraries
-   High failure rate of server access
-   Low availability with non-WiFi connection
-   High packet loss rate and domain name hijacking
-   Poor interactive experience
-   Differences in gaming experience of the users spread across the world.

These problems make Alibaba Cloud Mobile Accelerator the most appropriate fit to resolve the “last mile” acceleration between clients and edge locations. See the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668017053_en-US.png)

**Global Acceleration**

With Alibaba Cloud Global Acceleration, you do not have to manually configure content delivery acceleration to edge locations, which can be complicated, and requires longtime debugging. Global Acceleration ensures you the high availability, scalability, performance, and flexible routing.

Basically, Global Acceleration provides the point-to-point acceleration by using EIPs \(elastic IP addresses\) to map the ECS instances or VPC Server Load Balancer instances on your centralized servers to the public network. Global Acceleration speeds up the cross-region and cross-country connection to the servers, as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668017054_en-US.png)

**Whole-path Acceleration**

By combining Mobile Acceleration with Global Acceleration, we can accelerate the entire data path from the server to the client. In this solution, Mobile Acceleration speeds up the connection between clients and edge locations based on dynamic routing, while Global Acceleration speeds up the connection between the centralized servers and edge locations through Express Connect. See the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668027055_en-US.png)

## Gaming Architecture {#section_qxh_s1k_l2b .section}

Based on the market research, the in-depth discussion with our clients, and our own research on game architectures, we have designed and developed the following four architectures for global server games.

**Fully-centralized deployment**

For global server games, fully-centralized deployment is the preferred choice of architecture for games that are not sensitive to network latency. In this architecture, the game access layer, business logic layer, and data layer are all centrally deployed in the same region. Players across the world, access the game over the Internet. The following figure shows the deployment architecture.

![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/63554/cn_zh/1512545354322/5.png)

**Note:** For this architecture, we recommend the [c5 family of computing-type instances](../../../../intl.en-US/Product Introduction/Instance type families.md#c5). This instance type can support most game services that are not sensitive to network latency.

**Applicable scenarios**

This architecture is suitable for global server games where players are concentrated in a certain region and the gameplay method is designed to be insensitive to network latency. If your preliminary game server architecture design is not suitable for the distributed deployment \(for example, if no data synchronization mechanism is set in the logic architecture\), you must select fully-centralized deployment when you launch your game.

**Architecture advantages and disadvantages**

-   Architecture advantages:

    -   Easy deployment
    -   Better gaming experience in the primary coverage region
    -   No data consistency issues
-   Architecture disadvantages:

    In this architecture, not all players can access through the nearest node.


**Centralized deployment and network optimization**

In this architecture, the game access layer, business logic layer, and data layer are centrally deployed in a single region. Then, Global Acceleration is deployed on the Alibaba Cloud nodes for the regions you need to cover.After using intelligent DNS for scheduling, players in the various regions automatically access the game from the nearest node. Alibaba Cloud Express Connect provides an intranet connection between the game service and the various access points. The following figure shows the deployment architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668027057_en-US.png)

**Note:** We recommend using the[sn1ne family of computing network enhanced instances](../../../../intl.en-US/Product Introduction/Instance type families.md#sn1ne). This instance type is an enhanced network model, which can meet the architecture’s low-latency needs.

**Applicable scenarios**

This solution is appropriate for game server architectures that are unsuitable for the distributed deployment. It is a better choice for operators who want to cover as many regions as possible, while keeping the game’s network latency below 200ms.

**Architecture advantages and disadvantages**

-   Architecture advantages:

    -   Easy deployment
    -   Network acceleration
    -   No data consistency issues
-   Architecture disadvantages:

    Fixed latency \(For some games, differences in fixed latency can lead to an unfair gameplay. In this case, you must use frame synchronization or other methods to eliminate the latency difference\).


**Centralized data and distributed logic**

In this architecture, the data layer is centrally deployed in a single data center. Then, the game access layer, business logic layer, and cache layer are deployed in each of the regions that must be covered. The distributed architecture is shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668027058_en-US.png)

**Note:** We recommend using one of the following two instance type families:

-   [The se1ne family of memory network enhanced instances](../../../../intl.en-US/Product Introduction/Instance type families.md#se1ne) provides high network packet sending and receiving capabilities. As this is an enhanced network model, it also provides high PPS.
-   The [hfc5 family of high-frequency computing instances](../../../../intl.en-US/Product Introduction/Instance type families.md#hfc5) is suitable for MMO games and provides high-frequency specifications.

**Applicable scenarios**

This architecture is suitable for games where players mostly interact with others in the same region and that have high requirements for network latency \(for example, below 120ms, the minimum latency noticeable by human eyes\). This architecture is a good choice for action games that want to equally cover all regions of the world.

**Architecture advantages and disadvantages**

-   Architecture advantages:

    -   Players can access through the nearest node
    -   The game logic is computed on the nearest node \(local cache server data sync function: After a player exits, data is written back to the database in real time, 100 rows of dirty data is written every 5 seconds\)
    -   Almost no data consistency issues
    -   Flexible distributed node adjustment
-   Architecture disadvantages:

    -   Must be deployed in multiple regions
    -   When players interact across regions, the latency of one party increases \(a special solution can resolve the problem of latency differences\)
    -   A complete dirty data writeback function is required to ensure data consistency

**Key design aspects**

This section gives a detailed description of several key design aspects.

-   Key aspect 1: Centralized game database deployment

    In global server games, gaming rule interaction may occur between any players. Therefore, gaming data, player account data, and global game data \(such as rankings\) between, must be centrally deployed in a single IDC.

    The player data read/write frequency is high and a large proportion of records have a single line. Therefore, it is best to use distributed storage architecture. For example, you can use the Alibaba Cloud DRDS and ApsaraDB for RDS products for a database and table-based splitting. This avoids the performance bottleneck of a single database instance.

-   Key aspect 2: Regional player access

    As this type of game is a service provided to players around the world and access to Chinese networks from other countries may be poor, you need to provide the nearest access to the players in various regions around the world.

    For example, based on the distribution of Alibaba Cloud data centers, you could deploy access nodes in South China, North China, Southeast Asia, Europe, and North America. Specifically, you can deploy access services in the China East 2, China North 2, Singapore, Germany, and the East US regions.

-   Key aspect 3: Player data is regionally cached and regularly written back to the central data center

    The players in various regions play together. To avoid the network latency of the remote data reading to affect the overall gaming experience, player data must be regionally cached and then regularly written back to the central data center in batches. This way, the regional game logic servers only need to remotely read data once, during player log on. Then, they can quickly read player data from the cache server.

    For example, you can use the Alibaba Redis product for caching, allowing you to implement data caching and persistence. In this way, you do not lose data even if the leased line connection is unavailable.

-   Key aspect 4: Intelligent DNS allows nearby access

    When players from around the world access the game, the best option is to use the intelligent DNS service for auto scheduling. You can also create your own scheduling service. During scheduling, players’ locations must be used to schedule players in the same region to the same access point. If the gameplay involves player matching, the matching algorithm must consider the player’s location.

-   Key aspect 5: Players playing against each other should be controlled within the same region as much as possible

    Because a game logic server is deployed in each region and player data is cached regionally, the backend should limit direct combat to players in the same region as far as possible.

-   Key aspect 6: Centralized storage of global data

    Because rankings and other global data is generated by summarizing data from all regions, this data must be centrally stored. Then, each region can regularly pull the necessary global data \(the data pull interval must be set according to the ranking generation cycle\). However, the previous data version in the local cache cannot be deleted before the latest data is pulled, and the services used to generate global data must also be centrally deployed.

-   Key aspect 7: Cross-region player access

    It is possible that a player may log on from different regions, during each logon, the system must check if the current logon access point is the same as the last access point used. If they are different, the player data in the cache of the previously used access point must be written back to the database. Then, the player is permitted to log on from the new access node. This prevents data inconsistencies.


**Fully-distributed deployment**

In this architecture, the game logic and game data are deployed in a distributed manner. Only global game services and data are centrally deployed. This architecture is suitable for games with low read/write frequencies and less-strict network latency requirements. The following figure shows the deployment architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668027059_en-US.png)

**Note:** We recommend using one of the following three instance type families:

-   The cm4 family of high-frequency general instances is suitable for fully-distributed architectures with high requirements for network, computing, and storage resources.
-   The se1ne family of memory network enhanced instances provides high network packet sending and receiving capabilities.
-   The g5 family of general instances provides a distributed cache function while balancing the ratios of various resources.

**Applicable scenarios**

This architecture is suitable for games with high network latency requirements, a great deal of interaction between players in different regions, equivalent coverage in all regions, and a complete data synchronization mechanism in the game architecture.

**Architecture advantages and disadvantages**

-   Architecture advantages:

    -   Players can access through the nearest node
    -   Game logic is computed on the nearest node
    -   The game business logic layer is completely stateless
    -   Fast data reading and writing speed
-   Architecture disadvantages:

    -   Must be deployed in multiple regions
    -   Large amounts of data must be synchronized across different regions

**Key design aspects**

This section gives a detailed description of several key design aspects.

-   Key aspect 1: Centralized storage of global data

    As the rankings and other global data are generated by summarizing data from all regions, this data must be centrally stored. Then, each region can regularly pull the necessary global data \(the data pull interval must be set according to the ranking generation cycle\). However, the previous data version in the local cache cannot be deleted before the latest data is obtained, and the services used to generate global data must also be centrally deployed.

-   Key aspect 2: Regional player access

    As this type of game is a service provided to players around the world and access to Chinese networks from other countries may be poor, you need to provide nearby access for players in multiple regions around the world. For example, based on the distribution of Alibaba Cloud data centers, you could deploy access nodes in South China, North China, Southeast Asia, Europe, and North America. Specifically, you can deploy access services in the China East 2, China North 2, Singapore, Germany, and US East regions.

-   Key aspect 3: Intelligent DNS allows nearby access

    When players from around the world access the game, it is best to use the intelligent DNS service for automatic scheduling. You can also create your own scheduling service. During scheduling, players’ locations must be used to schedule players in the same region to the same access point. If the gameplay involves player matching, the matching algorithm must consider the player’s location.

-   Key aspect 4: Real-time game database synchronization

    This architecture allows players in different regions to play across servers. Therefore, the game databases of the different regions must be synchronized. You can use Alibaba Cloud DTS for real-time data synchronization, or migrate data when players interact across servers. The data synchronization method used in this solution is described as a special solution later in this article.


## Cloud product introduction {#section_e4n_dj4_k2b .section}

**ECS product introduction**

Elastic Compute Service \(ECS\) is a basic cloud computing service provided by Alibaba Cloud. You can create any number of ECS instances at any time according to your business needs, without having to purchase hardware in advance. As your business grows, you can resize the disks and increase the bandwidth of your ECS instances. When you no longer need an ECS instance, you can release it to reduce your fees. An ECS instance is a virtual computing environment which includes a CPU, memory, operating system, disks, bandwidth and other basic server components.

It is the actual operating entity presented to each user. For more information, refer to [ECS](../../../../intl.en-US/Product Introduction/What is ECS?.md#).

**ApsaraDB for RDS product introduction**

Alibaba Cloud ApsaraDB for RDS \(Relational Database Service\) is a stable, reliable, and elastically scalable online database service.

Based on Alibaba Cloud’s distributed file system and high-performance storage, ApsaraDB for RDS supports the MySQL, SQL Server, PostgreSQL, and PPAS \(Postgre Plus Advanced Server, a database highly compatible with Oracle\) engines. It provides a complete set of solutions for disaster tolerance, backup, recovery, monitoring, migration, and other functions. For more information, refer to [ApsaraDB for RDS](../../../../intl.en-US/Product Introduction/What is RDS.md#).

**Redis product introduction**

ApsaraDB for Redis is compatible with open-source Redis protocol standards and provides persistent memory database services. Based on its high-reliability, dual-machine hot standby architecture, and seamlessly scalable cluster architecture, this service can meet the needs of businesses that require high read/write performance and flexible capacity adjustment.

Using a memory + hard disk storage layout, ApsaraDB for Redis can meet your persistence requirements, while providing high-speed data read/write capability. For more information, refer to [ApsaraDB for Redis](../../../../intl.en-US/Product Introduction/What is ApsaraDB for Redis.md#).

**Express Connect**

Alibaba Cloud Express Connect helps you build private network communication channels between VPC instances and between a VPC instance and your data center. This increases the flexibility of the network topology and enhances the quality and security of cross-network communication. For more information, refer to [Express Connect](../../../../intl.en-US/Product Introduction/What is Express Connect?.md#).

**Global Acceleration**

Global Acceleration is a web acceleration product. Supported by Alibaba’s global backbone network, Global Acceleration enables nearest possible access globally. This helps minimize the impact of network problems such as latency, jitter, and packet loss on the quality of service, and brings a better experience to the global users of your services. At its far end, Global Acceleration only requires a GA public network IP address portal. The backend is the same game server that helps to truly achieving a global server architecture. For more information, refer to [Global Acceleration](../../../../intl.en-US/Product Introduction/What is Global Acceleration?.md#).

**Cloud resolution**

Alibaba Cloud DNS is a cloud computing service portal. It gradually integrates existing Alibaba Cloud products, forming an indispensable element in the cloud product family. ECS, ApsaraDB for RDS, CDN, Server Load Balancer, and other products provide users with efficient and reliable computing, storage, website acceleration, and load balancing services. Alibaba Cloud DNS provides a powerful and stable resolution scheduling portal. This ensures that users have a smooth access experience and provides them with an all-in-one service experience.

For more information, refer to [Alibaba Cloud DNS](https://www.alibabacloud.com/help/doc-detail/58165.htm).

## Typical system design {#section_ld3_qck_l2b .section}

**Global rankings design**

When designing a global rankings service, you must consider demand, analysis, data structure, rank data persistence, rank server SPOF issues, and other issues. A ranking service architecture is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668027060_en-US.png)

This architecture uses Redis to implement data at regular intervals. The game server reports rank data to the rank server. Clients pull rank data from the game server and the game server pulls rank data from the rank server.

**Game time design**

All the game servers use GMT Jan 1, 1970 00:00:00 offset \(generally an absolute value of 1 for the second count\) to express the in-game time. This time is synced to game clients, who use the time zone set on the cell phone to compute the game time to be displayed in the client. The specifications are shown in the figure below.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668027061_en-US.png)

Why is the game time synchronization necessary?

-   To prevent client modifications to the local time from confusing the game logic, the client must use the server time.
-   Basically all techniques to solve game status synchronization problems, such as predictive pull or server verification sync, require time synchronization.
-   In games, some timed events or time-related gameplay features require a standard and uniform game server time to ensure fair play.
-   During the client and server communication, a more secured method is to add a timestamp to each packet so that the server can verify the validity of the packets.

**Game data synchronization**

Solution 1 Use a cache for instant data writeback

In this method, data is stored centrally and the local caches instantly write data back to the database. The specific architecture is shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668037062_en-US.png)

Currently, there are two main scenarios that involve data synchronization:

Scenario 1 Players log on in their local regions \(explanation marked in red text in the previous figure\).

1.  The database proxy reads role data from the database.
2.  The database proxy inserts the role data into the cache.
3.  The role data read from the cache is used in computing.
4.  When the data changes, the cache data and database data are updated simultaneously.
5.  When players exit the game, the data in the cache is deleted.

Scenario 2 Players log on across regions.

1.  First, the system checks if the player is logged on in another region. If yes, it goes to step 2. If no, it goes to step 3.
2.  The player’s role data is written back to the database and deleted from the cache when the player is logged out.
3.  The database proxy reads role data from the database.
4.  The database proxy inserts the role data into the cache.
5.  The role data read from the cache is used in computing.
6.  When the data changes, the cache data and database data are updated simultaneously.
7.  When players exit the game, the data in the cache is deleted.

Solution 2 Real-time regional database synchronization

The various regional databases are synchronized in real time. You can accomplish this using Message Service or Alibaba Cloud DTS. Each of the regional databases store data for all players.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668037063_en-US.png)

-   Advantages:

    The players can play the game from different regions, their data need not be migrated. All regions support local data reading and writing.

-   Disadvantages:

    The asynchronous data synchronization can produce data inconsistencies. If the player uses a VPN to access the game, a transient VPN disconnection can cause the player to log on again in another region. If data synchronization messages are lost or delayed, the data read when a player logs in again may not be up to date.

    The various regions contain all player data, so real-time synchronization may put a high level of pressure on the database. Real-time synchronization between regional databases demands a great deal of cross-region leased line bandwidth.


Solution 3 Cross-region data update

When a player logs on from a different region, the player’s local server must remotely read combat data. The combat results call an API to trigger a data update on the local server.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668037064_en-US.png)

-   Advantages:

    For players in the same region, role data is read/written to/from the nearest local node.

-   Disadvantages:

    The implementation logic is complicated. All gameplay results must be abstracted to an interface and data is centrally changed by the original server interface.


Solution 4 Remote data migration

When a player logs on from a different region, the player data is remotely migrated from the player’s previous server. Each time a player logs on, the system must check if data migration is required.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668037065_en-US.png)

**Game localization solutions**

Game localization is an important factor for consideration of games seeking to enter overseas markets. Games, either being released overseas or looking to expand to new regions must be attentive to localization work. The general approach is to build a standard client installation package, which contains several basic material packages, art material packages for different languages, and some program materials. This allows the game to be dynamically rendered based on the phone’s language version.

There are three common installation strategies:

-   After the client installation package is downloaded, the user manually sets the language version. Such large packages contain various language packs and support one-click language selection.
-   When the game is installed on the client, it detects the language used by the mobile device to dynamically select the language version to install. These installation packages generally have a built-in default language and, if another language is needed, a language pack is downloaded from the Internet.
-   Different language versions of the client installation package are submitted to the app store. Then, different installation package versions are served to different regions. Each installation package must be customized.

## Case studies {#section_hkc_tdk_l2b .section}

**Case study 1 Game A**

Game A is a global server card game. Currently, the game server is deployed in the Alibaba Cloud US West 1 region. The gameplay is not very sensitive to network latency and a latency below 300 ms does not affect the gaming experience. Therefore, the customer did not plan for any network access optimization or distributed deployment, adopting the fully-centralized deployment architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668037066_en-US.png)

**Case study 2 Game B**

Game B is a global real-time multiplayer war game. The game adopted the global server game reference architecture with centralized deployment and network optimization.

The game’s access layer, business logic layer, and data layer services are all deployed in the Alibaba Cloud US West 1 region. Chinese players use Alibaba Cloud public network BGP to access Express Connect and connect to the VPC for the US West 1 region. Global Acceleration is deployed in China North 2 and set as the access layer’s public network portal, with intelligent DNS used for traffic scheduling. This layout improves the game access speed for players on the Chinese mainland.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15628/15429668037067_en-US.png)

## Conclusion and future prospects {#section_dvs_vdk_l2b .section}

By describing business needs, technical difficulties, cloud products, player acceleration methods, reference architectures, and typical designs, this article aims to provide complete solutions to more customers who want to develop global server games.

Alibaba Cloud already offers the following technical solutions to address a series of technical difficulties \(such as time synchronization, localization, and latency\) faced by global server games:

-   Distributed deployment cross-region data synchronization
-   Global serverless game time \(such as GMT\)
-   Global serverless game localization \(such as text, materials, and code\)
-   Global serverless latency elimination \(such as server frame sync\)

In future, Alibaba Cloud plans to perfect and provide general solutions and solutions for different global server game architectures:

-   Distributed node traffic proxy construction best practices
-   Global server SLG game architecture solutions
-   Global server card/board game architecture solutions

In a nutshell, Alibaba Cloud’s global data centers and Express Connect form a global network that assists the global deployment of games.

