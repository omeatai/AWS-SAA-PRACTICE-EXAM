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
  <summary>Question X</summary>

- [ ]    

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn on S3 Transfer Acceleration

</details>










