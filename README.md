# AWS-SAA-PRACTICE-EXAM
AWS-SAA-PRACTICE-EXAM

<details>
  <summary>==Questions 1-10==</summary>
  
<details>
  <summary>Question 1</summary>

A company collects data for temperature, humidity, and atmospheric pressure in cities across multiple continents.  The average volume of data that the company collects from each site daily is 500 GB.  Each site has a high-speed Internet connection.    

The company wants to aggregate the data from all these global sites as quickly as possible in a single Amazon S3 bucket.  The solution must minimize operational complexity.    

Which solution meets these requirements?

- [ ]  A. Turn on S3 Transfer Acceleration on the destination S3 bucket.  Use multipart uploads to directly upload site data to the destination S3 bucket. 
- [ ]  B. Upload the data from each site to an S3 bucket in the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.  Then remove the data from the origin S3 bucket. 
- [ ]  C. Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket. 
- [ ]  D. Upload the data from each site to an Amazon EC2 instance in the closest Region.  Store the data in an Amazon Elastic Block Store (Amazon EBS) volume.  At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket.  Restore the EBS volume in that Region. 

</details>

<details>
  <summary>Answer</summary>

- [ ]  A. Turn on S3 Transfer Acceleration on the destination S3 bucket.  Use multipart uploads to directly upload site data to the destination S3 bucket. 

Why this is the correct answer:

- [ ]  S3 Transfer Acceleration: This feature leverages Amazon CloudFront's globally distributed edge locations. When data arrives at an edge location, it is routed to Amazon S3 over an optimized network path. This is ideal for transferring large amounts of data over long distances, as is the case with global sites, and helps in aggregating data "as quickly as possible".    
- [ ]  Multipart Uploads: For large files like 500 GB daily from each site, multipart upload allows you to upload a single object as a set of parts.  This can lead to faster uploads by uploading parts in parallel and increased resilience, as a failed part can be retransmitted without affecting other parts.    
- [ ]  Minimized Operational Complexity: This solution involves configuring Transfer Acceleration on the destination bucket and using the AWS SDK or CLI (which support multipart uploads) to upload directly.  This is a straightforward approach with minimal moving parts compared to the other options, meeting the requirement to "minimize operational complexity".    

Why are the other answers wrong?

- [ ]  Option B is wrong because: While using S3 Cross-Region Replication works, uploading to a local bucket first and then replicating introduces an additional step and latency compared to a direct, accelerated transfer.  Managing data in multiple S3 buckets (origin and destination) and then ensuring deletion from the origin also adds to operational overhead.    
- [ ]  Option C is wrong because: AWS Snowball Edge is typically suited for situations where network connectivity is limited or for petabyte-scale data transfers where online transfer would be too slow.  Given that each site has a high-speed internet connection and daily transfers of 500GB, the logistics of ordering, receiving, managing, and shipping Snowball devices daily from multiple global sites would be highly operationally complex and slow, contradicting the requirements.    
- [ ]  Option D is wrong because: This solution introduces significant operational overhead by involving EC2 instances, EBS volumes, EBS snapshots, snapshot copying, and volume restoration.  This is a multi-step, complex process for a task that can be achieved much more simply and directly with S3 features, and it does not align with the requirement for the least operational overhead. 

</details>

<details>
  <summary>Question 2</summary>

A company needs the ability to analyze the log files of its proprietary application. The logs are stored in JSON format in an Amazon S3 bucket. Queries will be simple and will run on-demand. A solutions architect needs to perform the analysis with minimal changes to the existing architecture.

What should the solutions architect do to meet these requirements with the LEAST amount of operational overhead?

- [ ] A. Use Amazon Redshift to load all the content into one place and run the SQL queries as needed.
- [ ] B. Use Amazon CloudWatch Logs to store the logs. Run SQL queries as needed from the Amazon CloudWatch console.
- [ ] C. Use Amazon Athena directly with Amazon S3 to run the queries as needed.
- [ ] D. Use AWS Glue to catalog the logs. Use a transient Apache Spark cluster on Amazon EMR to run the SQL queries as needed.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use Amazon Athena directly with Amazon S3 to run the queries as needed.

Why this is the correct answer:

- [ ] Amazon Athena is a serverless query service that allows you to analyze data directly in Amazon S3 using standard SQL.
- [ ] Since the logs are already stored in JSON format in an S3 bucket, Athena can query this data in place.
- [ ] This perfectly aligns with the requirement for "minimal changes to the existing architecture" and "LEAST amount of operational overhead"  because there's no need to move data or manage any servers or clusters.
- [ ] Athena is well-suited for "simple" and "on-demand" queries.   

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Redshift is a powerful data warehousing service, but it requires loading data from S3 into Redshift clusters. This process involves setting up a Redshift cluster, defining schemas, and managing the data loading process (ETL), which introduces significant operational overhead and changes to the architecture, contradicting the requirements.   
- [ ] Option B is wrong because: While Amazon CloudWatch Logs can store and query logs, the scenario states the logs are already in an Amazon S3 bucket. Moving them to CloudWatch Logs would be an unnecessary step, adding complexity and operational overhead. CloudWatch Logs Insights is useful for operational logs generated by AWS services or applications sending logs directly to it, but for analyzing existing JSON files in S3, Athena is more direct and efficient.   
- [ ] Option D is wrong because: Using AWS Glue to catalog the logs and then running SQL queries with a transient Apache Spark cluster on Amazon EMR is a viable but more complex solution. It introduces more operational overhead (setting up Glue crawlers, configuring and managing EMR clusters, even if transient) compared to the serverless nature of Athena, especially for "simple and will run on-demand" queries. This option is more powerful than needed for the described scenario and doesn't meet the "LEAST amount of operational overhead" requirement as effectively as Athena.

</details>

<details>
  <summary>Question 3</summary>
   
A company uses AWS Organizations to manage multiple AWS accounts for different departments. The management account has an Amazon S3 bucket that contains project reports. The company wants to limit access to this S3 bucket to only users of accounts within the organization in AWS Organizations.

Which solution meets these requirements with the LEAST amount of operational overhead?

- [ ] A. Add the aws:PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy.
- [ ] B. Create an organizational unit (OU) for each department. Add the aws:PrincipalOrgPaths global condition key to the S3 bucket policy.
- [ ] C. Use AWS CloudTrail to monitor the CreateAccount, InviteAccountToOrganization, LeaveOrganization, and RemoveAccountFromOrganization events. Update the S3 bucket policy accordingly.
- [ ] D. Tag each user that needs access to the S3 bucket. Add the aws:PrincipalTag global condition key to the S3 bucket policy.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Add the aws:PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy.

Why this is the correct answer:

- [ ] The aws:PrincipalOrgID global condition key is specifically designed for this use case.
- [ ] By adding this condition to the S3 bucket policy and specifying your AWS Organizations ID, you can restrict access to IAM principals (users and roles) that belong to any account within that organization.
- [ ] This is the most direct and simple method to achieve the stated goal, thus ensuring the "LEAST amount of operational overhead" as it requires a one-time policy configuration that dynamically applies to all accounts in the organization.   

Why are the other answers wrong?

- [ ] Option B is wrong because: While aws:PrincipalOrgPaths allows you to grant permissions based on an account's membership in an Organizational Unit (OU) within AWS Organizations, it's more granular than required.  The requirement is to limit access to any user from any account within the organization, not specific OUs.  Using aws:PrincipalOrgID is simpler for this organization-wide access. Managing access via OUs for this purpose would add unnecessary complexity if the goal isn't to differentiate access by department/OU.   
- [ ] Option C is wrong because: Using AWS CloudTrail to monitor organization membership changes and then dynamically updating the S3 bucket policy is a highly complex and operationally burdensome solution.  It would require developing and maintaining automation (e.g., Lambda functions triggered by CloudTrail events) to parse logs and modify policies. This is far from the "LEAST amount of operational overhead."   
- [ ] Option D is wrong because: Using aws:PrincipalTag to control access based on tags applied to IAM users or roles would require a comprehensive tagging strategy and consistent application of these tags to all relevant principals across all accounts in the organization.  While a valid approach for attribute-based access control, it involves more setup and ongoing management (ensuring all users/roles are correctly tagged) compared to the simpler organization-wide condition offered by aws:PrincipalOrgID. 

</details>

<details>
  <summary>Question 4</summary>

An application runs on an Amazon EC2 instance in a VPC. The application processes logs that are stored in an Amazon S3 bucket. The EC2 instance needs to access the S3 bucket without connectivity to the internet.

Which solution will provide private network connectivity to Amazon S3?

- [ ] A. Create a gateway VPC endpoint to the S3 bucket.
- [ ] B. Stream the logs to Amazon CloudWatch Logs. Export the logs to the S3 bucket.
- [ ] C. Create an instance profile on Amazon EC2 to allow S3 access.
- [ ] D. Create an Amazon API Gateway API with a private link to access the S3 endpoint.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create a gateway VPC endpoint to the S3 bucket.

Why this is the correct answer:

- [ ] Gateway VPC Endpoint for S3: This is the specific AWS service designed to allow resources within your VPC (like EC2 instances) to access Amazon S3 (and DynamoDB) without needing an Internet Gateway, NAT device, VPN connection, or AWS Direct Connect connection.
- [ ] Traffic between your VPC and S3 does not leave the Amazon network. This directly meets the requirement for the EC2 instance to access S3 "without connectivity to the internet."   

Why are the other answers wrong?

- [ ] Option B is wrong because: This describes a data flow pattern into S3 via CloudWatch Logs, not how an EC2 instance accesses data from S3 for processing. It doesn't address the private network connectivity requirement for the EC2 instance that is processing logs already in S3. Moreover, sending logs to CloudWatch Logs might itself require internet access if a VPC endpoint for CloudWatch Logs isn't also configured.
- [ ] Option C is wrong because: An instance profile (which contains an IAM role) grants the EC2 instance permissions (authorization) to access S3. While necessary for the application to interact with S3, it does not provide the network path. The question is specifically about "private network connectivity." An instance profile is about "what" the EC2 instance can do, not "how" it connects from a network perspective.
- [ ] Option D is wrong because: Using Amazon API Gateway to front S3 and then accessing that API via AWS PrivateLink is an overly complex solution for this specific problem. While technically possible to achieve private connectivity this way, a gateway VPC endpoint for S3 is a much simpler, more direct, and standard solution designed for EC2-to-S3 private communication within a VPC. API Gateway is generally used for building and exposing APIs to clients, not typically for backend instance-to-service private access when a direct VPC endpoint is available.

</details>

<details>
  <summary>Question 5</summary>
   
A company is hosting a web application on AWS using a single Amazon EC2 instance that stores user-uploaded documents in an Amazon EBS volume. For better scalability and availability, the company duplicated the architecture and created a second EC2 instance and EBS volume in another Availability Zone, placing both behind an Application Load Balancer. After completing this change, users reported that, each time they refreshed the website, they could see one subset of their documents or the other, but never all of the documents at the same time.

What should a solutions architect propose to ensure users see all of their documents at once?

- [ ] A. Copy the data so both EBS volumes contain all the documents
- [ ] B. Configure the Application Load Balancer to direct a user to the server with the documents
- [ ] C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS
- [ ] D. Configure the Application Load Balancer to send the request to both servers. Return each document from the correct server

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS

Why this is the correct answer:

- [ ] The core issue is that each EC2 instance has its own separate EBS volume, meaning user documents are isolated to the instance they were uploaded to or are stored on. When the Application Load Balancer sends a user to a different instance on refresh, they see a different set of documents.
- [ ] Amazon EFS (Elastic File System) provides scalable, shared file storage that can be accessed concurrently by multiple EC2 instances, even across different Availability Zones within the same region.
- [ ] By migrating the documents to Amazon EFS and modifying the application to read from and write to this shared EFS file system, both EC2 instances will access the same central repository of documents.
- [ ] This ensures that users see all their documents consistently, regardless of which EC2 instance serves their request. This solution directly addresses the problem of data inconsistency across instances.

Why are the other answers wrong?

- [ ] Option A is wrong because: Manually or programmatically copying data between two EBS volumes to keep them synchronized is operationally complex, error-prone, and can lead to data inconsistencies, especially with active uploads. It's not a scalable or reliable solution for ensuring all users see all documents in real-time.
- [ ] Option B is wrong because: While an Application Load Balancer can use features like sticky sessions to send a user to the same instance, this doesn't solve the underlying problem. If a user's documents are meant to be accessible globally (e.g., new documents uploaded could theoretically land on either instance, or if an instance fails), sticky sessions won't ensure all documents are visible. The fundamental issue is the lack of a shared data store.
- [ ] Option D is wrong because: Configuring an Application Load Balancer to send requests to both servers and then have the application logic aggregate responses is not a standard or efficient way to handle shared file access. This would add significant complexity to the application and the infrastructure, and it doesn't solve the problem of where new documents are stored or how they are consistently accessed.

</details>

<details>
  <summary>Question 6</summary>
  
A company uses NFS to store large video files in on-premises network attached storage. Each video file ranges in size from 1 MB to 500 GB. The total storage is 70 TB and is no longer growing. The company decides to migrate the video files to Amazon S3. The company must migrate the video files as soon as possible while using the least possible network bandwidth.

Which solution will meet these requirements?

- [ ] A. Create an S3 bucket. Create an IAM role that has permissions to write to the S3 bucket. Use the AWS CLI to copy all files locally to the S3 bucket.
- [ ] B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3.
- [ ] C. Deploy an S3 File Gateway on premises. Create a public service endpoint to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.
- [ ] D. Set up an AWS Direct Connect connection between the on-premises network and AWS. Deploy an S3 File Gateway on premises. Create a public virtual interface (VIF) to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3.

Why this is the correct answer:

- [ ] AWS Snowball Edge is specifically designed for transferring large amounts of data (like 70 TB) into AWS when network bandwidth is a constraint or when the transfer needs to be done quickly without saturating the internet connection.
- [ ] "Least possible network bandwidth": Snowball Edge allows data to be copied to a physical device on-premises and then shipped to AWS. The actual data transfer to AWS occurs over AWS's internal network after they receive the device, not over your company's internet connection. This minimizes the use of your network bandwidth for the large data volume.
- [ ] "As soon as possible": For 70 TB of data, physical transfer via Snowball Edge (including shipping time) is often faster than transferring over a typical internet connection. Data can be loaded onto the device at local network speeds.

Why are the other answers wrong?

- [ ] Option A is wrong because: Transferring 70 TB of data using the AWS CLI over a standard internet connection would consume a massive amount of network bandwidth and would likely take a very long time, depending on the available upload speed. This contradicts both "least possible network bandwidth" and "as soon as possible."
- [ ] Option C is wrong because: While an S3 File Gateway provides an NFS interface and can help migrate data to S3, it would still transfer the 70 TB of data over your existing internet connection (unless combined with Direct Connect). This doesn't address the "least possible network bandwidth" requirement as effectively as an offline transfer method like Snowball for the initial bulk migration.
- [ ] Option D is wrong because: Setting up an AWS Direct Connect connection can be a lengthy and expensive process, especially if it's not already in place. While it provides dedicated bandwidth, Snowball Edge is generally a faster and more cost-effective solution for a one-time large-scale data migration when minimizing the use of existing network bandwidth is key. The requirement is "least possible network bandwidth," implying avoiding saturating the existing connection, which Snowball achieves by taking the primary data transfer offline.

</details>

<details>
  <summary>Question 7</summary>
  
A company has an application that ingests incoming messages. Dozens of other applications and microservices then quickly consume these messages. The number of messages varies drastically and sometimes increases suddenly to 100,000 each second. The company wants to decouple the solution and increase scalability.

Which solution meets these requirements?

- [ ] A. Persist the messages to Amazon Kinesis Data Analytics. Configure the consumer applications to read and process the messages.
- [ ] B. Deploy the ingestion application on Amazon EC2 instances in an Auto Scaling group to scale the number of EC2 instances based on CPU metrics.
- [ ] C. Write the messages to Amazon Kinesis Data Streams with a single shard. Use an AWS Lambda function to preprocess messages and store them in Amazon DynamoDB. Configure the consumer applications to read from DynamoDB to process the messages.
- [ ] D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SQS) subscriptions. Configure the consumer applications to process the messages from the queues.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SQS) subscriptions. Configure the consumer applications to process the messages from the queues.

Why this is the correct answer:

- [ ] Decoupling: Amazon SNS (Simple Notification Service) allows you to publish messages to a topic. Amazon SQS (Simple Queue Service) queues can then subscribe to this SNS topic. This creates a fan-out architecture where the message-producing application is decoupled from the numerous consuming applications. Each consuming application processes messages from its own SQS queue.
- [ ] Scalability for many consumers: An SNS topic can deliver messages to many SQS queues (and other subscribers like Lambda, HTTP endpoints, etc.). This allows "dozens of other applications and microservices" to consume the messages independently and scale their processing as needed. SQS queues can buffer a large number of messages, accommodating the "100,000 each second" requirement (though this might require using SQS FIFO queues with high throughput mode or multiple standard queues depending on ordering and exact-once processing needs, the pattern itself is sound).
- [ ] Handling drastic variations: SQS queues act as durable buffers. If there's a sudden spike in messages, the queues will hold them until the consumer applications can process them. This ensures messages are not lost and consumers can work at their own pace, addressing the "varies drastically" aspect.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Kinesis Data Analytics is designed for real-time analysis and processing of streaming data (e.g., running SQL queries or Apache Flink applications on data streams). It's not primarily a message bus for distributing messages to dozens of diverse consumer applications for general processing. Consumers typically read from a data stream (like Kinesis Data Streams) or a persistent store, not directly from Kinesis Data Analytics in the way described.
- [ ] Option B is wrong because: While scaling the ingestion application with an Auto Scaling group is a good practice for handling variable load on the producer side, it does not address the requirement of decoupling and providing a scalable consumption mechanism for the "dozens of other applications and microservices." This option only solves part of the problem.
- [ ] Option C is wrong because: A single shard in Amazon Kinesis Data Streams has ingestion limits (typically 1 MB/second or 1,000 records/second). 100,000 messages per second would far exceed the capacity of a single shard, making this option unfeasible for the described load. While Kinesis Data Streams is highly scalable by adding more shards, the specification of a "single shard" makes this choice incorrect. Furthermore, while storing preprocessed messages in DynamoDB for consumption is possible, SNS/SQS provides a more robust and flexible pub/sub and queuing mechanism for a large number of independent consumers.

</details>

<details>
  <summary>Question 8</summary>
  
A company is migrating a distributed application to AWS. The application serves variable workloads. The legacy platform consists of a primary server that coordinates jobs across multiple compute nodes. The company wants to modernize the application with a solution that maximizes resiliency and scalability.

How should a solutions architect design the architecture to meet these requirements?

- [ ]  A. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling to use scheduled scaling.
- [ ]  B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.
- [ ]  C. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure AWS CloudTrail as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the primary server.
- [ ]  D. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure Amazon EventBridge (Amazon CloudWatch Events) as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the compute nodes.

</details>

<details>
  <summary>Answer</summary>

- [ ]  B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.

Why this is the correct answer:

- [ ]  Decoupling with SQS: Using Amazon SQS as a destination for jobs effectively decouples the job producers from the consumers (compute nodes). This is a core principle for building resilient and scalable distributed applications. If compute nodes fail, the jobs remain safely in the SQS queue. This replaces the legacy "primary server that coordinates jobs" with a more robust, managed service.
- [ ]  Scalable Compute with Auto Scaling: Implementing compute nodes with Amazon EC2 instances in an Auto Scaling group allows the application to automatically adjust the number of compute instances based on the workload.
- [ ]  Dynamic Scaling based on Queue Size: Configuring EC2 Auto Scaling to scale based on the size of the SQS queue (e.g., the ApproximateNumberOfMessagesVisible metric) is a highly effective method for handling "variable workloads." As the number of jobs in the queue increases, the Auto Scaling group launches more instances to process them. As the queue shortens, instances are terminated to save costs. This directly ties compute capacity to the actual pending workload, maximizing both resiliency and scalability.

Why are the other answers wrong?

- [ ]  Option A is wrong because: Scheduled scaling is suitable for predictable workloads with known patterns. The problem states "variable workloads," which implies unpredictability. Scaling based on queue size (as in option B) is much more responsive to dynamic, variable demand than fixed scheduled scaling.
- [ ]  Option C is wrong because: AWS CloudTrail is a service for logging API calls and account activity for auditing and governance; it is not a "destination for jobs" or a message queuing system. Furthermore, scaling compute nodes based on the load of a single "primary server" can perpetuate a single point of bottleneck or failure, which modernizing seeks to avoid.
- [ ]  Option D is wrong because: Amazon EventBridge (formerly CloudWatch Events) is an event bus service used to build event-driven architectures by routing events between different services. While it can trigger compute resources, SQS is more specifically designed as a durable queue for decoupling tasks and managing a backlog of jobs for worker processes. Scaling based on the load on compute nodes is a valid approach, but scaling based on the SQS queue depth provides a more direct measure of the unprocessed workload.

</details>

<details>
  <summary>Question 9</summary>

A company is running an SMB file server in its data center. The file server stores large files that are accessed frequently for the first few days after the files are created. After 7 days the files are rarely accessed.

The total data size is increasing and is close to the company's total storage capacity. A solutions architect must increase the company's available storage space without losing low-latency access to the most recently accessed files. The solutions architect must also provide file lifecycle management to avoid future storage issues.

Which solution will meet these requirements?

- [ ]  A. Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS.
- [ ]  B. Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.
- [ ]  C. Create an Amazon FSx for Windows File Server file system to extend the company's storage space.
- [ ]  D. Install a utility on each user's computer to access Amazon S3. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days.

</details>

<details>
  <summary>Answer</summary>

- [ ]  B. Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.

Why this is the correct answer:

- [ ]  Extending storage and low-latency access: Amazon S3 File Gateway (part of AWS Storage Gateway) provides a file interface (SMB or NFS) into Amazon S3. It stores data in S3 while caching frequently accessed data locally on the gateway appliance. This allows users to continue accessing files via SMB with low latency for the cached, recently accessed files (frequently accessed for the first few days). The bulk of the storage resides in S3, effectively increasing the company's available storage space.
- [ ]  File lifecycle management: Once the files are in Amazon S3 (via the File Gateway), you can apply S3 Lifecycle policies. These policies can automatically transition objects to different storage classes based on their age. Transitioning files older than 7 days (which are rarely accessed) to a very cost-effective archival tier like S3 Glacier Deep Archive directly addresses the requirement for lifecycle management and helps avoid future storage capacity issues by moving cold data to cheaper storage.
- [ ]  Minimal disruption: Users can continue to access files using the SMB protocol through the File Gateway, minimizing changes to their existing workflows.

Why are the other answers wrong?

- [ ]  Option A is wrong because: AWS DataSync is a service for online data transfer, useful for migrating data or synchronizing it between on-premises systems and AWS. While it could copy older data to AWS, it doesn't provide the seamless storage extension with local caching for low-latency access to recent files that a File Gateway offers. Users would still be interacting with the capacity-constrained on-premises server for all files unless their access patterns were changed.
- [ ]  Option C is wrong because: While Amazon FSx for Windows File Server provides fully managed SMB file storage and could extend the company's storage space, this option by itself doesn't explicitly address the lifecycle management to a very cold, cost-effective archive tier like S3 Glacier Deep Archive for files older than 7 days. S3 File Gateway combined with S3 Lifecycle policies to S3 Glacier Deep Archive is typically more cost-effective for very cold, rarely accessed data compared to keeping it all within FSx tiers long-term.
- [ ]  Option D is wrong because: Requiring users to install a utility on their computers to access Amazon S3 directly would be a significant change from their current SMB file server access method and could be disruptive. It also bypasses the benefits of a local cache for frequently accessed files that a File Gateway provides, potentially increasing latency for those files. While S3 Lifecycle policies could be used, the access method is not ideal for maintaining low-latency access for recent files in an existing SMB setup.

</details>

<details>
  <summary>Question 10</summary>
   
A company is building an ecommerce web application on AWS. The application sends information about new orders to an Amazon API Gateway REST API to process. The company wants to ensure that orders are processed in the order that they are received.

Which solution will meet these requirements?

- [ ]  A. Use an API Gateway integration to publish a message to an Amazon Simple Notification Service (Amazon SNS) topic when the application receives an order. Subscribe an AWS Lambda function to the topic to perform processing.
- [ ]  B. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order. Configure the SQS FIFO queue to invoke an AWS Lambda function for processing.
- [ ]  C. Use an API Gateway authorizer to block any requests while the application processes an order.
- [ ]  D. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) standard queue when the application receives an order. Configure the SQS standard queue to invoke an AWS Lambda function for processing.

</details>

<details>
  <summary>Answer</summary>

- [ ]  B. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order. Configure the SQS FIFO queue to invoke an AWS Lambda function for processing.

Why this is the correct answer:

- [ ] Guaranteed Order Processing with SQS FIFO: The core requirement is to process orders "in the order that they are received." Amazon SQS FIFO (First-In, First-Out) queues are specifically designed for use cases where the order of operations and events is critical. They preserve the exact order in which messages are sent and received.
- [ ] Decoupling and Asynchronous Processing: When API Gateway receives an order, it can send it as a message to an SQS FIFO queue. This decouples the initial API request from the backend processing. An AWS Lambda function can then be configured to poll this SQS FIFO queue and process the orders one by one, in the correct sequence. This architecture is also scalable and resilient.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon SNS is a publish/subscribe service. While it's excellent for fanning out messages to multiple subscribers, it does not guarantee the order in which subscribers (like a Lambda function) receive or process messages. For strict order preservation, SNS alone is not suitable.
- [ ] Option C is wrong because: Using an API Gateway authorizer to block incoming requests while an order is being processed would effectively make the system process only one order at a time globally. This would severely impact the application's performance, scalability, and availability, especially for an ecommerce application that might receive many concurrent orders. Authorizers are intended for controlling access (authentication and authorization), not for managing processing flow in this manner.
- [ ] Option D is wrong because: Amazon SQS standard queues offer "best-effort" ordering, meaning that while messages are generally delivered in the order they are sent, there's no guarantee. For critical tasks like processing ecommerce orders in sequence, where mistakes in ordering can have significant consequences, standard SQS queues are not appropriate. SQS FIFO queues are the correct choice for guaranteed ordering.

</details>


</details>

<details>
  <summary>==Questions 11-20==</summary>

<details>
  <summary>Question 11</summary>
 
A company has an application that runs on Amazon EC2 instances and uses an Amazon Aurora database. The EC2 instances connect to the database by using user names and passwords that are stored locally in a file. The company wants to minimize the operational overhead of credential management.

What should a solutions architect do to accomplish this goal?

- [ ] A. Use AWS Secrets Manager. Turn on automatic rotation.
- [ ] B. Use AWS Systems Manager Parameter Store. Turn on automatic rotation.
- [ ] C. Create an Amazon S3 bucket to store objects that are encrypted with an AWS Key Management Service (AWS KMS) encryption key. Migrate the credential file to the S3 bucket. Point the application to the S3 bucket.
- [ ] D. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume for each EC2 instance. Attach the new EBS volume to each EC2 instance. Migrate the credential file to the new EBS volume. Point the application to the new EBS volume.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Use AWS Secrets Manager. Turn on automatic rotation.

Why this is the correct answer:

- [ ] AWS Secrets Manager is a service specifically designed for managing the lifecycle of secrets, including database credentials, API keys, and other sensitive data.
- [ ] Automatic Rotation: A key feature of Secrets Manager is its ability to automatically rotate secrets for supported AWS services like Amazon Aurora. This eliminates the manual effort and potential errors associated with rotating credentials, directly addressing the requirement to "minimize the operational overhead of credential management."
- [ ] Secure Storage and Retrieval: Secrets are stored securely, encrypted at rest, and applications can retrieve them programmatically using IAM permissions, removing the need to store credentials in local files on EC2 instances.

Why are the other answers wrong?

- [ ] Option B is wrong because: While AWS Systems Manager Parameter Store can store secrets (as SecureString parameters) and has some versioning capabilities, its native automatic rotation features are not as extensive or straightforward for database credentials like Amazon Aurora when compared to AWS Secrets Manager. Secrets Manager is the more specialized and feature-rich service for managing and automatically rotating database credentials. Parameter Store often requires custom Lambda functions for complex rotation scenarios, which adds operational overhead.
- [ ] Option C is wrong because: Storing credential files in Amazon S3, even with encryption, is not a best practice for managing active database credentials. This approach lacks built-in automatic rotation capabilities, which is crucial for minimizing operational overhead. The application would still need to fetch and parse the file, and managing access and rotation would remain largely a manual or custom-developed process.
- [ ] Option D is wrong because: Storing the credential file on an encrypted EBS volume still involves managing a file on the EC2 instance. This does not centralize credential management, nor does it offer any mechanism for automatic rotation. It does little to reduce the operational overhead associated with the credential lifecycle (e.g., changing passwords regularly) and still leaves credentials vulnerable if the instance itself is compromised.

</details>

<details>
  <summary>Question 12</summary>
    
A global company hosts its web application on Amazon EC2 instances behind an Application Load Balancer (ALB). The web application has static data and dynamic data. The company stores its static data in an Amazon S3 bucket. The company wants to improve performance and reduce latency for the static data and dynamic data. The company is using its own domain name registered with Amazon Route 53.

What should a solutions architect do to meet these requirements?

- [ ] A. Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.
- [ ] B. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint Configure Route 53 to route traffic to the CloudFront distribution.
- [ ] C. Create an Amazon CloudFront distribution that has the S3 bucket as an origin. Create an AWS Global Accelerator standard accelerator that has the ALB and the CloudFront distribution as endpoints. Create a custom domain name that points to the accelerator DNS name. Use the custom domain name as an endpoint for the web application.
- [ ] D. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint. Create two domain names. Point one domain name to the CloudFront DNS name for dynamic content. Point the other domain name to the accelerator DNS name for static content. Use the domain names as endpoints for the web application.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.

Why this is the correct answer:

- [ ] Comprehensive Content Delivery with CloudFront: Amazon CloudFront is a content delivery network (CDN) service that can significantly improve performance and reduce latency for both static and dynamic content.
- [ ] Static Content: CloudFront can cache static data from the Amazon S3 bucket at edge locations around the world, closer to the users. This drastically reduces latency for accessing these files.
- [ ] Dynamic Content: CloudFront can also serve dynamic content from the Application Load Balancer (ALB). Requests for dynamic content are routed through CloudFront's optimized network to the ALB, benefiting from features like persistent connections and SSL/TLS termination at the edge.
- [ ] Multiple Origins and Behaviors: A single CloudFront distribution can be configured with multiple origins (the S3 bucket for static content and the ALB for dynamic content). Cache behaviors can then be defined to route specific URL path patterns to the appropriate origin (e.g., /images/* to S3, /api/* to ALB).
- [ ] Simplified DNS: By configuring Amazon Route 53 to point the company's domain name to the CloudFront distribution, all user traffic is directed through CloudFront, ensuring that both static and dynamic content delivery is accelerated. This provides a unified and efficient solution.

Why are the other answers wrong?

- [ ] Option B is wrong because: While AWS Global Accelerator improves performance for applications by routing traffic over the AWS global network to optimal endpoints, CloudFront is generally the more suitable and cost-effective solution for caching and delivering static web content from S3. This option proposes using CloudFront only for the ALB (dynamic content) and Global Accelerator for the S3 bucket (static content). A single CloudFront distribution can handle both more effectively.
- [ ] Option C is wrong because: This solution is overly complex. It suggests using CloudFront for static S3 content and then placing an AWS Global Accelerator in front of both the ALB and this CloudFront distribution. CloudFront itself is designed to be the primary edge delivery service for web applications, capable of handling both static and dynamic content by routing to different origins. Adding Global Accelerator on top of CloudFront for static content adds an unnecessary layer.
- [ ] Option D is wrong because: This option introduces complexity by suggesting separate delivery mechanisms (CloudFront for dynamic, Global Accelerator for static) and potentially requiring two different domain names or complex application logic to route requests. A single CloudFront distribution with multiple origins and behaviors is a more streamlined and standard approach for handling both types of content under one domain. CloudFront's caching capabilities are more beneficial for static S3 content than using Global Accelerator alone for it.

</details>

<details>
  <summary>Question 13</summary>

A company performs monthly maintenance on its AWS infrastructure. During these maintenance activities, the company needs to rotate the credentials for its Amazon RDS for MySQL databases across multiple AWS Regions.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Store the credentials as secrets in AWS Secrets Manager. Use multi-Region secret replication for the required Regions. Configure Secrets Manager to rotate the secrets on a schedule.
- [ ] B. Store the credentials as secrets in AWS Systems Manager by creating a secure string parameter. Use multi-Region secret replication for the required Regions. Configure Systems Manager to rotate the secrets on a schedule.
- [ ] C. Store the credentials in an Amazon S3 bucket that has server-side encryption (SSE) enabled. Use Amazon EventBridge (Amazon CloudWatch Events) to invoke an AWS Lambda function to rotate the credentials.
- [ ] D. Encrypt the credentials as secrets by using AWS Key Management Service (AWS KMS) multi-Region customer managed keys. Store the secrets in an Amazon DynamoDB global table. Use an AWS Lambda function to retrieve the secrets from DynamoDB. Use the RDS API to rotate the secrets.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Store the credentials as secrets in AWS Secrets Manager. Use multi-Region secret replication for the required Regions. Configure Secrets Manager to rotate the secrets on a schedule.

Why this is the correct answer:

- [ ] AWS Secrets Manager for RDS: AWS Secrets Manager is specifically designed for managing secrets, including database credentials. It has built-in integration for automatically rotating credentials for Amazon RDS databases (including MySQL) on a defined schedule (e.g., monthly).    
- [ ] Multi-Region Secret Replication: Secrets Manager supports multi-Region replication.  This means you can create a primary secret in one region and replicate it to other specified AWS Regions. When the primary secret is rotated, the new version is automatically replicated. This ensures that applications or services in different regions can access the up-to-date credentials for the RDS databases.   
- [ ] Least Operational Overhead: This solution leverages managed AWS services for storing, rotating, and replicating secrets. Configuring automatic rotation and replication in Secrets Manager is straightforward and requires minimal ongoing operational effort compared to building custom solutions.    

Why are the other answers wrong?

- [ ] Option B is wrong because: While AWS Systems Manager Parameter Store can store secrets (as SecureStrings) and also supports multi-Region parameters, its native automatic rotation capabilities for database credentials like RDS are not as direct or as feature-rich as AWS Secrets Manager. Achieving scheduled rotation for RDS often requires more custom setup (e.g., with Lambda functions) if using Parameter Store, thus increasing operational overhead compared to Secrets Manager's built-in RDS rotation.
- [ ] Option C is wrong because: Storing credentials in an Amazon S3 bucket, even with server-side encryption, is not a recommended practice for active database credentials.  This approach would require building a custom solution using Amazon EventBridge and AWS Lambda to handle the rotation logic and distribution of credentials, which would involve significant development and ongoing operational overhead.    
- [ ] Option D is wrong because: This describes a highly custom and complex solution. It involves using AWS KMS for encryption, Amazon DynamoDB global tables for storing the secrets, and an AWS Lambda function for the retrieval and rotation logic.  Building and maintaining such a system would entail substantial operational overhead compared to using the purpose-built functionalities of AWS Secrets Manager.
      
</details>

<details>
  <summary>Question 14</summary>
   
A company runs an ecommerce application on Amazon EC2 instances behind an Application Load Balancer. The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. The Auto Scaling group scales based on CPU utilization metrics. The ecommerce application stores the transaction data in a MySQL 8.0 database that is hosted on a large EC2 instance.

The database's performance degrades quickly as application load increases. The application handles more read requests than write transactions. The company wants a solution that will automatically scale the database to meet the demand of unpredictable read workloads while maintaining high availability.

Which solution will meet these requirements?

- [ ] A. Use Amazon Redshift with a single node for leader and compute functionality.
- [ ] B. Use Amazon RDS with a Single-AZ deployment Configure Amazon RDS to add reader instances in a different Availability Zone.
- [ ] C. Use Amazon Aurora with a Multi-AZ deployment. Configure Aurora Auto Scaling with Aurora Replicas.
- [ ] D. Use Amazon ElastiCache for Memcached with EC2 Spot Instances.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use Amazon Aurora with a Multi-AZ deployment. Configure Aurora Auto Scaling with Aurora Replicas.

Why this is the correct answer:

- [ ] Amazon Aurora for High Availability and Read Scalability: Amazon Aurora is a MySQL and PostgreSQL-compatible relational database service. An Aurora Multi-AZ deployment ensures high availability by replicating data across multiple Availability Zones.
- [ ] Aurora Auto Scaling with Aurora Replicas: Aurora specifically addresses the need for read scalability with Aurora Replicas. You can configure Aurora Auto Scaling to automatically add or remove Aurora Replicas based on metrics like CPU utilization or number of connections. This is ideal for handling "unpredictable read workloads" and applications that are "more read requests than write transactions," as stated in the question. This allows the read capacity to scale dynamically without manual intervention.
- [ ] MySQL Compatibility: Since the existing database is MySQL 8.0, migrating to Amazon Aurora (MySQL compatible edition) would be a relatively straightforward path.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Redshift is a data warehousing service designed for online analytical processing (OLAP) and business intelligence, not for transactional workloads (OLTP) typical of an ecommerce application's primary database. A single-node Redshift cluster also does not offer high availability.
- [ ] Option B is wrong because: An Amazon RDS "Single-AZ deployment" for the primary database instance is not highly available. If the single Availability Zone experiences an issue, the database will become unavailable. The requirement is to maintain high availability. While RDS allows for read replicas, Aurora's Auto Scaling for replicas is generally more dynamic and integrated for handling unpredictable read scaling.
- [ ] Option D is wrong because: Amazon ElastiCache for Memcached is an in-memory caching service. While it can significantly improve read performance by caching frequently accessed data and reducing load on the database, it is not a persistent database itself. It complements a database but doesn't replace it or solve its underlying scaling and availability needs for persistent storage. Furthermore, using EC2 Spot Instances for a critical component like a cache (especially if data loss on interruption is an issue) or for a database itself is not recommended for production ecommerce applications requiring high availability.

</details>

<details>
  <summary>Question 15</summary>
  
A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the production VPC. The company had an inspection server in its on-premises data center. The inspection server performed specific operations such as traffic flow inspection and traffic filtering. The company wants to have the same functionalities in the AWS Cloud.

Which solution will meet these requirements?

- [ ]  A. Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.
- [ ]  B. Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.
- [ ]  C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.
- [ ]  D. Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC.

</details>

<details>
  <summary>Answer</summary>

- [ ]  C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.

Why this is the correct answer:

- [ ]  AWS Network Firewall is a managed, stateful firewall service that provides network protections for your Amazon Virtual Private Cloud (VPC). It allows you to define granular firewall rules for traffic inspection (e.g., deep packet inspection) and filtering for both ingress and egress traffic for your VPC.
- [ ]  This service directly provides the "traffic flow inspection and traffic filtering" capabilities that replicate the functionality of an on-premises inspection server.

Why are the other answers wrong?

- [ ]  Option A is wrong because: Amazon GuardDuty is a threat detection service. It continuously monitors for malicious activity and unauthorized behavior by analyzing AWS CloudTrail logs, VPC Flow Logs, and DNS logs. While GuardDuty inspects traffic patterns to identify threats, it is primarily a detection and alerting service. It does not perform active traffic filtering (blocking or allowing traffic based on rules) in the way a firewall does.
- [ ]  Option B is wrong because: Traffic Mirroring allows you to copy network traffic from an EC2 instance's elastic network interface and send it to out-of-band security and monitoring appliances. While this enables "traffic inspection" by those appliances, Traffic Mirroring itself doesn't perform filtering. You would still need a separate tool or system to analyze the mirrored traffic and then another mechanism to enforce filtering rules. AWS Network Firewall offers inline inspection and filtering.
- [ ]  Option D is wrong because: AWS Firewall Manager is a security management service that helps you centrally configure and manage firewall rules across your accounts and applications. You can use it to manage AWS WAF rules, AWS Shield Advanced protections, VPC security groups, AWS Network Firewall policies, and Amazon Route 53 Resolver DNS Firewall rules. While Firewall Manager would be used to deploy and manage AWS Network Firewall rules at scale, the service that actually provides the inspection and filtering functionality within the VPC is AWS Network Firewall itself. The question asks for the solution that performs the functionalities.

</details>

<details>
  <summary>Question 16</summary>

A company hosts a data lake on AWS. The data lake consists of data in Amazon S3 and Amazon RDS for PostgreSQL. The company needs a reporting solution that provides data visualization and includes all the data sources within the data lake. Only the company's management team should have full access to all the visualizations. The rest of the company should have only limited access.

Which solution will meet these requirements?

- [ ]  A. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate IAM roles. 
- [ ]  B. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate users and groups. 
- [ ]  C. Create an AWS Glue table and crawler for the data in Amazon S3. Create an AWS Glue extract, transform, and load (ETL) job to produce reports. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports. 
- [ ]  D. Create an AWS Glue table and crawler for the data in Amazon S3. Use Amazon Athena Federated Query to access data within Amazon RDS for PostgreSQL. Generate reports by using Amazon Athena. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports. 

</details>

<details>
  <summary>Answer</summary>

- [ ]  B. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate users and groups. 

Why this is the correct answer:

- [ ] Amazon QuickSight for Data Visualization: Amazon QuickSight is a business intelligence (BI) service that enables users to create interactive dashboards and visualizations. It can connect to a wide variety of data sources, including Amazon S3 and Amazon RDS (which includes PostgreSQL).  This directly meets the requirement for a "reporting solution that provides data visualization and includes all the data sources."    
- [ ] Connecting Data Sources: QuickSight allows you to connect to both S3 and RDS for PostgreSQL, create datasets from this data, and then build analyses and dashboards.    
- [ ] Granular Sharing and Permissions: QuickSight has its own user and group management system (which can be integrated with IAM Identity Center or be based on email invitations). Dashboards published in QuickSight can be shared with specific QuickSight users or groups, and you can define their access levels (e.g., viewer or co-owner). This allows the company to give the management team full access while providing limited access to the rest of the company, as required.    

Why are the other answers wrong?

- [ ] Option A is wrong because: While QuickSight is the correct tool, sharing of QuickSight dashboards and analyses is primarily managed through QuickSight's own user and group system, not directly by assigning IAM roles to dashboards for access control in the way IAM roles control access to AWS resources like S3 buckets or EC2 instances. Option B more accurately describes how QuickSight sharing works.    
- [ ] Option C is wrong because: This solution focuses on generating static reports using AWS Glue ETL jobs and storing them in Amazon S3.  While this can fulfill some reporting needs, it does not provide the interactive "data visualization" capabilities that are typically expected from a modern BI solution and are a key feature of QuickSight.  Managing fine-grained access to different visualizations for various user groups through S3 bucket policies can also be more cumbersome than QuickSight's built-in sharing model.   
- [ ] Option D is wrong because: Similar to option C, this approach relies on generating reports (using Amazon Athena with Federated Query for RDS data) and storing them in S3.  While Athena is excellent for querying data in S3 and federated sources, this solution still primarily delivers reports rather than the interactive "data visualization" dashboards that QuickSight provides. 

</details>

<details>
  <summary>Question 17</summary>
  
A company is implementing a new business application. The application runs on two Amazon EC2 instances and uses an Amazon S3 bucket for document storage. A solutions architect needs to ensure that the EC2 instances can access the S3 bucket.

What should the solutions architect do to meet this requirement?

- [ ] A. Create an IAM role that grants access to the S3 bucket. Attach the role to the EC2 instances.
- [ ] B. Create an IAM policy that grants access to the S3 bucket. Attach the policy to the EC2 instances.
- [ ] C. Create an IAM group that grants access to the S3 bucket. Attach the group to the EC2 instances.
- [ ] D. Create an IAM user that grants access to the S3 bucket. Attach the user account to the EC2 instances.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create an IAM role that grants access to the S3 bucket. Attach the role to the EC2 instances.

Why this is the correct answer:

- [ ] IAM Roles for EC2 Instances: The most secure and recommended way for an Amazon EC2 instance to access other AWS services (like Amazon S3) is by using an IAM role.
- [ ] How it Works: You create an IAM role and attach an IAM policy to it that grants the necessary permissions to access the S3 bucket (e.g., s3:GetObject, s3:PutObject).
- [ ] Then, you associate this IAM role with your EC2 instances via an instance profile. The EC2 instances automatically receive temporary security credentials from the role, which the application can use to make requests to S3. This method avoids storing long-term AWS credentials (like access keys) directly on the EC2 instances.

Why are the other answers wrong?

- [ ] Option B is wrong because: While an IAM policy defines the permissions, you cannot directly attach an IAM policy to an EC2 instance. IAM policies are attached to IAM identities such as users, groups, or roles. The role is then attached to the EC2 instance.
- [ ] Option C is wrong because: IAM groups are collections of IAM users and are used to manage permissions for multiple users. You cannot attach an IAM group to an EC2 instance to grant it permissions. EC2 instances leverage IAM roles for permissions.
- [ ] Option D is wrong because: Creating an IAM user and embedding its long-term access keys (access key ID and secret access key) on the EC2 instances is not a security best practice. These credentials could be exposed if the instance is compromised. IAM roles provide a more secure mechanism by using temporary credentials that are automatically rotated.

</details>

<details>
  <summary>Question 18</summary>
 
An application development team is designing a microservice that will convert large images to smaller, compressed images. When a user uploads an image through the web interface, the microservice should store the image in an Amazon S3 bucket, process and compress the image with an AWS Lambda function, and store the image in its compressed form in a different S3 bucket.

A solutions architect needs to design a solution that uses durable, stateless components to process the images automatically.

Which combination of actions will meet these requirements? (Choose two.)

- [ ]  A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.
- [ ]  B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.
- [ ]  C. Configure the Lambda function to monitor the S3 bucket for new uploads. When an uploaded image is detected, write the file name to a text file in memory and use the text file to keep track of the images that were processed.
- [ ]  D. Launch an Amazon EC2 instance to monitor an Amazon Simple Queue Service (Amazon SQS) queue. When items are added to the queue, log the file name in a text file on the EC2 instance and invoke the Lambda function.
- [ ]  E. Configure an Amazon EventBridge (Amazon CloudWatch Events) event to monitor the S3 bucket. When an image is uploaded, send an alert to an Amazon Simple Notification Service (Amazon SNS) topic with the application owner's email address for further processing.

</details>

<details>
  <summary>Answer</summary>

- [ ]  A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.
- [ ]  B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.

Why these are the correct answers:

This solution creates a robust, decoupled, and scalable image processing pipeline using durable and stateless components:

A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.

- [ ]  Durability and Decoupling: When an image is uploaded to the source S3 bucket, an S3 event notification sends a message (containing details like the bucket name and object key) to an SQS queue. SQS is a highly available and durable message queuing service.
- [ ]  This decouples the image upload event from the processing logic. If the processing function (Lambda) is temporarily unavailable or fails, the message remains in the SQS queue and can be processed later, preventing data loss. SQS acts as a durable buffer.

B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.

- [ ]  Stateless Processing: AWS Lambda functions are inherently stateless compute services. Lambda can be configured to automatically poll the SQS queue for new messages.
- [ ]  When a message is received, the Lambda function is invoked to process the image (retrieve from S3, compress, and store in the destination S3 bucket).
- [ ]  Reliable Processing: After the Lambda function successfully processes the image and the associated SQS message, it should delete the message from the queue.
- [ ]  This ensures that the image is processed (at least once, and with idempotent design, effectively once) and prevents reprocessing of the same message.

Why are the other answers wrong?

Option C is wrong because: While Lambda can be triggered directly by S3 events, the part about "write the file name to a text file in memory and use the text file to keep track of the images that were processed" introduces statefulness within the Lambda function's memory. This is not durable; if the Lambda invocation fails before completion or if multiple concurrent invocations occur, this in-memory tracking can be lost or become inconsistent. The requirement is for "durable, stateless components." SQS provides better durability for the event notification.

Option D is wrong because: Using an Amazon EC2 instance to monitor an SQS queue and then invoke a Lambda function adds unnecessary complexity and operational overhead. Lambda can be directly integrated with SQS as an event source, where Lambda polls the queue automatically. Managing an EC2 instance for this task goes against the serverless approach of using Lambda and also introduces a stateful component (the EC2 instance and its local text file) where it's not needed.

Option E is wrong because: While Amazon EventBridge can detect S3 events and Amazon SNS can send notifications, sending an alert to an "application owner's email address for further processing" does not automate the image processing workflow as required. The problem states the Lambda function should process and compress the image. This option describes a manual intervention step, not an automated processing solution.

</details>

<details>
  <summary>Question 19</summary>
   
A company has a three-tier web application that is deployed on AWS. The web servers are deployed in a public subnet in a VPC. The application servers and database servers are deployed in private subnets in the same VPC. The company has deployed a third-party virtual firewall appliance from AWS Marketplace in an inspection VPC. The appliance is configured with an IP interface that can accept IP packets.

A solutions architect needs to integrate the web application with the appliance to inspect all traffic to the application before the traffic reaches the web server.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ]  A. Create a Network Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.
- [ ]  B. Create an Application Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.
- [ ]  C. Deploy a transit gateway in the inspection VPC. Configure route tables to route the incoming packets through the transit gateway.
- [ ]  D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.

</details>

<details>
  <summary>Answer</summary>

- [ ]  D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.

Why this is the correct answer:

- [ ]  Gateway Load Balancer (GWLB): AWS Gateway Load Balancer is specifically designed to make it easy to deploy, scale, and manage third-party virtual network appliances, such as firewalls, intrusion detection and prevention systems (IDS/IPS), and deep packet inspection systems. It operates at Layer 3 (network layer) and is transparent to the traffic flow.   
- [ ]  Gateway Load Balancer Endpoint (GWLBE): You deploy GWLB in the inspection VPC where your virtual firewall appliances are located. Then, you create Gateway Load Balancer Endpoints (GWLBEs) in your application VPC (in this case, in the public subnet or in a subnet that processes traffic before it reaches the web servers).
- [ ]  Route tables in the application VPC are then configured to send traffic destined for the web servers through the GWLBE.
- [ ]  The GWLBE forwards the traffic to the GWLB, which distributes it to the firewall appliances for inspection. After inspection, the traffic is routed back through the GWLBE to its original destination (the web servers).
- [ ]  Least Operational Overhead: This solution simplifies the integration of third-party appliances, providing high availability and scalability for the appliance fleet with less complex routing and configuration compared to other methods. It handles challenges like maintaining flow symmetry.

Why are the other answers wrong?

- [ ]  Option A is wrong because: While a Network Load Balancer (NLB) operates at Layer 4 and could distribute traffic to appliances, it's not specifically designed for transparently inserting a fleet of inspection appliances into the traffic path as effectively as GWLB. GWLB provides features tailored for this use case, like flow stickiness to a specific appliance instance and simpler integration via GWLBEs for inline inspection.
- [ ]  Option B is wrong because: An Application Load Balancer (ALB) operates at Layer 7 (application layer, primarily for HTTP/HTTPS). Virtual firewall appliances typically need to inspect raw IP packets at lower network layers (Layer 3 or 4). An ALB terminates HTTP/HTTPS connections and would not be suitable for forwarding raw IP packets to such an appliance for general inspection.
- [ ]  Option C is wrong because: While a Transit Gateway is used for interconnecting VPCs and on-premises networks and would likely be part of the overall architecture to route traffic between the application VPC and the inspection VPC, it does not itself provide the load balancing and transparent integration capabilities for a fleet of virtual appliances that GWLB offers. You might use Transit Gateway to route traffic to the GWLB, but GWLB is the key component for integrating the appliances themselves.

</details>

<details>
  <summary>Question 20</summary>

A company wants to improve its ability to clone large amounts of production data into a test environment in the same AWS Region. The data is stored in Amazon EC2 instances on Amazon Elastic Block Store (Amazon EBS) volumes. Modifications to the cloned data must not affect the production environment. The software that accesses this data requires consistently high I/O performance.

A solutions architect needs to minimize the time that is required to clone the production data into the test environment.

Which solution will meet these requirements?

- [ ] A. Take EBS snapshots of the production EBS volumes. Restore the snapshots onto EC2 instance store volumes in the test environment.
- [ ] B. Configure the production EBS volumes to use the EBS Multi-Attach feature. Take EBS snapshots of the production EBS volumes. Attach the production EBS volumes to the EC2 instances in the test environment.
- [ ] C. Take EBS snapshots of the production EBS volumes. Create and initialize new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.
- [ ] D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.

Why this is the correct answer:

- [ ] EBS Snapshots for Independent Clones: Taking EBS snapshots of the production volumes and then restoring these snapshots to new EBS volumes is the standard method for creating independent clones. This ensures that "Modifications to the cloned data must not affect the production environment."
- [ ] EBS Fast Snapshot Restore (FSR): Normally, when an EBS volume is created from a snapshot, data is loaded lazily from S3 (where snapshots are stored) to the EBS volume.
- [ ] This can result in higher latency for initial I/O operations until the volume is fully hydrated.
- [ ] EBS Fast Snapshot Restore allows you to enable faster restores of EBS snapshots by fully pre-warming the snapshot data. When you create a volume from an FSR-enabled snapshot, it receives its full I/O performance immediately.
- [ ] This addresses the requirements for "consistently high I/O performance" from the outset and helps to "minimize the time that is required to clone the production data" by making the cloned volumes performant right away.
- [ ] New EBS Volumes for Test: Restoring snapshots into new EBS volumes for the test environment ensures data isolation.

Why are the other answers wrong?

- [ ] Option A is wrong because: EC2 instance store volumes are ephemeral storage, meaning data is lost if the instance is stopped, hibernated, or terminated. While they offer very high I/O performance, they are not suitable for storing cloned production data that needs to persist for a test environment. Restoring an EBS snapshot to an instance store is generally not a recommended or durable approach for this use case.
- [ ] Option B is wrong because: EBS Multi-Attach allows a single Provisioned IOPS SSD EBS volume to be attached to multiple EC2 instances within the same Availability Zone. However, attaching the production EBS volumes directly to test environment EC2 instances, even after taking snapshots, is highly risky and directly contradicts the requirement that modifications to test data must not affect production. This would lead to data corruption in the production environment if the test instances write to the shared production volumes.
- [ ] Option C is wrong because: The wording "Create and initialize new EBS volumes... Attach the new EBS volumes... before restoring the volumes from the production EBS snapshots" is illogical. You typically create a new volume from a snapshot, or restore a snapshot to a new volume. More importantly, if you just create a volume from a regular snapshot (without FSR), it will still undergo the lazy loading process, which can impact initial I/O performance. This option does not guarantee consistently high I/O performance from the moment of creation, nor does it minimize the time to achieve full performance as effectively as FSR.

</details>


</details>




















<details>
  <summary>Question X</summary>

- [ ]    


</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn


</details>










