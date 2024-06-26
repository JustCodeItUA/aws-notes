# AWS Cloud Technical Essentials

> Written by yours https://github.com/eddienubes
 
## General

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled.png)

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%201.png)

## IAM

*Everyone with this policy is allowed to **Run EC2 Instances** for the specified resource with additional conditions(optional)* 

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%202.png)

*AWS Recommends using groups instead of attaching policy to a particular user*

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%203.png)

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%204.png)

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%205.png)

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%206.png)

*So, roles must be used for apps (apps retrieve credentials from assigned role to sign AWS API calls) and for external identity providers. Like on the picture above when we have a couple of thousands of users - we leverage “**Federated roles**”, instead of creating a couple of thousands IAM users.*

---

## EC2

*EC2 Instance lifecycle* 

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%207.png)

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%208.png)

**IMPORTANT**: keep in mind, **AMI** (Amazon Machine Image) copies all OS settings along with network configurations 

## AWS Container Services

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%209.png)

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2010.png)

There are 3 types of compute in the **Cloud:**

- Virtual Machines
- Containers
- Serverless

**AWS ECS(Elastic Container Service) VS EKS(Elastic Kubernetes Service)**

| ECS | EKS |
| --- | --- |
| Runs EC2 instances with ECS Agent installed, which are called “container instances” | Meanwhile EKS has the analogy called “Worker node” |
| EC2 container called “Task” | EKS container called “Pod” |
| ECS is AWS native | Kubernetes  |
| Multiple containers per EC2 instance  | Multiple containers per Pod instance |

---

## Serverless

**AWS Fargate**

One of the serverless examples is **AWS Fargate.** Containers on **Fargate** instances within **ECS** don’t have to be managed infrastructurely (PaaS), that means their underlying CPU/Memory resources and OS are isolated reducing the size of the platform to attack. 

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2011.png)

**AWS Lambda** 

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2012.png)

---

## Networking

**IP Address**
32-bit representation

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2013.png)

IPv4 Notation - representation of IP address in decimal format.

In the diagram below, the 32 bits are grouped into groups of 8 bits, also called octets.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2014.png)

**CIDR Notation**

**CIDR** (Classless Inter-Domain Routing) notation is a compressed way of specifying a range of IP addresses. Specifying a range determines how many IP addresses are available to you.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2015.png)

32 bits - 24 bits = 8 bits ⇒ 256 available IP addresses in the range

Useful **CIDR** bits reservation answer. Each 8 bits reserve exactly 1 slot in IPv4.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2016.png)

Example of **VPC** (Virtual Private Cloud) with 2 subnets (private/public) and IGW (Internet Gateway).

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2017.png)

Example of **VPC** isolated from public internet, but connected with on-premises data center via **VGW** - Virtual Private Gateway thus forming **VPN** (Virtual Private Network).

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2018.png)

Reserved IP-addresses within each subnet

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2019.png)

**Network Routing**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2020.png)

Let’s say user sends a request to our internet gateway and in order to choose proper subnet we have to define **Route Table.**

Example of complete network routing

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2021.png)

Key points:

- When you create **VPC AWS** creates Main Route Table. Target: “Local” and Destination: “10.1.0.0/16” mean that all subnets can communicate with each other by default.

*Target* - entry point of request. “*Local*” means any **subnet** in this **VPC**

*Destination* - possible endpoint (CIDR) 

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2022.png)

- All subnets are **private** by default. Their publicity is defined by route tables. For example if we have subnet with a route table related to **IGW -** our subnet is public.
- **CIDR** of subnets cannot overlap, so if you have multiple subnets in a **VPC** consider creating more than one **CIDR** with multiple available blocks, i.e. 10.1.0.0/16 - 16 bits / 8 bits = 2 blocks are reserved, but 256 subnets can be created, i.e. 10.1.1.0/24.

**Network ACL (Access Control List)**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2023.png)

**ACL** defines inbound and outbound traffic rules. Pink squares on the picture above are these rules. 

**IMPORTANT**: By default, even thought we have **IGW** tied with our subnets, it doesn’t mean that traffic flow is allowed, thus we have to defined additional rules. 

**Security Groups**

Security groups allow us to manage inbound/outbound traffic on the **EC2** instance level.

**IMPORTANT:** security groups are stateful, meaning they will remember if a connection is originally initiated by the **EC2** instance or from the outside and temporarily allow traffic to respond without having to modify the inbound rules. I.e. we have an application on an **EC2** instance which frequently request some third-party **API,** security group without inbound rules defined will temporary allow responses to income so our app can normally consume the traffic.  

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2024.png)

 

---

## Storages on AWS

**Storage types**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2025.png)

**Block Storage** - 1 GB of data divided into pieces. Writing is faster.
Use cases:

- Since operations here have low-latency it’s a common choice for high-performance enterprise workloads

**Object Storage** - 1 GB of data stored in 1 block, entirely. Writing slower. It’s a flat storage where each object has a unique identifier.

Use cases:

- Large data sets
- Unstructured files like media assets
- Static assets like photos

**File Storage** - files stored as on any OS file systems in the hierarchies.
Use cases:

- Large content repositories
- Development environments
- User home directories

**Amazon EC2 Instance Storage**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2026.png)

**EC2** Instance store is a temporary store with a lifecycle of ec2 instance. It means that as soon as **EC2** instance is terminated this store is gone as well.

Use cases:

- Temporary content
- Buffers
- Caches
- Scratch data
- Ideal for data distribution/replication across multiple **EC2** instances (Hadoop cluster)

**AWS EBS (Elastic Block Storage)**

Key points:

- It is a block storage that resides separately from an **EC2** Instance.
- Most of the time you have one-to-one relationship between **EC2** and **EBS**, but **AWS** has recently added multi-attach option with some nuances like **AZ** location etc.
- **EBS** is scalable and you can attach multiple volumes to a single **EC2** instance.
- There are multiple types of **EBS,** have a look at the diagram below.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2027.png)

- You can take snapshots of **EBS** data and these snapshots are stored on **S3.** Snapshots contain only changed/migrated data to save additional space.
- You can create a new **EBS** volume from a snapshot

**AWS S3 (Simple Storage Service)**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2028.png)

Key points:

- **S3** is an aforementioned object storage.
- In fact, there is no actual folders in **S3,** the separation of files is achieved by using prefixes keeping the overall structure flat.
- The name of the bucket must be unique across all region and **DNS**-compliant
- **S3** bucket exists redundantly in multiple **AZs.**
- Everything in **S3** is private by default.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2029.png)

Use cases:

- *Backup and storage*: **S3** is a natural place to back up files because it is highly redundant.
- *Media hosting*: each individual object can be up to 5 TBs, **S3** is an ideal location to host video, photo, or music uploads
- *Software delivery*: You can use **S3** to host your software applications that customers can download. (Frontend of websites)
- *Data lakes*: You can increase storage from gigabytes to petabytes of content
- *Static websites*: You can configure your bucket to host a static website of **HTML**, **CSS**, and client-side scripts.
- *Static content*: Because of the limitless scaling, the support for large files, and the fact that you access any object over the web at any time, **S3** is the perfect place to store static content

Example of bucket policy allowing anonymous viewers to read the objects in employeebucket

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2030.png)

The `Sid`(statement ID) is an optional identifier that you provide for the policy statement. You can assign a `Sid` value to each statement in a statement array. In services that let you specify an `ID`
element, such as SQS and SNS, the `Sid` value is just a sub-ID of the policy document's ID. In IAM, the `Sid` value must be unique within a JSON policy

**S3 Object Versioning**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2031.png)

**S3 Storage Classes**

- **Amazon S3 Standard:** This is considered general purpose storage for cloud applications, dynamic websites, content distribution, mobile and gaming applications, and big data analytics.
- **Amazon S3 Intelligent-Tiering:** This tier is useful if your data has unknown or changing access patterns. S3 Intelligent-Tiering stores objects in two tiers, a frequent access tier and an infrequent access tier. Amazon S3 monitors access patterns of your data, and automatically moves your data to the most cost-effective storage tier based on frequency of access
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA):** S3 Standard-IA is for data that is accessed less frequently, but requires rapid access when needed. S3 Standard-IA offers the high durability, high throughput, and low latency of S3 Standard, with a low per-GB storage price and per-GB retrieval fee. This storage tier is ideal if you want to store long-term backups, disaster recovery files, and so on
- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA):** Unlike other S3 storage classes which store data in a minimum of three Availability Zones (AZs), S3 One Zone-IA stores data in a single AZ and costs 20% less than S3 Standard-IA. S3 One Zone-IA is ideal for customers who want a lower-cost option for infrequently accessed data but do not require the availability and resilience of S3 Standard or S3 Standard-IA. It’s a good choice for storing secondary backup copies of on-premises data or easily re-creatable data.
- **Amazon S3 Glacier:** S3 Glacier is a secure, durable, and low-cost storage class for data archiving. You can reliably store any amount of data at costs that are competitive with or cheaper than on-premises solutions. To keep costs low yet suitable for varying needs, S3 Glacier provides three retrieval options that range from a few minutes to hours.
- **Amazon S3 Glacier Deep Archive:** S3 Glacier Deep Archive is Amazon S3’s lowest-cost storage class and supports long-term retention and digital preservation for data that may be accessed once or twice in a year. It is designed for customers—particularly those in highly regulated industries, such as the Financial Services, Healthcare, and Public Sectors—that retain data sets for 7 to 10 years or longer to meet regulatory compliance requirements.

****Object Lifecycle Management****

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2032.png)

---

## AWS Databases

There 2 types of databases on **AWS**:

- Managed DB:

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2033.png)

- Unmanaged DB (i.e. hosted on **EC2**):

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2034.png)

**Amazon Relation Database (RDS)**
Use cases:

- Migrating from on-premises environments
- Traditional applications
- **ERP** (Enterprise Resource Planning)
- **CRM** (Customer Relationship Management)

Key points:

- Includes **Amazon Aurora** specifically built for large amounts of data. One instance can hold up to 128 GBs.
- Best practice is to use auto-replication feature to launch another instance in another **AZ.** Data synchronization happens automatically between the instances.
- Uses **EBS** (Elastic Block Storage) as its storage layer.
- Has performance issues under high load/request rate

**Purpose Built DBs**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2035.png)

**Amazon DynamoDB**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2036.png)

Key points: 

- Scaled automatically
- Replicated in multiple **AZs**
- Milliseconds of response time
- Schema is flexible, you can add or remove attributes
- Data stored on **SSDs**
- **Primary keys** are used to identify each item and secondary indices to provide more flexibility for querying
- **Primary key** can be made up of just a **Partition Key** or *composed* of a **Partition Key** and a **Sort Key. Sort Key** is used to sort items within the same partition. Without secondary indices you can only query using **Primary Key.**
- **Local Secondary Indices** allow us to ease querying, but when querying you still have to specify both **Partition Key** and **Sort Key** of **Secondary Index**
- **Global Secondary Indices** define an entirely new **Primary Key** and give us opportunity to query data w/o knowing original **Primary Key**

Use cases:

- High-traffic web apps
- E-commerce systems
- Gaming applications

**Global AWS DBs use cases cheat sheet**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2037.png)

 

---

## **AWS Monitoring**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2038.png)

Key points:

- Based on certain metrics we can trigger particular events called **Alarms**
- Discover insights to keep our applications healthy

**AWS CloudWatch**

Key points:

- Create Alarms based on certain metrics
- Gather logs
- Set up metrics
- Set up dashboards with multiple metrics

---

## AWS Optimisation

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2039.png)

Key points: 

- Some of the **AWS** services are optimised by default. Such as **DynamoDB**, **S3** etc. ****
- To increase **availability** we need **redundancy**
- **Horizontal** scaling - quantity
- **Vertical** scaling - power/resources
- With **Horizontal** scaling you no longer need to use public **IP** addresses for each instance, just use load balancer. (**AWS ELB**)
- Best practice is to have your instances in at least 2 **AZs.**

**Types of High Availability**

- *Active-Passive:* With an active-passive system, only one of the two instances is available at a time. One advantage of this method is that for stateful applications where data about the client’s session is stored on the server, there won’t be any issues as the customers are always sent to the same server where their session is stored. ****
- *Active-Active:* A disadvantage of active-passive and where an active-active system shines is scalability. By having both servers available, the second server can take some load for the application, thus allowing the entire system to take more load. However, if the application is stateful, there would be an issue if the customer’s session isn’t available on both servers. Stateless applications work better for active-active systems.

**AWS Load Balancing**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2040.png)

There are 3 types of load balancers:

- **Application Load Balancer** (**ALB**): this is the distribution of requests based on multiple variables, from the network layer to the application layer. It is *context-aware* 
and can direct requests based on any single variable as easily as it can a combination of variables. Applications are load balanced based on their peculiar behavior and not solely on server (operating system or virtualization layer) information.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2041.png)

**ALB** consists of 3 main components: 

1. *Listener* - checks for requests
2. *Target group* - type of backend we want to direct traffic to. Every target group needs to have a health check condition.
3. *Rules* - defines how requests are routed to targets

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2042.png)

- **Network Load Balancer** (**NLB**): this is the distribution of traffic based on network variables, such as IP address and destination ports. It is layer 4 (**TCP**) and below and is not designed to take into consideration anything at the application layer such as content type, cookie data, custom headers, user location, or the application behavior. It is *context-less*, caring only about the network-layer information contained within the packets it is directing this way and that.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2043.png)

- **Gateway Load Balancer** (**GLB**): used mostly to route traffic to 3rd-party apps. Helps you easily deploy, scale, and manage your third-party virtual appliances**.** It gives you one gateway for distributing traffic across multiple virtual appliances while scaling them up or down, based on demand.

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2044.png)

**Example of load balancers combined:**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2045.png)

**Complete load balancers comparison:**

![ALB-NLB-GLB-1-1200x2624.jpg](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/ALB-NLB-GLB-1-1200x2624.jpg)

**Example of migration from all-in architecture to separation of concerns architecture**

- *Before*:

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2046.png)

- *After*:

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2047.png)

---

## EC2 Auto-Scaling

Visual example of auto scaling

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2048.png)

According to the diagram we will **horizontally** scale the fleet of our **ec2** instances when the alarm threshold of CPU usage is reached.   ****

**Auto-Scaling group (ASG) specification:**

![Untitled](AWS%20Cloud%20Technical%20Essentials%201e59be40463049b08dae970cbad17615/Untitled%2049.png)

Key points:

- You have to create *launch template* for an **ec2** instance in an auto-scaling group
- Then create *target group* to use in **ELB**
- Create **ELB** itself
- Configure scaling trigger, i.e. CPU utilization threshold
