# Serverless use cases {#concept_zr2_ftm_j2b .concept}

Serverless architectures have rapidly emerged as a new technology concept over recent years. Using this architecture, you can create a variety of applications for various industries. If you have a need for computation-light, highly-flexible, stateless applications, you can familiarize yourself with the basic concepts of serverless architectures and draw inspiration from this document.

## Serverless evolution {#section_v2r_bbn_j2b .section}

On the Internet, people often draw parallels with human evolution to explain the evolution of serverless architectures. Humans have evolved from the crawling ape which emerged in to a squatting ape, a standing humanoid, all the way to modern humans with their tools.

Each stage in human evolution was accompanied by an increase in productivity. Likewise, the history of IT computing also tells a story of gradual increases in productivity, as shown in the following diagram:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6787_en-US.png)

This development process is marked by several milestones:

1.  Virtualization technology virtualizes large physical servers into individual VM resources.
2.  Virtualization clusters are moved to cloud computing platforms for simple O&M.
3.  Each VM is subdivided into Docker containers based on the principle of minimizing the operating space.
4.  Applications built on Docker containers do not require any runtime environment management, only a serverless architecture for the core code.

Therefore, this is also an evolution of IT architectures driven by a series of technological revolutions. In this process, resources have been broken down, operational efficiency has increased, and software maintenance has been simplified. The evolution of IT architecture has the following characteristics:

-   Hardware resources become more granular.
-   Resource utilization increases.
-   O&M work is gradually reduced.
-   Businesses are more focused on the code layer.

**Serverless architectures has the following features:**

-   Granular computing resources are allocated.
-   Resources do not need to be pre-allocated.
-   In a real sense, the architectures are becoming highly scalable and flexible.
-   Users only use and pay for the resources they need.

Based on these general features of serverless architectures, let's now look at several typical cases that you can use as references.

## What are the typical applications of Serverless architecture? {#section_bmd_1zw_k2b .section}

**Event request**

-   Custom images

    When online sellers maintain images of their products, they must dynamically cut the images into different sizes based on the product display location or add different image watermarks. When online sellers maintain images of their products, they must dynamically cut the images into different sizes based on the product display location or add different image watermarks. When sellers upload images to [OSS](https://www.alibabacloud.com/product/oss), they can use [Function Compute](https://www.alibabacloud.com/product/function-compute) to create custom function computing triggers.  Based on computing rules, sellers can generate images of different sizes to meet their online product display needs. This process does not require the construction of additional servers or manual intervention to ensure the aesthetics of the webpage.

-   Low frequency requests in IoT applications

    In the IoT industry, IoT devices transmit small quantities of data, often at regular intervals. Therefore, there are scenarios involving low frequency requests. For example: An IoT application may only run for 50 ms once a minute. This means that the CPU utilization is 0.1% per hour. In other words, 1,000 identical applications can share the same computing resources. Moreover, in a serverless architecture, you can purchase 100 ms of CPU resources per minute to meet your computing needs. This not only effectively solves efficiency issues, but also reduces usage costs.

-   Custom events

    During user registration, an email is sent to verify the user's email address. Likewise, you can create custom events to trigger subsequent registration processes without configuring additional applications or using servers to process subsequent requests.

-   Fixed time triggers

    Events can be triggered at fixed times. For example, you can have your service process transaction data submitted during busy periods at night or when the service is idle. Or, you can run batch data to generate data reports. The serverless method removes the need for additional processing resources, which are less often used.


**Traffic spike**

-   Elastic scaling can cope with traffic bursts.

    Mobile Internet applications are often grappling with sudden traffic spikes. For example: A mobile application may usually have a traffic rate of 20 QPS, but every five minutes see an increase to 200 QPS \(10 times the normal rate\) sustained for 10 seconds. With a traditional architecture, enterprises must scale their hardware capabilities to 200 QPS to deal with these business peaks, even though these peaks account for only 4% of the overall operating time.

    With a serverless architecture, you can take advantage of elastic scalability to quickly increase your computing capabilities to meet the current demand. Then, after a business peak, the resources are automatically released to reduce costs.

-   Transcoding and traffic capacity scaling

    During a live video activity, as you cannot predict how many viewers may access the livestream, you can scale your transcoding and traffic capabilities using the Function method. This means you no longer have to consider concurrency or traffic capacity scaling issues.


**Big data processing**

Due to security audit issues, you may need to locate access logs with specific keywords in your OSS data accumulated over the past year for multiple regions \(one file is created each hour\) and simultaneously perform aggregation operations \(to compute total values\). If you use Alibaba Cloud Function Compute, you can run function computing on your access logs every two hours during peak periods or every four hours during non-peak periods and then store the processing results in RDS. You can use one function to dispatch data to another function so as to run thousands of identical instances.

This way, you can run nearly a thousand compute functions \(24 x 365 / 10\) in less than a minute. When performing the same computing operation using ECS and computing scripts, it is quite a task just to configure a network solely for these instances \(different regions cannot download OSS files over the intranet\). In addition, instances may overnumber IP addresses available in the subnet \(for example, if your VPC uses a 24-bit subnet mask\).

Below we will use Alibaba Cloud's Function Compute product to discuss the architectures in various use cases and illustrate how you can solve difficulties specific to these cases. Alibaba Cloud Function Compute is a fully-hosted product based on a serverless architecture. You only need to upload your core code to Function Compute to run the code using event sources or SDKs and APIs. Function Compute prepares the runtime environment and dynamically scales the environment based on your request peaks. You are billed for Function Compute based on the time you use it. After requests are processed, billing stops. This reduces costs for applications with significant fluctuations in request volumes.

The following figure shows a developer trial operation procedure for Function Compute:\*\*

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6791_en-US.png)

1.  This product currently supports the Java, NodeJS, and Python programming languages.
2.  You can use an API or SDK to upload your code to Function Compute. In addition, you can perform this operation on the console or use the Fcli command line tool.
3.  You can trigger the execution of Function Compute using the API or SDK, or through cloud product event sources.
4.  In the execution process, Function Compute is dynamically resized based on the user request volume to ensure successful execution during request peaks. This process is transparent to users.
5.  After the function is executed, you can view the fees recorded in your bill. You are billed based on the time you use the product in minimum units of 100 ms.

Below we will explain several serverless use cases in detail to increase your understanding of the serverless architecture.

## Scenario 1: Event-triggered computing {#section_mch_gpn_j2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6793_en-US.png)

**Scenario**

Users upload various file types \(including images, videos, and text files\) to OSS from a mobile phone, web application, or PC. Then, the OSS PutObject event triggers Function Compute to process the uploaded files.

**Typical scenarios**

After a user uploads video files to OSS, Function Compute is triggered to obtain and transmit the object metadata to the core algorithm library. Based on the algorithm, the core algorithm library pushes the relevant video files to the CDN origin site, hot-loading the specified video. In another scenario, after video files are uploaded to OSS, Function Compute is triggered to synchronize multiple transcoding rates and store the processed video files in OSS. This provides a lightweight data processing solution.

In multimedia processing scenarios, massive volumes of files are often uploaded to OSS and then must be processed \(for example, watermarking, transcoding, fetching file attribute data\). In such a scenario, you must solve the following technical difficulties:

-   How to trigger events after files are uploaded. Generally, a custom message channel is created to receive OSS event notifications. In addition, you must construct a runtime environment and write the relevant code to process the event notifications.
-   How to efficiently process massive volumes of uploaded files.
-   How to seamlessly connect multiple cloud products.

Function Compute provides a solution that can easily address these technical difficulties:

-   Function Compute can set OSS triggers to receive event notifications. In Function Compute, you can write business code to process files and transmit files to OSS over the intranet. The entire process is simple and scalable.
-   You can build your core code into Function Compute and use the code to concurrently process event notifications.
-   Function Compute currently supports internal interaction with other products. By setting simple configurations on the console, you can efficiently connect products.

**Conventional approach to event triggering:**

-   Set a message channel to receive event notifications and write the relevant business code.
-   Purchase server resources for backend data processing.
-   Design a multi-concurrent framework to process uploaded files during business peaks.
-   Activate multiple products and call the SDK code for business interaction.

**Function Compute approach:**

-   Configure event source notifications on the console and write the relevant business code.
-   Write the code to Function Compute. You do not need to manage the software or hardware environment.
-   Function Compute scales dynamically to cope with business peaks. No management is required.
-   Function Compute's built-in connectivity with many other products allows you to seamlessly connect products using simple configurations.

## Scenario 2: Elastic resizing \(live video with multiple connected microphones scenarios\) {#section_qnh_xpn_j2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6794_en-US.png)

**Scenario**

The broadcasting room client collects audio and video streams from host and audience members with connected microphones and sends them to Function Compute for multiplexing. Function Compute sends the collected data to the multiplexing service for synthesis and pushes the synthesized video stream to CDN. Viewers pull the livestream in real time to view the multiplexed and synthesized video live.

In some live video scenarios, multiple audience members interact using connected microphones, so the host is simultaneously connected to multiple microphones. The host can connect multiple audience members or friends to the screen and synthesize the picture into a single scenario, which is then provided to the livestream viewers. In this scenario, you must be aware of the following technical difficulties:

-   The number of connected microphones is not fixed, so you must consider concurrency and elasticity.
-   The livestream is not active 24 hours a day and has significant fluctuations in business access volumes.
-   Live broadcasting is an event-driven scenario involving sharp viewer fluctuations. It requires fast updates and version iterations and must rapidly incorporate the latest technologies.

Serverless architecture provides the perfect solution for these difficulties.

As a real-time audio and video forwarding cluster for the host and connected microphones, Function Compute automatically resizes multiple execution environments used to process real-time data streams based on the concurrent volume. After a business peak, the function appropriately reduces the resource volume. Code management capabilities are deployed on the cloud, allowing you to modify and maintain code iterations at any time. You no longer have to manage multiple software runtime environments.

**Conventional approach to live video streaming:**

-   Purchase a load balancer to handle concurrent traffic.
-   Purchase computing resources to process data.
-   Find a way to release hardware resources during non-peaks periods to cut costs.
-   Maintain multiple runtime environments for multiple versions.

**Function Compute approach:**

-   Write a load distribution program in the function.
-   Instead of changing runtime environments for version iteration, you only need to replace the code version.
-   You are billed based on the actual business traffic, with low or no charges during non-peak periods.

## Scenario 3: IoT data processing {#section_mpc_lqn_j2b .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6796_en-US.png)

The architecture is divided into two parts:

-   Web application: Simulates a social media content update and data processing flow. Requests from web users are forwarded from API Gateway to Function Compute for processing. Function Compute then updates the processed content in the database and updates the index. Another Function Compute instance pushes the index update to the search engine, where the new content is retrieved by external customers. This is a closed loop data process.
-   Smart devices: The IoT gateway pushes smart device statuses to Function Compute for processing. Function Compute uses an API to send messages to Mobile Push, which pushes the messages to mobile terminals for status confirmation and management.

Smart device status processing also generates several key technical difficulties. You must design an efficient non-polling technical framework to process the status data from a large number of devices to the IoT platform. Then, you need a way to efficiently and transparently transmit the processed data to other products, such as writing to a database or pushing data to mobile terminals.

**Conventional approach to IoT device status:**

-   Set a message channel to receive event notifications and write the relevant business code.
-   Purchase server resources for backend data processing.
-   Activate multiple products and call the SDK code for business interaction.
-   Maintain the relevant hardware and software environments.

**Function Compute approach:**

-   Customize IoT platform event notifications and directly write the relevant business code in Function Compute.
-   You can release the running environment immediately after you use it.
-   Use console configurations to transparently transmit information to the relevant products.

By comparing these two methods, we can see that the Function Compute solution is more universal and greatly reduces the maintenance workload.

## Scenario 4: Shared dispatch system details {#section_rpc_lqn_j2b .section}

Customers can use a dispatch platform to choose from the services provided by various sellers, such as ordering food or buying products. The dispatch platform then notifies the nearest delivery staff to pick up the relevant product from the nearest seller and deliver the product to the customer. A simple process is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6797_en-US.png)

**Process details:**

1.  The customer notifies the dispatch platform to order a product.
2.  The dispatch platform notifies the nearest delivery staff.
3.  The dispatch platform simultaneously notifies the seller to sell the product.
4.  The delivery staff goes to the specified seller to pick up the product.
5.  The delivery staff delivers the product to the customer's location.

**In this dispatch scenario, you must solve several technical difficulties:**

-   Various resources have to be integrated. The computing resources involve delivery staff location information, optimal routes, vehicle conditions, and scheduling systems.
-   Low latency: The dispatch system requires fast order response. The entire process from the time the seller receives the order to the order's delivery to the customer must be completed within a given period.
-   Massive data volume: Three types of data are involved: customer data, seller data, and platform delivery staff data, including location information and product information.
-   Significant request volume fluctuations: The resources required by the dispatch system fluctuate throughout the day, involving certain peak times \(such as lunch or dinner for food delivery services\) and non-peak periods.

After you choose an Alibaba Cloud product solution, Function Compute works with other products to effectively solve these problems. The solution's architecture is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15081/6798_en-US.png)

**Process details:**

1.  The customer app transparently transmits an order request through API Gateway to Function Compute.
2.  Function Compute transmits the processed data to Table Store.
3.  Table Store stores route data, seller information, and location information.

    **Note:** Here, the route logs are stored in Log Service to facilitate subsequent reporting and analysis. In the delivery process, delivery staff's profile pictures and street views are stored in OSS. Function Compute can be used to pull delivery staff locations from third-party map information, such as AutoNavi Maps.


In this solution, Function Compute can provide dynamic resizing capabilities, while API Gateway performs authentication and ensures secure access. Function Compute can communicate with multiple products to seamlessly use other resources and content. All processed data is stored in Table Store databases, and all logs are directly loaded to Log Service for subsequent data reporting.

**Conventional approach to shared dispatch systems:**

-   Purchase multiple servers to support peak traffic. Independently configure release principles for low traffic periods.
-   Write code to allow interaction among multiple products.
-   Purchase the relevant products to support load balancing.
-   Maintain the relevant hardware and software environments.

**Function Compute approach:**

-   Customize IoT platform event notifications and directly write the relevant business code in Function Compute.
-   You can release the running environment immediately after you use it.
-   Use console configurations to transparently transmit information to the relevant products.
-   Both solutions are effective, but the serverless architecture is superior in terms of resource utilization and maintenance.

## Summary {#section_zpc_lqn_j2b .section}

Through the preceding scenarios, we can reach the following conclusion: In event-triggered scenarios, scenarios with business traffic fluctuations, and iterative scenarios that require rapid connection to multiple other products, Function Compute provides an optimal solution to address cost, efficiency, and connectivity considerations.

| |Function Compute metric reference|Self-built Computing Environment|
|:-|:--------------------------------|:-------------------------------|
|Maintainability| -   Easy built-in connectivity for API Gateway, OSS, Table Store, IoTHub, Log Service, Message Service, Datahub, and other products. The sandbox execution environment requires no configuration.
-   Automatic scaling and load balancing are provided.
-   Simple trigger conditions, multiple portals.

 | -   You must write code to connect to other products, which requires a certain level of technical capabilities.
-   The self-built physical environment requires you to configure runtime environments. This consumes human and physical resources.
-   You must independently construct scaling mechanisms and load balancers, and this is time consuming.

 |
|Reliability|The code and configurations are stored in OSS with automatic, multi-level redundant backup.| -   Restricted by hardware reliability, self-built environments are prone to errors. If the runtime environment or data is damaged, data may be irretrievably lost.
-   Manual data recovery is complex and takes much time and effort.

 |
|Cost| -   You are billed based on usage, so fees are low during low request periods.
-   Uplink traffic is free.
-   No maintenance staff or hosting fees.
-   Internal transmission is free between Alibaba Cloud products.
-   Given equal computing power, you save up to 1/3.

 | -   You must resize resources to cope with business request peaks. This produces a waste of resources during non-peak periods.
-   Designated personnel are needed to maintain runtime environments and hardware resources, resulting in high HR costs.
-   Communication between products over the Internet may result in additional traffic fees.

 |
|Security| -   The sandbox runs in Alibaba Cloud's enterprise-grade security environment.
-   Server-level isolation separates different users.
-   The product provides various service authorizations and primary and subaccounts.

 | -   You must separately purchase traffic cleansing and black hole equipment.
-   Access security mechanisms must be implemented independently.

 |

Although Function Compute applies in many scenarios, it is not a one-fits-all solution. For example, if your business does not experience significant request fluctuations during the day, Function Computing does not save much.

As emerging technology, serverless frameworks have yet to support more development tools with the general edition still in the pipeline. In addition, Function Compute's execution environment does not record states, so serverless frameworks are not well suited to tightly coupled applications. Due to the limited amount of resources available for allocation, it is no easy job to split up and launch certain large applications.

