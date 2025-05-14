# AWS-SAA-PRACTICE-EXAM
AWS-SAA-PRACTICE-EXAM

<details>
  <summary>==Questions 1-20==</summary>
  
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
  <summary>==Questions 21-30==</summary>

<details>
  <summary>Question 21</summary>

An ecommerce company wants to launch a one-deal-a-day website on AWS. Each day will feature exactly one product on sale for a period of 24 hours. The company wants to be able to handle millions of requests each hour with millisecond latency during peak hours.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ]  A. Use Amazon S3 to host the full website in different S3 buckets. Add Amazon CloudFront distributions. Set the S3 buckets as origins for the distributions. Store the order data in Amazon S3.
- [ ]  B. Deploy the full website on Amazon EC2 instances that run in Auto Scaling groups across multiple Availability Zones. Add an Application Load Balancer (ALB) to distribute the website traffic. Add another ALB for the backend APIs. Store the data in Amazon RDS for MySQL.
- [ ]  C. Migrate the full application to run in containers. Host the containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use the Kubernetes Cluster Autoscaler to increase and decrease the number of pods to process bursts in traffic. Store the data in Amazon RDS for MySQL.
- [ ]  D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.

</details>

<details>
  <summary>Answer</summary>

- [ ]  D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.

Why this is the correct answer:

- [ ] Static Content Hosting (S3 and CloudFront): For a "one-deal-a-day" website, much of the content (product description, images, pricing for that day) is static.
- [ ] Hosting this static content (HTML, CSS, JavaScript, images) in an Amazon S3 bucket and distributing it via Amazon CloudFront provides high scalability ("millions of requests each hour") and low latency ("millisecond latency") due to CloudFront's global network of edge locations. This setup has very low operational overhead.
- [ ] Serverless Backend (API Gateway and Lambda): For dynamic functionalities like processing orders, handling user interactions, or inventory checks, using Amazon API Gateway to create RESTful APIs that trigger AWS Lambda functions is a highly scalable, pay-per-use, and low operational overhead approach. Lambda functions can execute code in response to API calls without needing to manage servers.
- [ ] Scalable Database (DynamoDB): Amazon DynamoDB is a fully managed NoSQL database service that delivers single-digit millisecond performance at any scale.
- [ ] It is well-suited for ecommerce applications that require low-latency data access and can handle high request volumes, such as storing order information or user session data. Its serverless nature means no servers to manage.   
- [ ] Least Operational Overhead: This combination of serverless and managed services (S3, CloudFront, API Gateway, Lambda, DynamoDB) minimizes infrastructure management, patching, and manual scaling efforts, directly meeting the requirement for the "LEAST operational overhead."

Why are the other answers wrong?

- [ ] Option A is wrong because: While using S3 and CloudFront for hosting is good, storing "order data in Amazon S3" is not appropriate for a transactional system. S3 is an object store, not a database designed for frequent, low-latency transactional reads and writes with querying capabilities needed for order management.
- [ ] Option B is wrong because: Deploying the application on Amazon EC2 instances, even with Auto Scaling and Application Load Balancers, involves significantly more operational overhead (managing instances, operating systems, scaling policies, patching) compared to a serverless architecture like the one described in option D.
- [ ] Option C is wrong because: Migrating to containers on Amazon EKS introduces considerable operational complexity. Managing a Kubernetes cluster, even with EKS, requires expertise and effort for cluster operations, node management, and container orchestration. This is generally more operationally intensive than a serverless approach, especially when "LEAST operational overhead" is a primary goal.

</details>


<details>
  <summary>Question 22</summary>
  
A solutions architect is using Amazon S3 to design the storage architecture of a new digital media application. The media files must be resilient to the loss of an Availability Zone. Some files are accessed frequently while other files are rarely accessed in an unpredictable pattern. The solutions architect must minimize the costs of storing and retrieving the media files.

Which storage option meets these requirements?

- [ ]  A. S3 Standard
- [ ]  B. S3 Intelligent-Tiering
- [ ]  C. S3 Standard-Infrequent Access (S3 Standard-IA)
- [ ]  D. S3 One Zone-Infrequent Access (S3 One Zone-IA)

</details>

<details>
  <summary>Answer</summary>

- [ ]  B. S3 Intelligent-Tiering

Why this is the correct answer:

- [ ] Resilience to Availability Zone Loss: S3 Intelligent-Tiering stores data across multiple Availability Zones (AZs) by default, just like S3 Standard and S3 Standard-IA. This means it is resilient to the loss of a single AZ.    
- [ ] Handles Unpredictable Access Patterns: The S3 Intelligent-Tiering storage class is specifically designed for data with unknown, changing, or unpredictable access patterns.
- [ ] It automatically monitors access patterns and moves objects that have not been accessed for a certain period to a lower-cost infrequent access tier.
- [ ] If an object in the infrequent access tier is accessed, it is automatically moved back to the frequent access tier. This is done without performance impact or retrieval fees between these two tiers.
- [ ] This perfectly matches the requirement for files accessed "frequently while other files are rarely accessed in an unpredictable pattern."    
- [ ] Cost Minimization: By automatically tiering data based on access, S3 Intelligent-Tiering helps to minimize storage costs without the operational overhead of creating and managing S3 Lifecycle policies for data with unpredictable access.
- [ ] It provides the performance of S3 Standard for frequently accessed objects and the cost savings of an infrequent access tier for objects that become cold.
- [ ] There are no retrieval fees when data is moved between the frequent and infrequent access tiers within S3 Intelligent-Tiering.    

Why are the other answers wrong?

- [ ] Option A is wrong because: S3 Standard is designed for frequently accessed data and provides high durability and availability across multiple AZs.  However, it is not the most cost-effective option if a significant portion of the media files are rarely accessed, as it has a higher storage cost compared to infrequent access tiers.  It doesn't automatically optimize costs for unpredictable access patterns.    
- [ ] Option C is wrong because: S3 Standard-Infrequent Access (S3 Standard-IA) is designed for data that is accessed less frequently but requires rapid access when needed and is resilient to AZ loss. While it offers lower storage costs than S3 Standard, it has data retrieval fees. For "unpredictable access patterns," if data thought to be infrequent is suddenly accessed frequently, these retrieval fees could add up and might negate the storage cost savings. S3 Intelligent-Tiering avoids this retrieval fee concern between its main tiers.    
- [ ] Option D is wrong because: S3 One Zone-Infrequent Access (S3 One Zone-IA) stores data in a single Availability Zone. This means it is not resilient to the loss of an Availability Zone, which is a specific requirement ("media files must be resilient to the loss of an Availability Zone").  While it is a lower-cost option, it doesn't meet the resiliency criteria.

</details>

<details>
  <summary>Question 23</summary>

A company is storing backup files by using Amazon S3 Standard storage. The files are accessed frequently for 1 month. However, the files are not accessed after 1 month. The company must keep the files indefinitely.

Which storage solution will meet these requirements MOST cost-effectively?

- [ ]  A. Configure S3 Intelligent-Tiering to automatically migrate objects.
- [ ]  B. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.
- [ ]  C. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month.
- [ ]  D. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month.

</details>

<details>
  <summary>Answer</summary>

- [ ]  B. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.

Why this is the correct answer:

- [ ] Predictable Access Pattern: The problem states a clear and predictable access pattern: files are accessed frequently for the first month and then not accessed thereafter, but must be kept indefinitely.
- [ ] S3 Glacier Deep Archive for Long-Term, Cold Storage: Amazon S3 Glacier Deep Archive is the lowest-cost storage class in Amazon S3 and is designed for long-term data archiving where data is rarely accessed (e.g., once or twice a year) and retrieval times of several hours are acceptable.
- [ ] Since the backup files are not accessed after 1 month and must be kept indefinitely, transitioning them to S3 Glacier Deep Archive is the MOST cost-effective solution for long-term storage.
- [ ] S3 Lifecycle Configuration: An S3 Lifecycle configuration can be easily set up to automatically transition objects from S3 Standard (where they reside for the first month of frequent access) directly to S3 Glacier Deep Archive after 30 days (or 1 month).

Why are the other answers wrong?

- [ ] Option A is wrong because: S3 Intelligent-Tiering is optimized for data with unknown, changing, or unpredictable access patterns. In this scenario, the access pattern is known and predictable. While S3 Intelligent-Tiering can eventually move data to archive access tiers, for data that is definitively cold and will not be accessed after a specific period, a direct lifecycle transition to S3 Glacier Deep Archive is generally more cost-effective and straightforward. S3 Intelligent-Tiering also incurs a small per-object monitoring and automation fee.
- [ ] Option C is wrong because: S3 Standard-Infrequent Access (S3 Standard-IA) is designed for data that is accessed less frequently but still requires rapid access when needed. While it has lower storage costs than S3 Standard, its storage costs are significantly higher than S3 Glacier Deep Archive. Given that the files are not accessed at all after one month and must be kept indefinitely, S3 Standard-IA is not the most cost-effective long-term archival solution.
- [ ] Option D is wrong because: S3 One Zone-Infrequent Access (S3 One Zone-IA) stores data in a single Availability Zone. This means it is not resilient to the physical loss of that Availability Zone. For backup files that must be kept "indefinitely," relying on a storage class with lower durability (due to being single-AZ) is generally not advisable, especially when multi-AZ archival options like S3 Glacier Deep Archive offer very low costs with higher durability. The priority for indefinite backups should include durability.

</details>

<details>
  <summary>Question 24</summary>
   
A company observes an increase in Amazon EC2 costs in its most recent bill. The billing team notices unwanted vertical scaling of instance types for a couple of EC2 instances. A solutions architect needs to create a graph comparing the last 2 months of EC2 costs and perform an in-depth analysis to identify the root cause of the vertical scaling.

How should the solutions architect generate the information with the LEAST operational overhead?

- [ ] A. Use AWS Budgets to create a budget report and compare EC2 costs based on instance types.
- [ ] B. Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.
- [ ] C. Use graphs from the AWS Billing and Cost Management dashboard to compare EC2 costs based on instance types for the last 2 months.
- [ ] D. Use AWS Cost and Usage Reports to create a report and send it to an Amazon S3 bucket. Use Amazon QuickSight with Amazon S3 as a source to generate an interactive graph based on instance types.



</details>

<details>
  <summary>Answer</summary>

- [ ]   B. Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.

Why this is the correct answer:

- [ ] AWS Cost Explorer for Analysis: AWS Cost Explorer is an interactive tool that allows you to visualize, understand, and manage your AWS costs and usage over time.
- [ ] It is specifically designed for ad-hoc analysis and exploration of cost data.
- [ ] Graphing and Comparison: Cost Explorer can easily generate graphs comparing costs over specified periods, such as the last 2 months.
- [ ] Granular Filtering by Instance Type: A key feature of Cost Explorer is its ability to filter and group cost and usage data by various dimensions, including service (EC2), region, usage type, and critically for this scenario, instance type. This allows the solutions architect to drill down into EC2 costs, identify trends related to specific instance types, and investigate the "unwanted vertical scaling."
- [ ] Least Operational Overhead: Using Cost Explorer's built-in interface and features for this type of analysis requires minimal setup. The data is readily available, and the tool provides the necessary graphing and filtering capabilities out-of-the-box. This aligns with the requirement for the "LEAST operational overhead."

Why are the other answers wrong?

- [ ] Option A is wrong because: AWS Budgets is primarily used for setting spending limits and receiving alerts when costs or usage exceed (or are forecasted to exceed) those limits. While budget reports can provide some insights, Cost Explorer is the more appropriate tool for detailed, ad-hoc investigation and root cause analysis of cost spikes or specific usage patterns like changes in instance types.
- [ ] Option C is wrong because: The AWS Billing and Cost Management dashboard provides a high-level overview of your AWS spending (e.g., month-to-date spend by service, cost trends). While it offers some basic graphs, it generally lacks the deep, granular filtering and analytical capabilities of Cost Explorer needed to perform an "in-depth analysis" to identify the root cause of vertical scaling by instance type over a 2-month period.
- [ ] Option D is wrong because: AWS Cost and Usage Reports (CUR) provide the most detailed cost and usage data, but analyzing this data typically requires additional tools. Setting up CUR, configuring delivery to an S3 bucket, and then using Amazon QuickSight (or another BI tool) to ingest, prepare, and visualize this data involves significantly more setup, configuration, and operational overhead compared to using Cost Explorer directly for this specific task. While powerful for complex reporting, it's not the solution with the "LEAST operational overhead" for this scenario.

</details>


<details>
  <summary>Question 25</summary>

A company is designing an application. The application uses an AWS Lambda function to receive information through Amazon API Gateway and to store the information in an Amazon Aurora PostgreSQL database. During the proof-of-concept stage, the company has to increase the Lambda quotas significantly to handle the high volumes of data that the company needs to load into the database.

A solutions architect must recommend a new design to improve scalability and minimize the configuration effort.

Which solution will meet these requirements?

- [ ] A. Refactor the Lambda function code to Apache Tomcat code that runs on Amazon EC2 instances. Connect the database by using native Java Database Connectivity (JDBC) drivers.
- [ ] B. Change the platform from Aurora to Amazon DynamoDB. Provision a DynamoDB Accelerator (DAX) cluster. Use the DAX client SDK to point the existing DynamoDB API calls at the DAX cluster.
- [ ] C. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using Amazon Simple Notification Service (Amazon SNS).
- [ ] D. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using an Amazon Simple Queue Service (Amazon SQS) queue.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using an Amazon Simple Queue Service (Amazon SQS) queue.

Why this is the correct answer:

- [ ] Decoupling with SQS for Scalability and Resilience: The core issue is handling "high volumes of data" that stressed the original Lambda function, requiring quota increases. By introducing an Amazon SQS queue, you decouple the data ingestion (from API Gateway) from the database writing process.
- [ ] The first Lambda function (or API Gateway directly) receives the information and quickly places it as a message onto an SQS queue. This is a fast operation.
- [ ] The SQS queue acts as a durable buffer, holding these messages.
- [ ] The second Lambda function is configured to process messages from the SQS queue and write them to the Aurora PostgreSQL database. This function can process messages at a rate that the database can comfortably handle, preventing it from being overwhelmed.
- [ ] Improved Scalability: SQS can handle massive throughput of messages. The Lambda functions can scale independently. The receiving Lambda can scale to handle bursts from API Gateway, and the database-writing Lambda can scale based on the queue depth, with its concurrency potentially limited to avoid overloading the database. This effectively manages the "high volumes of data."
- [ ] Minimized Configuration Effort (for the new design): This serverless pattern (API Gateway -> SQS -> Lambda -> Database) is a common and well-supported architecture. While it adds an SQS queue and potentially a second Lambda, it leverages managed services and generally involves less ongoing configuration and operational overhead for scaling and maintenance compared to moving to EC2 instances (Option A). It addresses the Lambda scaling issue without a complete platform overhaul (Option B).
- [ ] Resilience: If the database-writing Lambda fails or is temporarily throttled, messages remain in the SQS queue for later processing (potentially with a Dead-Letter Queue for error handling), improving the overall resilience of the data loading process.

Why are the other answers wrong?

- [ ] Option A is wrong because: Refactoring to an Apache Tomcat application running on Amazon EC2 instances introduces significant operational overhead (managing servers, patching, scaling EC2 infrastructure) compared to the serverless Lambda approach. This contradicts the goal of minimizing configuration effort, especially for scaling and maintenance.
- [ ] Option B is wrong because: Changing the database platform from Amazon Aurora PostgreSQL (a relational database) to Amazon DynamoDB (a NoSQL database) is a major architectural shift. This would likely require substantial changes to the application's data model, query patterns, and overall code, which is far from "minimizing configuration effort" in the context of solving a Lambda scaling problem. While DynamoDB with DAX is highly scalable, this solution doesn't directly address the Lambda processing bottleneck without extensive rework.
- [ ] Option C is wrong because: While Amazon SNS can be used to trigger a Lambda function, SQS is generally a better choice for buffering and decoupling when there's a work queue involved, especially for tasks like writing to a database that might have rate limitations. SQS provides features like message visibility timeouts, retry mechanisms, and dead-letter queues (DLQs) that are very useful for managing asynchronous task processing. SNS is more suited for fan-out notification patterns to multiple, independent subscribers. For a one-to-one handoff with buffering, SQS is preferred.

</details>

<details>
  <summary>Question 26</summary>

A company needs to review its AWS Cloud deployment to ensure that its Amazon S3 buckets do not have unauthorized configuration changes.

What should a solutions architect do to accomplish this goal?

- [ ] A. Turn on AWS Config with the appropriate rules.
- [ ] B. Turn on AWS Trusted Advisor with the appropriate checks.
- [ ] C. Turn on Amazon Inspector with the appropriate assessment template.
- [ ] D. Turn on Amazon S3 server access logging. Configure Amazon EventBridge (Amazon CloudWatch Events).

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Turn on AWS Config with the appropriate rules.

Why this is the correct answer:

- [ ] AWS Config for Configuration Auditing: AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. It continuously monitors and records resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations.   
- [ ] Detecting Unauthorized Changes: For Amazon S3 buckets, AWS Config can track configuration changes such as bucket policies, ACLs, versioning settings, and more. You can use AWS Config Rules (predefined or custom) to define your desired configurations (e.g., no public read access, logging enabled). If an S3 bucket configuration deviates from these rules or an unauthorized change is made, AWS Config will flag it. This directly helps in ensuring that S3 buckets "do not have unauthorized configuration changes."
- [ ] Configuration History: AWS Config maintains a history of configuration changes, which is valuable for auditing and troubleshooting.

Why are the other answers wrong?

- [ ] Option B is wrong because: AWS Trusted Advisor provides recommendations to help you follow AWS best practices across cost optimization, performance, security, fault tolerance, and service limits. While it may offer some checks related to S3 security (like identifying buckets with open access permissions), it's primarily a guidance tool and does not provide the continuous, detailed configuration tracking, auditing, and history features that AWS Config offers for identifying specific unauthorized changes.
- [ ] Option C is wrong because: Amazon Inspector is an automated security assessment service that focuses on identifying vulnerabilities and deviations from best practices within your Amazon EC2 instances (e.g., checking for software vulnerabilities, network exposure). It does not monitor or audit the configurations of Amazon S3 buckets.
- [ ] Option D is wrong because: Amazon S3 server access logging records all requests made to an S3 bucket, which is useful for access auditing. Amazon EventBridge (formerly CloudWatch Events) can react to API calls made to S3. While you could potentially build a complex custom solution using these services to infer configuration changes, AWS Config is the purpose-built service for comprehensive configuration auditing and tracking with significantly less operational overhead. AWS Config provides a managed and integrated solution for this specific requirement.

</details>

<details>
  <summary>Question 27</summary>

A company is launching a new application and will display application metrics on an Amazon CloudWatch dashboard. The company's product manager needs to access this dashboard periodically. The product manager does not have an AWS account. A solutions architect must provide access to the product manager by following the principle of least privilege.

Which solution will meet these requirements?

- [ ] A. Share the dashboard from the CloudWatch console. Enter the product manager's email address, and complete the sharing steps. Provide a shareable link for the dashboard to the product manager.
- [ ] B. Create an IAM user specifically for the product manager. Attach the CloudWatchReadOnlyAccess AWS managed policy to the user. Share the new login credentials with the product manager. Share the browser URL of the correct dashboard with the product manager.
- [ ] C. Create an IAM user for the company's employees. Attach the ViewOnlyAccess AWS managed policy to the IAM user. Share the new login credentials with the product manager. Ask the product manager to navigate to the CloudWatch console and locate the dashboard by name in the Dashboards section.
- [ ] D. Deploy a bastion server in a public subnet. When the product manager requires access to the dashboard, start the server and share the RDP credentials. On the bastion server, ensure that the browser is configured to open the dashboard URL with cached AWS credentials that have appropriate permissions to view the dashboard.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Share the dashboard from the CloudWatch console. Enter the product manager's email address, and complete the sharing steps. Provide a shareable link for the dashboard to the product manager.

Why this is the correct answer:

- [ ] CloudWatch Dashboard Sharing Feature: Amazon CloudWatch allows you to share dashboards with individuals outside of your AWS account, including those who do not have AWS credentials. You can share a dashboard publicly (not recommended for sensitive data but possible) or with specific email addresses. The recipient receives a unique link to access the dashboard.    
- [ ] No AWS Account Required for Product Manager: This method directly addresses the constraint that "The product manager does not have an AWS account."    
- [ ] Principle of Least Privilege: Sharing a specific dashboard grants view-only access to only that dashboard's content. The product manager does not gain access to the AWS Management Console or any other AWS resources, adhering strictly to the principle of least privilege.    
- [ ] Simplicity and Low Operational Overhead: This is the simplest and most straightforward method, involving minimal configuration and no need to manage IAM users or credentials for the product manager.    

Why are the other answers wrong?

- [ ] Option B is wrong because: This solution requires creating an IAM user for the product manager, which contradicts the statement that "The product manager does not have an AWS account."  While using the CloudWatchReadOnlyAccess policy is more specific than ViewOnlyAccess, the creation of an IAM user for an external party who only needs dashboard access is unnecessary overhead when a direct sharing feature exists.    
- [ ] Option C is wrong because: Similar to option B, this involves creating an IAM user. Furthermore, the ViewOnlyAccess AWS managed policy grants read-only access to all AWS services and resources in the account. This is a significant violation of the principle of least privilege when the requirement is solely to view a single CloudWatch dashboard.    
- [ ] Option D is wrong because: Deploying and managing a bastion server, handling RDP credentials, and relying on cached AWS credentials for browser access is an extremely complex, insecure, and operationally intensive solution for providing view-only access to a CloudWatch dashboard.  It introduces multiple potential security vulnerabilities and significant overhead.   

</details>

<details>
  <summary>Question 28</summary>

A company is migrating applications to AWS. The applications are deployed in different accounts. The company manages the accounts centrally by using AWS Organizations. The company's security team needs a single sign-on (SSO) solution across all the company's accounts. The company must continue managing the users and groups in its on-premises self-managed Microsoft Active Directory.

Which solution will meet these requirements?

- [ ] A. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a one-way forest trust or a one-way domain trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
- [ ] B. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a two-way forest trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.
- [ ] C. Use AWS Directory Service. Create a two-way trust relationship with the company's self-managed Microsoft Active Directory.
- [ ] D. Deploy an identity provider (IdP) on premises. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a two-way forest trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.

Why this is the correct answer:

- [ ] AWS Single Sign-On (AWS IAM Identity Center): AWS Single Sign-On (now known as AWS IAM Identity Center) is the recommended service for centrally managing SSO access to multiple AWS accounts within an AWS Organization, as well as to business applications.
- [ ] Integration with On-Premises Active Directory: To allow users from an on-premises self-managed Microsoft Active Directory to use IAM Identity Center, you typically use AWS Directory Service for Microsoft Active Directory (AWS Managed Microsoft AD) as an intermediary.
- [ ] A trust relationship is established between your on-premises AD and the AWS Managed Microsoft AD.
- [ ] Two-Way Forest Trust: A two-way trust (either forest or domain, though forest trust is often broader) allows users and groups from the on-premises AD to be recognized and authenticated by the AWS Managed Microsoft AD. This AWS Managed Microsoft AD is then configured as the identity source for IAM Identity Center.
- [ ] This setup ensures that user and group management continues to happen in the on-premises AD, and these identities can be used to sign in via IAM Identity Center to access assigned AWS accounts and permission sets.
- [ ] The two-way nature of the trust facilitates the necessary authentication and authorization lookups.

Why are the other answers wrong?

- [ ] Option A is wrong because: While a one-way trust might allow some level of authentication, a two-way trust is generally required or recommended for full functionality when integrating an on-premises AD with AWS Managed Microsoft AD for IAM Identity Center. The AWS Managed AD needs to be able to authenticate users from the on-premises AD, and users in the on-premises AD need to be able to authenticate against resources that trust the AWS Managed AD. A two-way trust typically provides more seamless integration for this scenario.
- [ ] Option C is wrong because: Simply creating an AWS Directory Service (e.g., AWS Managed Microsoft AD) and establishing a two-way trust with the on-premises AD allows for AD-based authentication for resources that are domain-joined or AD-aware (like specific EC2 instances). However, it does not, by itself, provide the centralized single sign-on portal and management of access across all AWS accounts in the organization that AWS IAM Identity Center offers. IAM Identity Center is the key component for SSO to AWS accounts.
- [ ] Option D is wrong because: While deploying an on-premises Identity Provider (IdP) like ADFS and using SAML 2.0 federation is a valid way to achieve SSO to AWS, this option is vague about how it integrates with AWS IAM Identity Center. If the goal is to leverage IAM Identity Center's features for managing access across the AWS Organization using existing on-premises AD identities, the path involving AWS Managed Microsoft AD and a trust relationship (as described in options A and B) is a common pattern. This option also doesn't specify the connection mechanism between the on-premises IdP and AWS SSO for using on-premises AD users.

</details>

<details>
  <summary>Question 29</summary>

A company provides a Voice over Internet Protocol (VoIP) service that uses UDP connections. The service consists of Amazon EC2 instances that run in an Auto Scaling group. The company has deployments across multiple AWS Regions. The company needs to route users to the Region with the lowest latency. The company also needs automated failover between Regions.

Which solution will meet these requirements?

- [ ] A. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Use the NLB as an AWS Global Accelerator endpoint in each Region.
- [ ] B. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Use the ALB as an AWS Global Accelerator endpoint in each Region.
- [ ] C. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 latency record that points to aliases for each NLB. Create an Amazon CloudFront distribution that uses the latency record as an origin.
- [ ] D. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 weighted record that points to aliases for each ALB. Deploy an Amazon CloudFront distribution that uses the weighted record as an origin.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Use the NLB as an AWS Global Accelerator endpoint in each Region.

Why this is the correct answer:

- [ ] Network Load Balancer (NLB) for UDP: The VoIP service uses UDP connections. NLB operates at the transport layer (Layer 4) and supports both TCP and UDP traffic, making it suitable for this use case.    
- [ ] AWS Global Accelerator for Low Latency and Failover: AWS Global Accelerator improves the availability and performance of your applications with local or global users.
- [ ] It directs traffic to optimal endpoints over the AWS global network. This helps in routing users to the Region with the "lowest latency."
- [ ] Global Accelerator also provides "automated failover between Regions" by continuously monitoring the health of regional endpoints (the NLBs in this case) and routing traffic only to healthy ones.    
- [ ] NLB as Global Accelerator Endpoint: NLBs in each of the multiple AWS Regions can be configured as endpoints for Global Accelerator.  This allows Global Accelerator to manage and direct traffic to the appropriate regional deployment.   
- [ ] Auto Scaling Integration: The NLB's target group is associated with the Auto Scaling group of EC2 instances, ensuring that traffic within each region is distributed across healthy, scalable instances.    

Why are the other answers wrong?

- [ ] Option B is wrong because: Application Load Balancer (ALB) operates at the application layer (Layer 7) and supports HTTP, HTTPS, and gRPC protocols. It does not support UDP traffic, which is a requirement for the VoIP service.    
- [ ] Option C is wrong because: While Amazon Route 53 latency-based routing can direct users to the region with the lowest latency, and NLB handles UDP, Amazon CloudFront is a content delivery network (CDN) primarily designed for caching and delivering web content (HTTP/HTTPS). It is not suitable for distributing real-time UDP traffic for a VoIP service. AWS Global Accelerator is better designed for non-HTTP use cases like UDP and for providing static IP addresses that act as a fixed entry point.    
- [ ] Option D is wrong because: This option has multiple issues. As mentioned, ALB does not support UDP.  Amazon CloudFront is not suitable for UDP traffic distribution.  Also, Amazon Route 53 weighted records distribute traffic based on predefined weights, not primarily on dynamic lowest latency routing or the automated failover capabilities that Global Accelerator provides for global applications. 

</details>

<details>
  <summary>Question 30</summary>

A development team runs monthly resource-intensive tests on its general purpose Amazon RDS for MySQL DB instance with Performance Insights enabled. The testing lasts for 48 hours once a month and is the only process that uses the database. The team wants to reduce the cost of running the tests without reducing the compute and memory attributes of the DB instance.

Which solution meets these requirements MOST cost-effectively?

- [ ] A. Stop the DB instance when tests are completed. Restart the DB instance when required.
- [ ] B. Use an Auto Scaling policy with the DB instance to automatically scale when tests are completed.
- [ ] C. Create a snapshot when tests are completed. Terminate the DB instance and restore the snapshot when required.
- [ ] D. Modify the DB instance to a low-capacity instance when tests are completed. Modify the DB instance again when required.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create a snapshot when tests are completed. Terminate the DB instance and restore the snapshot when required.

Why this is the correct answer:

- [ ] Significant Cost Savings: The database is only used for 48 hours (2 days) a month. By creating a snapshot of the DB instance after testing, terminating the instance, and then restoring it from the snapshot before the next testing cycle, the company avoids paying for DB instance hours for the majority of the month (approximately 28 days). You only pay for the storage of the snapshot, which is significantly cheaper than running an RDS instance. This makes it the "MOST cost-effectively" solution.
- [ ] Maintains Compute and Memory Attributes: When the instance is restored from the snapshot, it can be launched with the same instance class (compute and memory attributes) as the original, ensuring that the testing environment is consistent.
- [ ] Data Preservation: The snapshot preserves all the data from the DB instance at the time the snapshot was taken.    

Why are the other answers wrong?

- [ ] Option A is wrong because: While stopping an Amazon RDS DB instance does stop billing for the instance hours, you are still billed for the provisioned storage. More critically, an RDS instance can only be stopped for a maximum of 7 consecutive days. After 7 days, AWS automatically starts the instance. Since the tests are run "once a month," this means the instance would be automatically started multiple times between tests, incurring unnecessary costs.    
- [ ] Option B is wrong because: Amazon RDS for MySQL (general purpose, not Aurora Serverless) does not have an "Auto Scaling policy" that scales the primary DB instance's compute capacity down to zero or significantly reduces its cost when idle in the way EC2 Auto Scaling does for instances. While Aurora Serverless offers compute scaling, the question refers to a "general purpose Amazon RDS for MySQL DB instance."
- [ ] Option D is wrong because: Modifying a DB instance to a lower-capacity instance type when tests are completed and then modifying it back before the next tests involves instance modification operations. These operations can take time and might involve downtime (though RDS attempts to minimize this for Multi-AZ instances). Furthermore, even a low-capacity instance still incurs charges for instance hours and storage throughout the month. Terminating the instance (as in option C) eliminates the instance hour charges entirely during the idle period, making it more cost-effective. 

</details>


</details>

<details>
  <summary>==Questions 31-40==</summary>

<details>
  <summary>Question 31</summary>

A company that hosts its web application on AWS wants to ensure all Amazon EC2 instances, Amazon RDS DB instances, and Amazon Redshift clusters are configured with tags.  The company wants to minimize the effort of configuring and operating this check.    

What should a solutions architect do to accomplish this?    

- [ ] A. Use AWS Config rules to define and detect resources that are not properly tagged. 
- [ ] B. Use Cost Explorer to display resources that are not properly tagged. Tag those resources manually. 
- [ ] C. Write API calls to check all resources for proper tag allocation. Periodically run the code on an EC2 instance. 
- [ ] D. Write API calls to check all resources for proper tag allocation. Schedule an AWS Lambda function through Amazon CloudWatch to periodically run the code.    

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Use AWS Config rules to define and detect resources that are not properly tagged. 

Why this is the correct answer:

- [ ] AWS Config for Configuration Compliance: AWS Config is a service that allows you to assess, audit, and evaluate the configurations of your AWS resources.  It's designed for tracking resource configurations and ensuring compliance with desired policies.   
- [ ] Config Rules for Tagging: AWS Config provides managed rules (like required-tags) and the ability to create custom rules to check whether your resources (including EC2 instances, RDS DB instances, and Redshift clusters) have the necessary tags.    
- [ ] Automated Detection and Minimal Effort: Once set up, AWS Config rules automatically and continuously evaluate your resources. If a resource is found to be non-compliant (e.g., missing required tags), AWS Config can flag it. This provides an automated way to perform the check with minimal ongoing operational effort.    

Why are the other answers wrong?

- [ ] Option B is wrong because: AWS Cost Explorer is a tool for visualizing, understanding, and managing your AWS costs and usage. While it can use tags to filter cost data, it is not designed as a primary tool for detecting resources that are missing required tags for compliance purposes.  Its focus is cost analysis, not configuration auditing of tags.   
- [ ] Option C is wrong because: Writing custom scripts with API calls to check for tags and running them periodically on an EC2 instance involves significant initial development effort and ongoing operational overhead for maintaining the script and the EC2 instance.  This does not minimize effort compared to using a managed service like AWS Config.   
- [ ] Option D is wrong because: Similar to option C, developing custom API calls within an AWS Lambda function scheduled by Amazon CloudWatch (now Amazon EventBridge) requires custom coding and maintenance.  While serverless, it's still more effort to develop and manage than using the built-in capabilities of AWS Config, which is specifically designed for configuration compliance checks like ensuring proper tagging.

</details>  

<details>
  <summary>Question 32</summary>

A development team needs to host a website that will be accessed by other teams. The website contents consist of HTML, CSS, client-side JavaScript, and images.

Which method is the MOST cost-effective for hosting the website?    

- [ ] A. Containerize the website and host it in AWS Fargate. 
- [ ] B. Create an Amazon S3 bucket and host the website there. 
- [ ] C. Deploy a web server on an Amazon EC2 instance to host the website. 
- [ ] D. Configure an Application Load Balancer with an AWS Lambda target that uses the Express.js framework.    

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create an Amazon S3 bucket and host the website there. 

Why this is the correct answer:

- [ ] Amazon S3 for Static Website Hosting: Amazon S3 has a feature that allows you to configure a bucket to host a static website.  Since the website consists only of static content (HTML, CSS, client-side JavaScript, and images), S3 is an ideal choice.    
- [ ] MOST Cost-Effective: Hosting a static website on S3 is generally the most cost-effective option.  You primarily pay for the S3 storage consumed by your website files and the data transfer out (requests for your website content). There are no charges for compute instances, making it significantly cheaper than solutions involving EC2, Fargate, or Lambda for simple static content delivery.   
- [ ] Scalability and Durability: S3 is inherently highly scalable, durable, and available, automatically handling variations in traffic without requiring manual intervention.

Why are the other answers wrong?

- [ ] Option A is wrong because: Containerizing a purely static website and hosting it on AWS Fargate is an overly complex and more expensive solution.  Fargate is a serverless compute engine for containers, and you would incur costs for the vCPU and memory resources allocated to your container, which is unnecessary for serving static files.   
- [ ] Option C is wrong because: Deploying a web server (e.g., Apache, Nginx) on an Amazon EC2 instance to host a static website involves the cost of the EC2 instance running 24/7 (or on-demand), plus the operational overhead of managing the server (patching, security, updates).  This is less cost-effective and requires more management than S3 static website hosting.   
- [ ] Option D is wrong because: Configuring an Application Load Balancer with an AWS Lambda target using a framework like Express.js is designed for dynamic web applications that require server-side processing.  For a website with only static content, this architecture is unnecessarily complex, has higher latency for static file delivery compared to direct S3/CloudFront, and incurs higher costs (ALB charges, Lambda invocation costs).

</details>

<details>
  <summary>Question 33</summary>

A company runs an online marketplace web application on AWS. The application serves hundreds of thousands of users during peak hours. The company needs a scalable, near-real-time solution to share the details of millions of financial transactions with several other internal applications. Transactions also need to be processed to remove sensitive data before being stored in a document database for low-latency retrieval.

What should a solutions architect recommend to meet these requirements?

- [ ] A. Store the transactions data into Amazon DynamoDB. Set up a rule in DynamoDB to remove sensitive data from every transaction upon write. Use DynamoDB Streams to share the transactions data with other applications.
- [ ] B. Stream the transactions data into Amazon Kinesis Data Firehose to store data in Amazon DynamoDB and Amazon S3. Use AWS Lambda integration with Kinesis Data Firehose to remove sensitive data. Other applications can consume the data stored in Amazon S3.
- [ ] C. Stream the transactions data into Amazon Kinesis Data Streams. Use AWS Lambda integration to remove sensitive data from every transaction and then store the transactions data in Amazon DynamoDB. Other applications can consume the transactions data off the Kinesis data stream.
- [ ] D. Store the batched transactions data in Amazon S3 as files. Use AWS Lambda to process every file and remove sensitive data before updating the files in Amazon S3. The Lambda function then stores the data in Amazon DynamoDB. Other applications can consume transaction files stored in Amazon S3.

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn
Correct Answer: C

Why this is the correct answer:

- [ ] Scalable, Near-Real-Time Ingestion with Kinesis Data Streams: Amazon Kinesis Data Streams is designed for ingesting and processing large volumes of streaming data in near real-time. It can handle "millions of financial transactions" from a high-traffic application.
- [ ] Data Processing with Lambda: Kinesis Data Streams integrates seamlessly with AWS Lambda. A Lambda function can be configured to consume records from the stream, perform transformations like "remove sensitive data," and then pass the cleaned data to other services.
- [ ] Low-Latency Storage with DynamoDB: After processing, the Lambda function can store the sanitized transaction data in Amazon DynamoDB. DynamoDB is a NoSQL document database that provides "low-latency retrieval" and scales to handle high throughput, suitable for serving data to applications.
- [ ] Sharing with Multiple Applications: "Several other internal applications" can be configured as independent consumers of the Kinesis data stream. They can read and process the transaction data (either raw or after some initial processing by a primary Lambda) in parallel and at their own pace, providing a flexible and scalable way to share data.

Why are the other answers wrong?

- [ ] Option A is wrong because: DynamoDB itself does not have built-in "rules" to remove sensitive data upon write in the manner described. While you can use DynamoDB Streams to trigger a Lambda function after data is written to perform transformations, this means sensitive data would first be stored in DynamoDB before being processed. Kinesis Data Streams with Lambda allows processing before it hits the final document database.
- [ ] Option B is wrong because: Amazon Kinesis Data Firehose is primarily a data delivery service designed to load streaming data into destinations like S3, Redshift, or Amazon OpenSearch Service. While it supports data transformation via Lambda, it's less suited for providing a persistent stream that "several other internal applications" can consume in near real-time for varied purposes compared to Kinesis Data Streams. Consuming from S3 (as suggested for other applications) introduces batch latency rather than near-real-time stream processing.
- [ ] Option D is wrong because: Storing "batched transactions data in Amazon S3 as files" and then processing these files with Lambda is a batch processing approach, not a "scalable, near-real-time solution" suitable for handling millions of individual financial transactions that need to be shared quickly. This method introduces inherent latency due to batching and file processing.

</details>

<details>
  <summary>Question 34</summary>

A company hosts its multi-tier applications on AWS. For compliance, governance, auditing, and security, the company must track configuration changes on its AWS resources and record a history of API calls made to these resources.    

What should a solutions architect do to meet these requirements?

- [ ] A. Use AWS CloudTrail to track configuration changes and AWS Config to record API calls. 
- [ ] B. Use AWS Config to track configuration changes and AWS CloudTrail to record API calls. 
- [ ] C. Use AWS Config to track configuration changes and Amazon CloudWatch to record API calls. 
- [ ] D. Use AWS CloudTrail to track configuration changes and Amazon CloudWatch to record API calls.    

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use AWS Config to track configuration changes and AWS CloudTrail to record API calls. 

Why this is the correct answer:

- [ ] AWS Config for Tracking Configuration Changes: AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. It continuously monitors AWS resource configurations and maintains a history of configuration changes.
- [ ] This directly addresses the requirement to "track configuration changes on its AWS resources."    
- [ ] AWS CloudTrail for Recording API Call History: AWS CloudTrail records user activity and API calls made to AWS services.
- [ ] These logs provide a detailed history of actions taken in your AWS account, including who made the API call, when it was made, from what IP address, and what resources were affected.
- [ ] This directly addresses the requirement to "record a history of API calls made to these resources."    

Why are the other answers wrong?

- [ ] Option A is wrong because: This option reverses the primary roles of the two services. AWS CloudTrail is for recording API calls, and AWS Config is for tracking configuration changes.    
- [ ] Option C is wrong because: While Amazon CloudWatch is a monitoring and observability service and can store logs (including CloudTrail logs), it is not the primary service for recording a comprehensive history of API calls for auditing and governance purposes; that is the role of AWS CloudTrail.    
- [ ] Option D is wrong because: This option incorrectly assigns the roles. AWS CloudTrail tracks API calls, not primarily configuration changes (which is AWS Config's strength). Amazon CloudWatch is for monitoring and does not replace CloudTrail for API call history for audit purposes. 

</details>

<details>
  <summary>Question 35</summary>

A company is preparing to launch a public-facing web application in the AWS Cloud. The architecture consists of Amazon EC2 instances within a VPC behind an Elastic Load Balancer (ELB).  A third-party service is used for the DNS. The company's solutions architect must recommend a solution to detect and protect against large-scale DDoS attacks.    

Which solution meets these requirements?

- [ ] A. Enable Amazon GuardDuty on the account. 
- [ ] B. Enable Amazon Inspector on the EC2 instances. 
- [ ] C. Enable AWS Shield and assign Amazon Route 53 to it. 
- [ ] D. Enable AWS Shield Advanced and assign the ELB to it.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Enable AWS Shield Advanced and assign the ELB to it. 

Why this is the correct answer:

- [ ] AWS Shield Advanced for Enhanced DDoS Protection: AWS Shield Advanced offers a higher level of protection against larger and more sophisticated Distributed Denial of Service (DDoS) attacks compared to AWS Shield Standard (which provides default protection). For "large-scale DDoS attacks," Shield Advanced is the appropriate service.    
- [ ] Protecting the Elastic Load Balancer (ELB): The ELB is the entry point for traffic to the web application.  Protecting the ELB with Shield Advanced ensures that volumetric network layer attacks and other types of DDoS attacks targeting the application's frontend are mitigated.    
- [ ] Detection and Mitigation: Shield Advanced provides detection of DDoS attacks and includes automatic inline mitigations for many common infrastructure (Layer 3 and 4) attacks. For application layer (Layer 7) attacks, it integrates with AWS WAF (Web Application Firewall), allowing you to create custom rules for mitigation. Shield Advanced also provides access to the AWS DDoS Response Team (DRT) for expert assistance during an attack.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon GuardDuty is a threat detection service that monitors for malicious or unauthorized behavior within your AWS environment.  While it might detect activity related to a DDoS attack (e.g., if instances are compromised and participating in an attack, or if there are unusual traffic patterns), GuardDuty itself is not a DDoS mitigation service; it provides alerts and findings, but AWS Shield is designed for DDoS protection.   
- [ ] Option B is wrong because: Amazon Inspector is a vulnerability management service that scans your EC2 instances for software vulnerabilities and unintended network exposure.  It helps improve the security posture of your instances but does not provide real-time protection against incoming DDoS attacks.   
- [ ] Option C is wrong because: All AWS customers benefit from AWS Shield Standard protection automatically and at no additional cost. This option mentions enabling "AWS Shield," which likely refers to Shield Standard. While Route 53 can be a target for DNS-based DDoS attacks, and Shield Advanced can protect Route 53 hosted zones, the primary concern for application availability during a large-scale DDoS attack is typically the application endpoint, which in this architecture is the ELB.  Shield Advanced provides more robust protection than Shield Standard and should be associated directly with the application entry points like the ELB for comprehensive protection against attacks aimed at overwhelming the application servers. 

</details>

<details>
  <summary>Question 36</summary>

A company is building an application in the AWS Cloud. The application will store data in Amazon S3 buckets in two AWS Regions. The company must use an AWS Key Management Service (AWS KMS) customer managed key to encrypt all data that is stored in the S3 buckets. The data in both S3 buckets must be encrypted and decrypted with the same KMS key. The data and the key must be stored in each of the two Regions.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Create an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.
- [ ] B. Create a customer managed multi-Region KMS key. Create an S3 bucket in each Region. Configure replication between the S3 buckets. Configure the application to use the KMS key with client-side encryption.
- [ ] C. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.
- [ ] D. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with AWS KMS keys (SSE-KMS). Configure replication between the S3 buckets.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create a customer managed multi-Region KMS key. Create an S3 bucket in each Region. Configure replication between the S3 buckets. Configure the application to use the KMS key with client-side encryption.

Why this is the correct answer:

- [ ] Customer Managed Multi-Region KMS Key: The requirement is to use the same AWS KMS customer managed key for encrypting and decrypting data in S3 buckets across two AWS Regions. AWS KMS multi-Region keys are designed for this.
- [ ] A multi-Region key has the same key ID and key material in all specified Regions, allowing data encrypted in one Region to be decrypted in another using the same logical key.    
- [ ] Key and Data Stored in Each Region: Multi-Region keys are replicated and thus "stored" in each of the designated Regions.  S3 buckets are regional, so the data is stored in each respective Region.    
- [ ] Client-Side Encryption: To ensure the data is encrypted with this specific multi-Region KMS key before it's written to S3 in either Region, and to allow the application to use this same key for decryption regardless of the S3 bucket's region, client-side encryption is employed.
- [ ] The application uses the AWS KMS SDK to interact with the multi-Region KMS key for encryption (before upload) and decryption (after download).    
- [ ] Least Operational Overhead (from a key consistency perspective): While client-side encryption adds some complexity to the application code, the use of a multi-Region KMS key simplifies ensuring that the exact same encryption key (material and ID) is used across regions.
- [ ] This avoids the complexity of managing separate regional keys and trying to ensure interoperability or consistent encryption policies if using server-side encryption with regional keys. The operational overhead of creating and managing a multi-Region key itself is relatively low.

Why are the other answers wrong?

- [ ] Option A is wrong because: It specifies using server-side encryption with Amazon S3 managed encryption keys (SSE-S3).  The requirement is to use a "customer managed key." SSE-S3 does not meet this.   
- [ ] Option C is wrong because: This option is contradictory. It mentions creating a customer managed KMS key but then configuring the S3 buckets to use SSE-S3 (S3 managed keys).  These are different encryption methods. If a customer managed KMS key is to be used with server-side encryption, it would be SSE-KMS.   
- [ ] Option D is wrong because: If "a customer managed KMS key" refers to a standard, regional KMS key created independently in each Region, then these would be different keys, and data encrypted with one could not be decrypted by the other. This would not meet the requirement that "data in both S3 buckets must be encrypted and decrypted with the same KMS key."  While S3 replication with SSE-KMS can work, if the keys in each region are distinct regional keys, the "same key" criteria isn't met. If this option implied using a single multi-Region key with SSE-KMS, it could be viable; however, client-side encryption (Option B) gives the application more direct control to ensure the same key is used for the cryptographic operations across regions before data lands in S3. 

</details>

<details>
  <summary>Question 37</summary>
 
A company recently launched a variety of new workloads on Amazon EC2 instances in its AWS account. The company needs to create a strategy to access and administer the instances remotely and securely. The company needs to implement a repeatable process that works with native AWS services and follows the AWS Well-Architected Framework.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Use the EC2 serial console to directly access the terminal interface of each instance for administration.
- [ ] B. Attach the appropriate IAM role to each existing instance and new instance. Use AWS Systems Manager Session Manager to establish a remote SSH session.
- [ ] C. Create an administrative SSH key pair. Load the public key into each EC2 instance. Deploy a bastion host in a public subnet to provide a tunnel for administration of each instance.
- [ ] D. Establish an AWS Site-to-Site VPN connection. Instruct administrators to use their local on-premises machines to connect directly to the instances by using SSH keys across the VPN tunnel.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Attach the appropriate IAM role to each existing instance and new instance. Use AWS Systems Manager Session Manager to establish a remote SSH session.

Why this is the correct answer:

- [ ] AWS Systems Manager Session Manager: Session Manager is a fully managed AWS service that provides secure and auditable instance management without the need to open inbound SSH (port 22) or RDP (port 3389) ports, manage bastion hosts, or maintain SSH keys. This significantly enhances security and reduces operational overhead.
- [ ] IAM Role for Access Control: Access to instances via Session Manager is controlled through AWS Identity and Access Management (IAM) roles and policies. This allows for granular permissions, adherence to the principle of least privilege, and a repeatable process for granting access. This aligns well with the AWS Well-Architected Framework's security pillar.
- [ ] Native AWS Service and Low Overhead: Session Manager is a native AWS service, ensuring good integration and support. It eliminates the overhead associated with patching and maintaining bastion hosts, managing SSH key pairs, or configuring VPNs solely for instance administration. Commands and session activity can also be logged to services like S3 or CloudWatch Logs for auditing.
- [ ] Secure Remote Sessions: Session Manager allows users to start secure shell (SSH) or PowerShell sessions to instances directly from the AWS Management Console, AWS CLI, or via the AWS SDK.

Why are the other answers wrong?

- [ ] Option A is wrong because: The EC2 serial console is primarily a troubleshooting tool for instances that are unreachable via normal SSH or RDP (e.g., due to boot issues or network misconfigurations). It is not intended or suitable as a primary method for routine remote administration of a fleet of instances. It offers limited functionality compared to a full shell session.
- [ ] Option C is wrong because: Managing SSH key pairs (distribution, rotation, revocation) and deploying, maintaining, and securing bastion hosts introduces significant operational overhead and potential security risks. Bastion hosts themselves need to be patched and monitored, and SSH keys can be compromised. Session Manager (Option B) avoids these complexities.
- [ ] Option D is wrong because: While an AWS Site-to-Site VPN provides secure connectivity between an on-premises network and a VPC, it is primarily for network extension. Relying on it for instance administration still requires managing SSH keys for individual instances. Session Manager offers a more direct, instance-level access control method that doesn't depend on network-level VPNs and enhances security by not requiring open SSH ports on the instances themselves or direct SSH key management for session access. It is generally a more streamlined solution for secure instance access within AWS.

</details>

<details>
  <summary>Question 38</summary>

A company is hosting a static website on Amazon S3 and is using Amazon Route 53 for DNS. The website is experiencing increased demand from around the world. The company must decrease latency for users who access the website.

Which solution meets these requirements MOST cost-effectively?

- [ ] A. Replicate the S3 bucket that contains the website to all AWS Regions. Add Route 53 geolocation routing entries.
- [ ] B. Provision accelerators in AWS Global Accelerator. Associate the supplied IP addresses with the S3 bucket. Edit the Route 53 entries to point to the IP addresses of the accelerators.
- [ ] C. Add an Amazon CloudFront distribution in front of the S3 bucket. Edit the Route 53 entries to point to the CloudFront distribution.
- [ ] D. Enable S3 Transfer Acceleration on the bucket. Edit the Route 53 entries to point to the new endpoint.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Add an Amazon CloudFront distribution in front of the S3 bucket. Edit the Route 53 entries to point to the CloudFront distribution.

Why this is the correct answer:

- [ ] Amazon CloudFront for Low-Latency Content Delivery: Amazon CloudFront is AWS's content delivery network (CDN).
- [ ] It caches copies of your static website content (from the S3 origin) at edge locations distributed globally.
- [ ] When users request content, they are served from the nearest edge location, which significantly "decreases latency."
- [ ] Integration with S3 and Route 53: CloudFront is designed to work seamlessly with Amazon S3 as an origin for static websites.
- [ ] After setting up the CloudFront distribution, you update your Amazon Route 53 DNS record for your website's domain to point to the CloudFront distribution's domain name.
- [ ] MOST Cost-Effective for Static Websites: For delivering static website content globally with low latency, CloudFront is generally the most cost-effective solution. It reduces the load on your S3 origin and optimizes data transfer costs by serving content from edge locations. The pricing model is typically well-suited for website delivery.

Why are the other answers wrong?

- [ ] Option A is wrong because: Replicating an S3 bucket to all AWS Regions using S3 Cross-Region Replication (CRR) would incur significant storage costs (paying for storage in every region) and data transfer costs for replication. Managing this setup would also have higher operational overhead. While Route 53 geolocation routing could direct users to a nearby regional bucket, CloudFront provides a more extensive network of edge locations and is generally more cost-effective and simpler for caching and delivering static content.
- [ ] Option B is wrong because: AWS Global Accelerator is designed to improve the performance and availability of global applications by providing static IP addresses and routing user traffic over the AWS global network to optimal regional endpoints. While it can reduce latency for applications, CloudFront is more specifically tailored and usually more cost-effective for caching and delivering static website content from S3. Global Accelerator does not provide the same level of edge caching as CloudFront, which is key for static content latency reduction.
- [ ] Option D is wrong because: S3 Transfer Acceleration is primarily designed to speed up uploads of data to Amazon S3 over long distances by routing traffic through Amazon CloudFront's edge locations. It does not provide the same benefits for downloading or serving static website content with caching at edge locations to global users as a full CloudFront distribution does. For reducing latency for users accessing a website, a CloudFront distribution is the appropriate service.

</details>

<details>
  <summary>Question 39</summary>

A company maintains a searchable repository of items on its website. The data is stored in an Amazon RDS for MySQL database table that contains more than 10 million rows. The database has 2 TB of General Purpose SSD storage. There are millions of updates against this data every day through the company's website. The company has noticed that some insert operations are taking 10 seconds or longer. The company has determined that the database storage performance is the problem.

Which solution addresses this performance issue?

- [ ] A. Change the storage type to Provisioned IOPS SSD.
- [ ] B. Change the DB instance to a memory optimized instance class.
- [ ] C. Change the DB instance to a burstable performance instance class.
- [ ] D. Enable Multi-AZ RDS read replicas with MySQL native asynchronous replication.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Change the storage type to Provisioned IOPS SSD.

Why this is the correct answer:

- [ ] Identified Bottleneck is Storage Performance: The question explicitly states, "The company has determined that the database storage performance is the problem."
- [ ] General Purpose SSD vs. Provisioned IOPS SSD:
- [ ] Amazon RDS General Purpose SSD (gp2 and gp3) volumes deliver a baseline IOPS performance that scales with storage size and offer the ability to burst IOPS.
- [ ] For a database with "millions of updates every day" and slow insert operations (10 seconds or longer), it's likely that the current General Purpose SSD storage is hitting its IOPS or throughput limits.
- [ ] Provisioned IOPS SSD (io1, io2, io2 Block Express) volumes are designed for critical, I/O-intensive database workloads that require sustained high performance and low latency.
- [ ] You can specify a consistent amount of IOPS when you create the volume. Changing to Provisioned IOPS SSD directly targets the identified storage performance bottleneck by providing a higher and more consistent level of I/O operations per second.
- [ ] Addresses Slow Inserts: Slow insert operations are a common symptom of storage I/O contention. Upgrading the storage to a higher-performance tier like Provisioned IOPS SSD will provide the necessary throughput to handle the millions of daily updates more efficiently.

Why are the other answers wrong?

- [ ] Option B is wrong because: While increasing memory by changing to a memory-optimized instance class can improve database performance by allowing more data to be cached, the problem specifically states that "database storage performance is the problem." If the bottleneck is the disk's ability to handle I/O operations, adding more memory alone will not resolve the slow insert times caused by I/O limitations.
- [ ] Option C is wrong because: Burstable performance instance classes (like db.t3 or db.t4g for RDS) provide a baseline CPU performance with the ability to burst. The identified issue is with storage performance, not CPU. Moreover, a database handling "millions of updates every day" is likely to require sustained performance that would quickly deplete CPU credits on a burstable instance, leading to further performance degradation.
- [ ] Option D is wrong because: Multi-AZ deployments for RDS provide high availability and data durability by creating a standby replica. Read replicas are used to offload read traffic from the primary database instance. While read replicas can improve overall application performance by handling read queries, they do not directly improve the performance of write operations (like inserts) on the primary instance. Since the issue is with slow insert operations and the bottleneck is storage performance on the primary, this solution doesn't address the root cause.

</details>

<details>
  <summary>Question 40</summary>

A company has thousands of edge devices that collectively generate 1 TB of status alerts each day. Each alert is approximately 2 KB in size. A solutions architect needs to implement a solution to ingest and store the alerts for future analysis. The company wants a highly available solution. However, the company needs to minimize costs and does not want to manage additional infrastructure. Additionally, the company wants to keep 14 days of data available for immediate analysis and archive any data older than 14 days.   

What is the MOST operationally efficient solution that meets these requirements?

- [ ] A. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days. 
- [ ] B. Launch Amazon EC2 instances across two Availability Zones and place them behind an Elastic Load Balancer to ingest the alerts. Create a script on the EC2 instances that will store the alerts in an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days. 
- [ ] C. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster. Set up the Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster to take manual snapshots every day and delete data from the cluster that is older than 14 days. 
- [ ] D. Create an Amazon Simple Queue Service (Amazon SQS) standard queue to ingest the alerts, and set the message retention period to 14 days. Configure consumers to poll the SQS queue, check the age of the message, and analyze the message data as needed. If the message is 14 days old, the consumer should copy the message to an Amazon S3 bucket and delete the message from the SQS queue.    

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days. 

Why this is the correct answer:

- [ ] Managed Ingestion with Kinesis Data Firehose: Amazon Kinesis Data Firehose is a fully managed service for capturing, transforming, and loading streaming data. It can easily handle the 1 TB of alerts generated daily without requiring the company to manage any underlying infrastructure, thus meeting the "does not want to manage additional infrastructure" and "MOST operationally efficient" requirements.  It is also highly available.   
- [ ] Cost-Effective Storage with S3: Firehose can deliver the incoming alerts directly to an Amazon S3 bucket. S3 provides durable, scalable, and cost-effective storage for the alerts. Data stored in S3 for the initial 14 days can be easily accessed for "immediate analysis" using query services like Amazon Athena.   
- [ ] Automated Archival with S3 Lifecycle: An S3 Lifecycle configuration can be set up to automatically transition objects older than 14 days from the S3 standard storage class to a cheaper archival storage class like Amazon S3 Glacier (or S3 Glacier Deep Archive for even lower costs if retrieval times are flexible). This addresses the archival requirement and helps "minimize costs."   
- [ ] Overall Efficiency: This solution uses serverless and managed services, which greatly reduces operational burden.

Why are the other answers wrong?

- [ ] Option B is wrong because: This solution involves managing EC2 instances, an Elastic Load Balancer, and custom scripts for ingestion. This directly contradicts the requirement to "not want to manage additional infrastructure" and is less operationally efficient than a managed service like Kinesis Data Firehose.   
- [ ] Option C is wrong because: While Amazon OpenSearch Service is suitable for analyzing log data, managing an OpenSearch cluster (even a managed one) generally incurs higher costs and more operational overhead compared to storing data in S3 and querying it with Athena for this use case, especially when cost minimization is a goal. The process of taking manual snapshots and manually deleting old data from OpenSearch is also less operationally efficient than an automated S3 Lifecycle policy.   
- [ ] Option D is wrong because: Using Amazon SQS with a 14-day message retention period  and then relying on consumers to manually check message age, analyze, and then copy to S3 for archival is a complex and potentially error-prone process. This solution is not as operationally efficient or robust for ingesting and archiving 1 TB of data daily compared to using Kinesis Data Firehose for direct S3 delivery and S3 Lifecycle policies for archival. It also places a significant burden on the consumer logic.

</details>


</details>


<details>
  <summary>==Questions 41-50==</summary>

<details>
  <summary>Question 41</summary>
  
A company's application integrates with multiple software-as-a-service (SaaS) sources for data collection.  The company runs Amazon EC2 instances to receive the data and to upload the data to an Amazon S3 bucket for analysis.  The same EC2 instance that receives and uploads the data also sends a notification to the user when an upload is complete.  The company has noticed slow application performance and wants to improve the performance as much as possible.    

Which solution will meet these requirements with the LEAST operational overhead?    

- [ ] A. Create an Auto Scaling group so that EC2 instances can scale out.  Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete. 
- [ ] B. Create an Amazon AppFlow flow to transfer data between each SaaS source and the S3 bucket.  Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete. 
- [ ] C. Create an Amazon EventBridge (Amazon CloudWatch Events) rule for each SaaS source to send output data.  Configure the S3 bucket as the rule's target. Create a second EventBridge (Cloud Watch Events) rule to send events when the upload to the S3 bucket is complete.  Configure an Amazon Simple Notification Service (Amazon SNS) topic as the second rule's target. 
- [ ] D. Create a Docker container to use instead of an EC2 instance.  Host the containerized application on Amazon Elastic Container Service (Amazon ECS).  Configure Amazon CloudWatch Container Insights to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete.    

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create an Amazon AppFlow flow to transfer data between each SaaS source and the S3 bucket.  Configure an S3 event notification to send events to an Amazon Simple Notification Service (Amazon SNS) topic when the upload to the S3 bucket is complete. 

Why this is the correct answer:

- [ ] Amazon AppFlow for SaaS Data Integration: Amazon AppFlow is a fully managed integration service designed to securely transfer data between SaaS applications (like Salesforce, Marketo, Zendesk, etc.) and AWS services such as Amazon S3.  Using AppFlow automates the data collection and transfer from multiple SaaS sources to the S3 bucket, removing the need for the custom EC2 instances to perform this task. This directly addresses the performance issues related to the EC2 instances and significantly reduces operational overhead by replacing custom code and infrastructure with a managed service.   
- [ ] Decoupled Notifications with S3 Events and SNS: The solution also leverages S3 event notifications to trigger an Amazon SNS topic when data uploads to S3 are complete.  This decouples the notification process from the data ingestion process, which is a best practice. It ensures that notifications are handled efficiently and reliably without impacting the data transfer.   
- [ ] Improved Performance and Least Operational Overhead: By offloading the data transfer to the managed AppFlow service and handling notifications asynchronously through S3 events and SNS, the existing EC2 instances' responsibilities are greatly reduced or eliminated. This leads to improved performance (as AppFlow is optimized for these transfers) and the "LEAST operational overhead" since you're no longer managing custom data collection scripts on EC2 instances.    

Why are the other answers wrong?

- [ ] Option A is wrong because: While adding Auto Scaling to the EC2 instances might help handle more load, it doesn't fundamentally solve the inefficiency or potential bottlenecks in the custom data collection and notification logic running on those instances.  It still retains the higher operational overhead of managing EC2 instances for this task compared to a managed service like AppFlow. The notification part is good, but the data collection is not optimized for operational overhead.    
- [ ] Option C is wrong because: Using Amazon EventBridge to have SaaS sources directly send data to S3 might not be feasible or directly supported by many SaaS applications, which often rely on API-based integration or specific connectors that AppFlow provides.  AppFlow is purpose-built for these SaaS integrations. While EventBridge is excellent for event-driven architectures and can react to S3 events for notifications, its role in directly pulling from diverse SaaS sources is limited compared to AppFlow.   
- [ ] Option D is wrong because: Migrating the application to Docker containers on Amazon ECS addresses how the application is deployed and managed but doesn't necessarily change the underlying custom logic for data collection from SaaS sources.  It might not reduce the operational overhead related to the data integration itself as much as AppFlow would. Using CloudWatch Container Insights for notifications based on S3 uploads is also less direct than using S3 event notifications.  

</details>  

<details>
  <summary>Question 42</summary>

A company runs a highly available image-processing application on Amazon EC2 instances in a single VPC. The EC2 instances run inside several subnets across multiple Availability Zones. The EC2 instances do not communicate with each other. However, the EC2 instances download images from Amazon S3 and upload images to Amazon S3 through a single NAT gateway. The company is concerned about data transfer charges.

What is the MOST cost-effective way for the company to avoid Regional data transfer charges?    

- [ ] A. Launch the NAT gateway in each Availability Zone. 
- [ ] B. Replace the NAT gateway with a NAT instance. 
- [ ] C. Deploy a gateway VPC endpoint for Amazon S3. 
- [ ] D. Provision an EC2 Dedicated Host to run the EC2 instances.    

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Deploy a gateway VPC endpoint for Amazon S3. 

Why this is the correct answer:

- [ ] Avoiding NAT Gateway Charges for S3 Traffic: When EC2 instances in a private subnet access Amazon S3 through a NAT gateway, you incur charges for the data processed by the NAT gateway.    
- [ ] Gateway VPC Endpoint for S3: A gateway VPC endpoint provides a private connection from your VPC to Amazon S3. Traffic between your EC2 instances and S3 routes through the endpoint within the AWS network, without needing to go through a NAT gateway or an internet gateway.    
- [ ] Cost-Effectiveness: There are no hourly charges for using gateway VPC endpoints. More importantly, data transferred between EC2 instances and Amazon S3 through a gateway endpoint within the same AWS Region does not incur data transfer charges or NAT gateway processing charges.
- [ ] This directly addresses the company's concern about data transfer charges and is the most cost-effective solution for this scenario.   

Why are the other answers wrong?

- [ ] Option A is wrong because: Launching a NAT gateway in each Availability Zone is a strategy for high availability of internet access from private subnets. However, it would increase costs because you pay an hourly charge for each NAT gateway and for the data it processes. It does not avoid the data processing charges associated with accessing S3 through a NAT gateway.    
- [ ] Option B is wrong because: Replacing a managed NAT gateway with a NAT instance (an EC2 instance configured to perform NAT) might change the cost structure (paying for EC2 instance hours instead of NAT gateway hours). However, data transferred to S3 via a NAT instance would still effectively be routed out and back in, and you'd still have data transfer costs associated with the EC2 instance. It also increases operational overhead compared to a managed NAT gateway and does not eliminate S3 access costs as effectively as a gateway endpoint.
- [ ] Option D is wrong because: Provisioning an EC2 Dedicated Host is about having dedicated physical servers for your EC2 instances, typically for software licensing compliance or specific performance/isolation needs. It has no bearing on how data is transferred to or from Amazon S3 or the associated data transfer charges. 

</details>

<details>
  <summary>Question 43</summary>

A company has an on-premises application that generates a large amount of time-sensitive data that is backed up to Amazon S3.  The application has grown and there are user complaints about internet bandwidth limitations.  A solutions architect needs to design a long-term solution that allows for both timely backups to Amazon S3 and with minimal impact on internet connectivity for internal users.    

Which solution meets these requirements?

- [ ] A. Establish AWS VPN connections and proxy all traffic through a VPC gateway endpoint. 
- [ ] B. Establish a new AWS Direct Connect connection and direct backup traffic through this new connection. 
- [ ] C. Order daily AWS Snowball devices. Load the data onto the Snowball devices and return the devices to AWS each day. 
- [ ] D. Submit a support ticket through the AWS Management Console. Request the removal of S3 service limits from the account.    

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Establish a new AWS Direct Connect connection and direct backup traffic through this new connection.  

Why this is the correct answer:

- [ ] AWS Direct Connect for Dedicated Bandwidth: AWS Direct Connect establishes a dedicated, private network connection from your on-premises data center to AWS.  This connection does not traverse the public internet.   
- [ ] Minimal Impact on Existing Internet: By directing the large volume of backup traffic to Amazon S3 over the Direct Connect link, the company's existing internet connection is freed up.  This addresses the "user complaints about internet bandwidth limitations" by separating backup traffic from general internet usage.    
- [ ] Timely and Consistent Backups: Direct Connect provides high bandwidth and consistent network performance, which is crucial for "timely backups" of "large amount of time-sensitive data."    
- [ ] Long-Term Solution: Establishing a Direct Connect connection is a robust "long-term solution" for persistent, high-bandwidth requirements between an on-premises environment and AWS.    

Why are the other answers wrong?

- [ ] Option A is wrong because: An AWS VPN connection, while secure, typically routes traffic over the existing public internet connection.  This would still consume the company's internet bandwidth and would not alleviate the "internet bandwidth limitations" for other users.  A VPC gateway endpoint for S3 facilitates private access to S3 from within a VPC, not directly from an on-premises location over a VPN in a way that solves the internet bandwidth contention.    
- [ ] Option C is wrong because: Ordering AWS Snowball devices daily for backing up time-sensitive data is operationally very cumbersome and not suitable as a "long-term solution" for regular backups.  Snowball is better suited for large, one-time data migrations or for environments with extremely limited or no network connectivity.  The daily logistics would be a significant burden.   
- [ ] Option D is wrong because: The problem explicitly states "internet bandwidth limitations" as the issue.  S3 service limits (e.g., on request rates or bucket numbers) are generally very high and are unlikely to be the cause of slow backups when internet bandwidth is constrained. Requesting an increase in S3 service limits would not address the core problem of insufficient network capacity on the company's internet connection. 

</details>

<details>
  <summary>Question 44</summary>

A company has an Amazon S3 bucket that contains critical data. The company must protect the data from accidental deletion.    

Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- [ ] A. Enable versioning on the S3 bucket. 
- [ ] B. Enable MFA Delete on the S3 bucket. 
- [ ] C. Create a bucket policy on the S3 bucket.
- [ ] D. Enable default encryption on the S3 bucket. 
- [ ] E. Create a lifecycle policy for the objects in the S3 bucket.    

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Enable versioning on the S3 bucket. 
- [ ] B. Enable MFA Delete on the S3 bucket. 

Why these are the correct answers:

A. Enable versioning on the S3 bucket.
- [ ] Protection Against Accidental Deletion/Overwrite: S3 Versioning keeps multiple versions of an object in the same bucket. When an object is "deleted" in a versioning-enabled bucket, Amazon S3 inserts a delete marker for that object instead of permanently removing it.
- [ ] This delete marker becomes the current version. If an object is overwritten, the new version becomes the current version, and the previous version is retained.
- [ ] This allows you to easily recover from accidental deletions (by deleting the delete marker) or accidental overwrites (by restoring a previous version).    

B. Enable MFA Delete on the S3 bucket.
- [ ] Enhanced Protection for Deletion Operations: MFA (Multi-Factor Authentication) Delete adds an extra layer of security specifically for deletion operations in a versioning-enabled bucket.
- [ ] When MFA Delete is enabled, certain operations, such as permanently deleting an object version or changing the versioning state of the bucket, require the bucket owner to provide their standard AWS credentials plus a valid code from their MFA device.
- [ ] This significantly reduces the risk of accidental deletions, even by users with delete permissions, as it requires an additional, out-of-band authentication step.    

Why are the other answers wrong?

- [ ] C. Create a bucket policy on the S3 bucket.
While a bucket policy is crucial for controlling access and can be used to deny delete permissions to certain users or roles, it doesn't inherently protect against accidental deletion by an authorized user who makes a mistake. Versioning and MFA Delete provide mechanisms to recover from or add an extra check to the deletion action itself. A bucket policy is about preventing unauthorized actions, while versioning/MFA Delete are about mitigating the impact of (potentially authorized but accidental) actions.
- [ ] D. Enable default encryption on the S3 bucket.
Default encryption ensures that all new objects stored in the S3 bucket are encrypted at rest. This protects the confidentiality of the data from unauthorized access to the underlying storage. However, encryption does not prevent objects from being deleted. It's a data protection measure for confidentiality, not for preventing accidental deletion.    
- [ ] E. Create a lifecycle policy for the objects in the S3 bucket.
S3 Lifecycle policies are used to manage the lifetime of objects in a bucket by automating their transition to different storage classes or their expiration (deletion). While useful for cost optimization and data retention management, a misconfigured lifecycle policy could inadvertently cause data to be deleted. It is not primarily a mechanism to protect against accidental, ad-hoc deletions by users.

</details>

<details>
  <summary>Question 45</summary>
  
A company has a data ingestion workflow that consists of the following:

An Amazon Simple Notification Service (Amazon SNS) topic for notifications about new data deliveries
An AWS Lambda function to process the data and record metadata
The company observes that the ingestion workflow fails occasionally because of network connectivity issues. When such a failure occurs, the Lambda function does not ingest the corresponding data unless the company manually reruns the job.

Which combination of actions should a solutions architect take to ensure that the Lambda function ingests all data in the future? (Choose two.)

- [ ] A. Deploy the Lambda function in multiple Availability Zones.
- [ ] B. Create an Amazon Simple Queue Service (Amazon SQS) queue, and subscribe it to the SNS topic.
- [ ] C. Increase the CPU and memory that are allocated to the Lambda function.
- [ ] D. Increase provisioned throughput for the Lambda function.
- [ ] E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create an Amazon Simple Queue Service (Amazon SQS) queue, and subscribe it to the SNS topic.
- [ ] E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue.

Why these are the correct answers:

This combination creates a more resilient and reliable data ingestion workflow:

B. Create an Amazon Simple Queue Service (Amazon SQS) queue, and subscribe it to the SNS topic.
- [ ] Decoupling and Durability: SNS is great for fanning out messages, but if a direct Lambda subscriber fails transiently (due to "network connectivity issues"), the message might be lost or retries exhausted without successful processing.
- [ ] By inserting an SQS queue between SNS and Lambda, the messages sent by SNS are first delivered to the SQS queue. SQS provides durable message storage, acting as a buffer.
- [ ] If the Lambda function is temporarily unable to process, the messages remain safely in the queue.

E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue.
- [ ] Reliable Processing and Retries: When the Lambda function is configured to use the SQS queue as its event source, it will poll the queue for messages.
- [ ] If the Lambda function fails to process a message (e.g., due to a temporary network issue when trying to record metadata), the message can remain in the SQS queue (based on its visibility timeout) and be attempted again.
- [ ] SQS also supports configuring a Dead-Letter Queue (DLQ) to send messages that consistently fail processing after a certain number of retries, allowing for investigation without losing the message.
- [ ] This setup ensures that messages are not lost due to transient failures and that the Lambda function has a better chance to "ingest all data."

Why are the other answers wrong?

- [ ] Option A is wrong because: AWS Lambda functions automatically run in a highly available environment managed by AWS, which spans multiple Availability Zones. You do not manually deploy Lambda functions to specific AZs in the way you might for EC2 instances. The problem described is about handling transient processing failures and message loss, not about the inherent availability of the Lambda service itself.
- [ ] Option C is wrong because: While increasing CPU or memory might help if the Lambda function was failing due to resource exhaustion during its processing, the problem states the failures are "because of network connectivity issues." Simply increasing Lambda resources won't solve external network problems or ensure that a message that failed to process due to such an issue is reliably retried and eventually processed.
- [ ] Option D is wrong because: The term "provisioned throughput" is not directly applicable to AWS Lambda in the context of ensuring message ingestion from SNS or SQS. Lambda scales its execution based on the number of incoming events (or messages polled from a queue). While you can set "Provisioned Concurrency" for Lambda to keep a certain number of execution environments warm and reduce cold starts, it doesn't address the core issue of message durability and retries when failures occur due to external factors like network connectivity. The problem is about not losing data when a single invocation fails.

</details>

<details>
  <summary>Question 46</summary>

A company has an application that provides marketing services to stores. The services are based on previous purchases by store customers. The stores upload transaction data to the company through SFTP, and the data is processed and analyzed to generate new marketing offers. Some of the files can exceed 200 GB in size.

Recently, the company discovered that some of the stores have uploaded files that contain personally identifiable information (PII) that should not have been included. The company wants administrators to be alerted if PII is shared again. The company also wants to automate remediation.

What should a solutions architect do to meet these requirements with the LEAST development effort?

- [ ] A. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Inspector to scan the objects in the bucket. If objects contain PII, trigger an S3 Lifecycle policy to remove the objects that contain PII.
- [ ] B. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Macie to scan the objects in the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.
- [ ] C. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.
- [ ] D. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Email Service (Amazon SES) to trigger a notification to the administrators and trigger an S3 Lifecycle policy to remove the meats that contain PII.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Macie to scan the objects in the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.

Why this is the correct answer:

- [ ] Amazon S3 as Secure Transfer Point: Using an S3 bucket as the destination for SFTP uploads is a common and secure approach. (Often AWS Transfer Family is used for the SFTP frontend to S3).
- [ ] Amazon Macie for PII Detection: Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect sensitive data, such as PII, in Amazon S3. This directly addresses the need to identify files containing PII with the "LEAST development effort," as Macie provides this capability out-of-the-box.   
- [ ] Alerting with Amazon SNS: When Macie detects PII, it generates findings. These findings can be published to Amazon EventBridge, which can then trigger an Amazon SNS topic. Subscribing administrators to this SNS topic ensures they are alerted when PII is found.
- [ ] Path to Automated Remediation: While this option focuses on alerting administrators for removal, Macie's findings (via EventBridge) can also be used to trigger AWS Lambda functions for automated remediation actions (e.g., quarantining or deleting the object). This provides a pathway to the "automate remediation" requirement with further configuration, leveraging the initial PII discovery by Macie.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Inspector is a service that assesses Amazon EC2 instances for vulnerabilities and deviations from security best practices. It does not scan objects within S3 buckets for PII content.
- [ ] Option C is wrong because: Implementing custom scanning algorithms in an AWS Lambda function to detect PII would require significant development effort, including defining PII patterns, writing the scanning logic, and maintaining it. Amazon Macie offers this as a managed service, which aligns with the "LEAST development effort" requirement.
- [ ] Option D is wrong because: Similar to option C, this relies on custom scanning algorithms in Lambda, which means high development effort. While SES can send emails, SNS is typically more appropriate for system-generated alerts to administrators. The core issue is the custom development for PII scanning. (The typo "meats" likely means "objects").

</details>


<details>
  <summary>Question 47</summary>

A company needs guaranteed Amazon EC2 capacity in three specific Availability Zones in a specific AWS Region for an upcoming event that will last 1 week.

What should the company do to guarantee the EC2 capacity?

- [ ] A. Purchase Reserved Instances that specify the Region needed.
- [ ] B. Create an On-Demand Capacity Reservation that specifies the Region needed.
- [ ] C. Purchase Reserved Instances that specify the Region and three Availability Zones needed.
- [ ] D. Create an On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create an On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed.

Why this is the correct answer:

- [ ] On-Demand Capacity Reservations: This AWS service allows you to reserve compute capacity for your Amazon EC2 instances in a specific Availability Zone (AZ) for any duration. This ensures that you can launch the required number of instances of a specific type when you need them.
- [ ] Specificity for Availability Zones: The requirement is for guaranteed capacity in "three specific Availability Zones." On-Demand Capacity Reservations are created for a specific instance type within a specific AZ. The company would create separate capacity reservations for each of the three required AZs.
- [ ] Guaranteed Capacity for Short Duration: For an event lasting "1 week," On-Demand Capacity Reservations are ideal because they provide the capacity assurance without requiring a long-term commitment (unlike Reserved Instances). You can create them for the duration needed and then cancel them.
- [ ] Meets Requirements Directly: This option directly addresses the need for guaranteed capacity in specific AZs for a defined short-term period.

Why are the other answers wrong?

- [ ] Option A is wrong because: Regional Reserved Instances provide a billing discount and, for some types, can offer capacity benefits, but they do not guarantee capacity in specific Availability Zones. The capacity benefit of regional RIs is applied more broadly across AZs in a region. The requirement here is for guaranteed capacity in three specific AZs.
- [ ] Option B is wrong because: While creating an On-Demand Capacity Reservation is the correct service, it must be specified for each particular Availability Zone where the capacity is needed. A capacity reservation that only "specifies the Region needed" without specifying the AZs would not guarantee capacity in the "three specific Availability Zones" required.
- [ ] Option C is wrong because: Purchasing Reserved Instances (RIs) involves a 1-year or 3-year commitment for a billing discount. While Zonal RIs (Reserved Instances purchased for a specific Availability Zone) do provide a capacity reservation, it is not the most flexible or cost-effective approach for guaranteeing capacity for only a 1-week event due to the long commitment term. On-Demand Capacity Reservations are better suited for short-term capacity assurance without the long-term financial commitment of RIs.

</details>

<details>
  <summary>Question 48</summary>

A company's website uses an Amazon EC2 instance store for its catalog of items. The company wants to make sure that the catalog is highly available and that the catalog is stored in a durable location.    

What should a solutions architect do to meet these requirements?

- [ ] A. Move the catalog to Amazon ElastiCache for Redis. 
- [ ] B. Deploy a larger EC2 instance with a larger instance store. 
- [ ] C. Move the catalog from the instance store to Amazon S3 Glacier Deep Archive. 
- [ ] D. Move the catalog to an Amazon Elastic File System (Amazon EFS) file system.    

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Move the catalog to an Amazon Elastic File System (Amazon EFS) file system.      

Why this is the correct answer:

- [ ] Durability of Instance Stores: EC2 instance stores provide temporary, block-level storage. The data stored on an instance store volume is lost if the EC2 instance is stopped, hibernated, or terminated. This makes it unsuitable for storing a catalog that needs to be in a "durable location."    
- [ ] Amazon EFS for Durability and Availability: Amazon Elastic File System (EFS) offers scalable, elastic file storage that is designed for high availability and durability. Data in an EFS file system is stored redundantly across multiple Availability Zones (AZs) within an AWS Region.
- [ ] This makes it both highly available (as it can withstand an AZ failure) and durable.   
- [ ] Shared Access: EFS file systems can be mounted and accessed by multiple EC2 instances concurrently, which can be beneficial if the website scales to use more than one instance.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon ElastiCache for Redis is an in-memory caching service. While it can significantly improve the performance of accessing frequently requested data, it is not primarily designed as a durable, persistent storage solution for a catalog. Data in a cache can be lost if a cache node fails (though Redis can be configured with persistence, EFS is a more direct solution for durable file storage).    
- [ ] Option B is wrong because: Deploying a larger EC2 instance with a larger instance store does not change the fundamental characteristic that instance store volumes are ephemeral. The catalog data would still be at risk of loss if the instance fails or is terminated.    
- [ ] Option C is wrong because: Amazon S3 Glacier Deep Archive is an extremely low-cost storage class intended for long-term data archiving and digital preservation, where data is rarely accessed, and retrieval times of several hours are acceptable. It is not suitable for hosting an active website catalog that requires frequent and low-latency access.

</details>

<details>
  <summary>Question 49</summary>

A company stores call transcript files on a monthly basis. Users access the files randomly within 1 year of the call, but users access the files infrequently after 1 year. The company wants to optimize its solution by giving users the ability to query and retrieve files that are less than 1-year-old as quickly as possible. A delay in retrieving older files is acceptable.

Which solution will meet these requirements MOST cost-effectively?

- [ ] A. Store individual files with tags in Amazon S3 Glacier Instant Retrieval. Query the tags to retrieve the files from S3 Glacier Instant Retrieval.
- [ ] B. Store individual files in Amazon S3 Intelligent-Tiering. Use S3 Lifecycle policies to move the files to S3 Glacier Flexible Retrieval after 1 year. Query and retrieve the files that are in Amazon S3 by using Amazon Athena. Query and retrieve the files that are in S3 Glacier by using S3 Glacier Select.
- [ ] C. Store individual files with tags in Amazon S3 Standard storage. Store search metadata for each archive in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Instant Retrieval after 1 year. Query and retrieve the files by searching for metadata from Amazon S3.
- [ ] D. Store individual files in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Deep Archive after 1 year. Store search metadata in Amazon RDS. Query the files from Amazon RDS. Retrieve the files from S3 Glacier Deep Archive.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Store individual files in Amazon S3 Intelligent-Tiering. Use S3 Lifecycle policies to move the files to S3 Glacier Flexible Retrieval after 1 year. Query and retrieve the files that are in Amazon S3 by using Amazon Athena. Query and retrieve the files that are in S3 Glacier by using S3 Glacier Select.

Why this is the correct answer:

- [ ] S3 Intelligent-Tiering for Recent Files (< 1 year): For files accessed "randomly within 1 year," S3 Intelligent-Tiering is ideal. It automatically moves data between a frequent access tier (for quick retrieval) and an infrequent access tier to optimize costs based on actual access patterns, without performance impact or retrieval fees between these tiers. This ensures files less than 1 year old can be retrieved "as quickly as possible."
- [ ] Lifecycle to S3 Glacier Flexible Retrieval for Older Files (> 1 year): After 1 year, when files are "accessed infrequently" and "a delay in retrieving older files is acceptable," an S3 Lifecycle policy can transition these files to S3 Glacier Flexible Retrieval. This storage class is very cost-effective for archival data, with retrieval options ranging from minutes to hours, fitting the "delay is acceptable" criteria.
- [ ] Querying Capabilities:
- [ ] For files within S3 Intelligent-Tiering's active tiers (accessible via standard S3 APIs), Amazon Athena can be used for querying (e.g., if the transcripts or their metadata are in a queryable format).
- [ ] For files archived in S3 Glacier Flexible Retrieval, S3 Glacier Select allows you to perform SQL-like queries directly on the archived object's content without needing to restore the entire object, making it efficient to find and retrieve specific information from older files.
- [ ] MOST Cost-Effective Overall: This solution balances performance for newer files with aggressive cost savings for older, infrequently accessed files, along with appropriate query capabilities for both states.

Why are the other answers wrong?

- [ ] Option A is wrong because: Storing all files, including those actively and randomly accessed within the first year, directly in S3 Glacier Instant Retrieval would be more expensive for storage than using S3 Intelligent-Tiering for that initial period. S3 Glacier Instant Retrieval is an archive tier, albeit with fast retrieval, but it's designed for data that is rarely accessed overall.
- [ ] Option C is wrong because: Using S3 Standard for the first year is less cost-optimal than S3 Intelligent-Tiering if access patterns are indeed random and include periods of infrequent access even within that first year. While S3 Glacier Instant Retrieval is a good option for archiving data that needs quick access, S3 Glacier Flexible Retrieval (as in option B) generally offers lower storage costs if some retrieval delay (minutes to hours) is acceptable for files older than a year.
- [ ] Option D is wrong because: S3 Glacier Deep Archive is the lowest-cost storage, but its retrieval times are typically 12 hours or more. While a delay is acceptable, this might be longer than necessary or practical. More significantly, storing search metadata in Amazon RDS introduces the cost and operational overhead of managing a relational database. A combination of S3 object metadata, Athena, and S3 Glacier Select often provides a more cost-effective and integrated querying solution for data stored in S3 and its archive tiers.

</details>

<details>
  <summary>Question 50</summary>

A company has a production workload that runs on 1,000 Amazon EC2 Linux instances. The workload is powered by third-party software. The company needs to patch the third-party software on all EC2 instances as quickly as possible to remediate a critical security vulnerability.    

What should a solutions architect do to meet these requirements?

- [ ] A. Create an AWS Lambda function to apply the patch to all EC2 instances. 
- [ ] B. Configure AWS Systems Manager Patch Manager to apply the patch to all EC2 instances. 
- [ ] C. Schedule an AWS Systems Manager maintenance window to apply the patch to all EC2 instances. 
- [ ] D. Use AWS Systems Manager Run Command to run a custom command that applies the patch to all EC2 instances.    

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Use AWS Systems Manager Run Command to run a custom command that applies the patch to all EC2 instances.    

Why this is the correct answer:

- [ ] AWS Systems Manager Run Command for Immediate Execution: AWS Systems Manager Run Command allows you to remotely and securely execute commands on a fleet of managed instances (including EC2 instances) at scale. For a "critical security vulnerability" that needs to be remediated "as quickly as possible," Run Command provides the ability to immediately execute the necessary custom scripts or commands to apply the patch for the "third-party software."    
- [ ] Scalability and Targeting: Run Command can target a large number of instances (like 1,000) simultaneously based on tags, resource groups, or individual instance IDs. This ensures the patch is applied broadly and quickly.
- [ ] Custom Commands for Third-Party Software: Since it's a patch for third-party software, it will likely require specific commands or a custom script for installation, which Run Command can execute.

Why are the other answers wrong?

- [ ] A. Create an AWS Lambda function to apply the patch to all EC2 instances.    
While Lambda can automate tasks, using it to orchestrate patching across 1,000 EC2 instances (which might involve establishing connections, executing commands remotely, handling state, and managing errors for each instance) would be a complex custom solution to build and manage, especially for urgent deployment. AWS Systems Manager provides purpose-built tools for this.
- [ ] B. Configure AWS Systems Manager Patch Manager to apply the patch to all EC2 instances.    
AWS Systems Manager Patch Manager is designed to automate the patching of operating systems and some common applications based on defined patch baselines and schedules. While it can be extended for some application patching, it is generally more structured and might not be the quickest method for deploying an urgent, custom patch for specific "third-party software" that isn't covered by standard patch baselines. Run Command offers more direct control for immediate, custom actions.
- [ ] C. Schedule an AWS Systems Manager maintenance window to apply the patch to all EC2 instances.    
Maintenance windows are used to define recurring schedules for performing potentially disruptive actions like patching to minimize impact on production workloads. The requirement here is to remediate a "critical security vulnerability" "as quickly as possible." Waiting for a scheduled maintenance window or setting one up might introduce unacceptable delays. Run Command allows for immediate execution.

</details>

</details>

<details>
  <summary>==Questions 51-60==</summary>

<details>
  <summary>Question 51</summary>

A company is developing an application that provides order shipping statistics for retrieval by a REST API. The company wants to extract the shipping statistics, organize the data into an easy-to-read HTML format, and send the report to several email addresses at the same time every morning.

Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

- [ ] A. Configure the application to send the data to Amazon Kinesis Data Firehose.
- [ ] B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.
- [ ] C. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Glue job to query the application's API for the data.
- [ ] D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.
- [ ] E. Store the application data in Amazon S3. Create an Amazon Simple Notification Service (Amazon SNS) topic as an S3 event destination to send the report by email.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.
- [ ] D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.

Why these are the correct answers:

- [ ] D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.
Scheduled Execution: To send the report "at the same time every morning," a scheduled trigger is needed. Amazon EventBridge (formerly CloudWatch Events) allows you to create scheduled rules (e.g., using a cron expression) that can trigger various AWS services.
Data Extraction and Formatting: An AWS Lambda function is well-suited to be invoked by the EventBridge schedule. The Lambda function can then make a request to the application's REST API to "extract the shipping statistics." Once the data is retrieved, the Lambda function can "organize the data into an easy-to-read HTML format." Lambda is a serverless compute service ideal for such event-driven tasks.
- [ ] B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.
Email Delivery: Amazon Simple Email Service (SES) is a cloud-based email sending service designed to send notification and transactional emails. It can be used by the Lambda function (from option D) to "send the report to several email addresses."
Sending HTML Emails: While the Lambda function would primarily handle the HTML formatting of the report content, SES is capable of sending emails with HTML content, allowing the "easy-to-read HTML format" to be delivered effectively.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Kinesis Data Firehose is designed for capturing, transforming, and loading streaming data into data lakes, data stores, and analytics services. It is not suitable for querying a REST API on a schedule to extract statistics for a daily email report.
- [ ] Option C is wrong because: While Amazon EventBridge is appropriate for scheduling, using an AWS Glue job for this task is overly complex and not its primary design purpose. AWS Glue is an ETL (extract, transform, and load) service more suited for large-scale data processing and cataloging, typically involving data stores like S3 or databases, rather than querying a REST API for a daily report and formatting HTML for email. An AWS Lambda function is more lightweight and fitting for this scenario.
- [ ] Option E is wrong because: This option suggests storing data in S3 and using S3 event notifications with Amazon SNS. The primary data source is a REST API, not new objects being written to S3 that would trigger an event. While SNS can send email notifications, it is generally more for simple notifications, and SES (as in option B) provides more robust capabilities for sending formatted HTML emails, managing sender reputation, and handling bounces or complaints, which is more suitable for sending reports.

</details>

<details>
  <summary>Question 52</summary>

A company wants to migrate its on-premises application to AWS. The application produces output files that vary in size from tens of gigabytes to hundreds of terabytes. The application data must be stored in a standard file system structure. The company wants a solution that scales automatically, is highly available, and requires minimum operational overhead.

Which solution will meet these requirements?

- [ ] A. Migrate the application to run as containers on Amazon Elastic Container Service (Amazon ECS). Use Amazon S3 for storage.
- [ ] B. Migrate the application to run as containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use Amazon Elastic Block Store (Amazon EBS) for storage.
- [ ] C. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic File System (Amazon EFS) for storage.
- [ ] D. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic Block Store (Amazon EBS) for storage.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic File System (Amazon EFS) for storage.

Why this is the correct answer:

- [ ] Standard File System Structure (Amazon EFS): The requirement that "application data must be stored in a standard file system structure" points towards a service like Amazon EFS. EFS provides elastic, scalable file storage that uses the Network File System (NFS) protocol, which is a standard file system interface. EC2 instances can mount an EFS file system just like any network file share.
- [ ] Scales Automatically (EFS and Auto Scaling): EFS storage capacity scales automatically up or down as you add or remove files, so you don't need to provision storage in advance. Running the application on EC2 instances within a Multi-AZ Auto Scaling group allows the compute tier to scale automatically based on demand.
- [ ] Highly Available (EFS and Multi-AZ Auto Scaling): EFS is designed for high availability and durability by storing data redundantly across multiple Availability Zones (AZs) within a region. A Multi-AZ Auto Scaling group ensures that the application instances are also distributed across multiple AZs for high availability.
- [ ] Minimum Operational Overhead: EFS is a fully managed service, removing the need to manage file servers or storage infrastructure. EC2 Auto Scaling also reduces the operational burden of managing the compute fleet.
- [ ] Handles Large Files/Datasets: EFS can store petabytes of data and supports large individual file sizes, suitable for "output files that vary in size from tens of gigabytes to hundreds of terabytes" (this likely refers to total dataset size, as individual files in EFS have a limit, but the system as a whole can scale).

Why are the other answers wrong?

- [ ] Option A is wrong because: While ECS is a good option for running containerized applications, Amazon S3 is an object storage service. It does not provide a standard, mountable file system structure that an application expecting POSIX-like file system semantics can directly use without modification or tools like S3FS (which can have performance implications). The requirement is for a "standard file system structure."
- [ ] Option B is wrong because: Amazon Elastic Block Store (EBS) provides block-level storage volumes for EC2 instances. A standard EBS volume can only be attached to a single EC2 instance at a time within a specific AZ. While EKS can manage containers, if these containers need shared access to a "standard file system structure" across multiple nodes (especially across AZs for high availability), EBS is not the direct solution. EFS or FSx would be better for shared file storage.
- [ ] Option D is wrong because: Similar to option B, using EBS with EC2 instances in an Auto Scaling group means each instance would typically have its own isolated EBS volume(s). This does not provide a shared "standard file system structure" that can be concurrently accessed by all instances in the Auto Scaling group, which is often implied when an application needs a common file system.

</details>

<details>
  <summary>Question 53</summary>

A company needs to store its accounting records in Amazon S3. The records must be immediately accessible for 1 year and then must be archived for an additional 9 years. No one at the company, including administrative users and root users, can be able to delete the records during the entire 10-year period. The records must be stored with maximum resiliency.

Which solution will meet these requirements?

- [ ] A. Store the records in S3 Glacier for the entire 10-year period. Use an access control policy to deny deletion of the records for a period of 10 years.
- [ ] B. Store the records by using S3 Intelligent-Tiering. Use an IAM policy to deny deletion of the records. After 10 years, change the IAM policy to allow deletion.
- [ ] C. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 Glacier Deep Archive after 1 year. Use S3 Object Lock in compliance mode for a period of 10 years.
- [ ] D. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 year. Use S3 Object Lock in governance mode for a period of 10 years.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 Glacier Deep Archive after 1 year. Use S3 Object Lock in compliance mode for a period of 10 years.

Why this is the correct answer:

Immediate Accessibility and Archival:
- [ ] Storing records initially in Amazon S3 Standard (implied, as lifecycle policies transition from a storage class) ensures they are "immediately accessible for 1 year."    
- [ ] An S3 Lifecycle policy can then "transition the records from S3 Standard to S3 Glacier Deep Archive after 1 year." S3 Glacier Deep Archive is designed for long-term, low-cost archival storage, fitting the "archived for an additional 9 years" requirement.    
- [ ] Non-Deletion by Anyone (S3 Object Lock in Compliance Mode):
- [ ] S3 Object Lock in compliance mode is a Write-Once-Read-Many (WORM) model. When an object version is locked in compliance mode, its retention mode cannot be changed, and its retention period cannot be shortened by any user, including the root user in the AWS account, for the duration of the specified retention period (10 years in this case).  This stringently meets the requirement that "No one at the company, including administrative users and root users, can be able to delete the records during the entire 10-year period."    
- [ ] Maximum Resiliency:
- [ ] Amazon S3 Standard and S3 Glacier Deep Archive both store data redundantly across multiple (at least three) Availability Zones within an AWS Region, providing "maximum resiliency."    

Why are the other answers wrong?

- [ ] Option A is wrong because: Storing records directly in S3 Glacier (e.g., S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive) from the beginning does not allow for immediate accessibility during the first year, as standard retrievals from these services take minutes to hours.  While an access control policy can deny deletions, it can typically be modified or bypassed by users with sufficient privileges (like root or administrators), unlike S3 Object Lock in compliance mode.    
- [ ] Option B is wrong because: An IAM policy denying deletion can be altered or overridden by users with the necessary administrative permissions, including the root user. This does not provide the same level of immutability as S3 Object Lock in compliance mode, which is needed to ensure "No one... can be able to delete the records."  S3 Intelligent-Tiering is for optimizing costs based on access patterns but doesn't inherently provide the stringent deletion protection required.   
- [ ] Option D is wrong because:
S3 One Zone-Infrequent Access (S3 One Zone-IA) stores data in a single Availability Zone.  This does not offer "maximum resiliency" as data would be lost if that single AZ experiences a failure.    
S3 Object Lock in governance mode allows users with specific IAM permissions (s3:BypassGovernanceRetention) to override the lock settings or delete objects. This does not meet the requirement that "No one... can be able to delete the records."  Compliance mode is stricter.

</details>

<details>
  <summary>Question 54</summary>

A company runs multiple Windows workloads on AWS. The company's employees use Windows file shares that are hosted on two Amazon EC2 instances. The file shares synchronize data between themselves and maintain duplicate copies. The company wants a highly available and durable storage solution that preserves how users currently access the files.

What should a solutions architect do to meet these requirements?

- [ ] A. Migrate all the data to Amazon S3. Set up IAM authentication for users to access files.
- [ ] B. Set up an Amazon S3 File Gateway. Mount the S3 File Gateway on the existing EC2 instances.
- [ ] C. Extend the file share environment to Amazon FSx for Windows File Server with a Multi-AZ configuration. Migrate all the data to FSx for Windows File Server.
- [ ] D. Extend the file share environment to Amazon Elastic File System (Amazon EFS) with a Multi-AZ configuration. Migrate all the data to Amazon EFS.

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn
Correct Answer: C

Why this is the correct answer:

- [ ] Amazon FSx for Windows File Server: This service provides fully managed, native Microsoft Windows file systems, which means it supports the SMB (Server Message Block) protocol used by Windows file shares. This directly addresses the requirement to "preserve how users currently access the files".   
- [ ] High Availability and Durability: Amazon FSx for Windows File Server supports Multi-AZ deployments. In a Multi-AZ configuration, FSx automatically provisions and maintains a standby file server in a different Availability Zone, and data is synchronously replicated. This ensures high availability and enhances durability, protecting against the loss of an AZ.   
- [ ] Replaces EC2-based File Shares: Migrating the data to FSx for Windows File Server replaces the existing solution of two EC2 instances manually synchronizing data, which can be complex and less reliable. FSx is a managed service designed for this purpose, reducing operational overhead.
- [ ] Preserves Access Patterns: Users can continue to access the file shares using their existing Windows credentials (as FSx integrates with Active Directory) and familiar drive mapping methods.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon S3 is an object storage service. While it's highly durable and scalable, it does not natively provide SMB file share access. Users would need to change how they access files (e.g., via S3 APIs, console, or third-party tools), which does not "preserve how users currently access the files".   
- [ ] Option B is wrong because: While an Amazon S3 File Gateway can provide an SMB interface to data stored in S3, mounting this gateway on the existing EC2 instances that currently host the shares doesn't fully replace the EC2-based solution with a more robust, managed file server. The goal is to improve the availability and durability of the storage solution itself, and FSx for Windows File Server directly provides a managed Windows file system.   
- [ ] Option D is wrong because: Amazon Elastic File System (Amazon EFS) is a file storage service that uses the NFS (Network File System) protocol. While EFS is highly available and scalable, Windows environments typically use the SMB protocol for file sharing. Using EFS would require Windows clients to use an NFS client or other workarounds, which does not "preserve how users currently access the files"  in a native Windows file share manner.

</details>

<details>
  <summary>Question 55</summary>

A solutions architect is developing a VPC architecture that includes multiple subnets. The architecture will host applications that use Amazon EC2 instances and Amazon RDS DB instances. The architecture consists of six subnets in two Availability Zones. Each Availability Zone includes a public subnet, a private subnet, and a dedicated subnet for databases. Only EC2 instances that run in the private subnets can have access to the RDS databases.

Which solution will meet these requirements?

- [ ] A. Create a new route table that excludes the route to the public subnets' CIDR blocks. Associate the route table with the database subnets.
- [ ] B. Create a security group that denies inbound traffic from the security group that is assigned to instances in the public subnets. Attach the security group to the DB instances.
- [ ] C. Create a security group that allows inbound traffic from the security group that is assigned to instances in the private subnets. Attach the security group to the DB instances.
- [ ] D. Create a new peering connection between the public subnets and the private subnets. Create a different peering connection between the private subnets and the database subnets.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create a security group that allows inbound traffic from the security group that is assigned to instances in the private subnets. Attach the security group to the DB instances.

Why this is the correct answer:

- [ ] Security Groups for Instance-Level Access Control: Security groups act as stateful virtual firewalls for your instances (including EC2 and RDS) to control inbound and outbound traffic. They operate at the instance level.
- [ ] Referencing Security Groups as a Source: A key feature of security groups is that you can specify another security group as the source for an inbound rule. This allows instances associated with the source security group to access instances associated with the destination security group on the specified ports and protocols.
- [ ] Meeting the Requirement:
- [ ] You would have a security group (e.g., sg-private-app) attached to the EC2 instances running in the private subnets.
- [ ] You would have a security group (e.g., sg-database) attached to the RDS DB instances in the database subnets.
- [ ] In the inbound rules of sg-database, you would add a rule that allows traffic on the database port (e.g., 3306 for MySQL, 5432 for PostgreSQL) and specifies sg-private-app as the source. This configuration ensures that only EC2 instances in the private subnets (those associated with sg-private-app) can initiate connections to the RDS databases. All other traffic would be implicitly denied by the security group.

Why are the other answers wrong?

- [ ] Option A is wrong because: Route tables control the routing of traffic at the subnet level (i.e., where network packets are sent). While they are essential for connectivity between subnets, they do not provide the fine-grained, stateful, instance-level access control that security groups offer. Modifying route tables for the database subnets wouldn't specifically grant or deny access from instances in private subnets based on instance identity or group membership.
- [ ] Option B is wrong because: Security groups work on an "allow list" principle. By default, all inbound traffic is denied unless explicitly allowed by an inbound rule. There is no explicit "deny" rule in security groups. To achieve the desired outcome, you should allow traffic from the intended source (private subnets' instances) rather than trying to explicitly deny traffic from other sources like public subnets.
- [ ] Option D is wrong because: VPC peering connections are used to enable private network connectivity between two different VPCs. The scenario describes subnets (public, private, database) that are all part of the same VPC architecture. Therefore, VPC peering is not relevant or necessary for controlling access between these subnets. Access control within a single VPC is managed using security groups and Network Access Control Lists (NACLs).

</details>

<details>
  <summary>Question 56</summary>

A company has registered its domain name with Amazon Route 53. The company uses Amazon API Gateway in the ca-central-1 Region as a public interface for its backend microservice APIs. Third-party services consume the APIs securely. The company wants to design its API Gateway URL with the company's domain name and corresponding certificate so that the third-party services can use HTTPS.

Which solution will meet these requirements?

- [ ] A. Create stage variables in API Gateway with Name="Endpoint-URL" and Value="Company Domain Name" to overwrite the default URL. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM).
- [ ] B. Create Route 53 DNS records with the company's domain name. Point the alias record to the Regional API Gateway stage endpoint. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region.
- [ ] C. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the same Region. Attach the certificate to the API Gateway endpoint. Configure Route 53 to route traffic to the API Gateway endpoint.
- [ ] D. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region. Attach the certificate to the API Gateway APIs. Create Route 53 DNS records with the company's domain name. Point an A record to the company's domain name.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the same Region. Attach the certificate to the API Gateway endpoint. Configure Route 53 to route traffic to the API Gateway endpoint.

Why this is the correct answer:

- [ ] Custom Domain Name for API Gateway: Amazon API Gateway allows you to set up a custom domain name (e.g., api.yourcompany.com) for your APIs. This involves creating a custom domain name resource within API Gateway.
- [ ] Regional Endpoint and Certificate Location: The question states the API Gateway is in the ca-central-1 Region, implying a Regional API endpoint. For Regional API Gateway custom domain names, the SSL/TLS certificate must be imported into or provisioned by AWS Certificate Manager (ACM) in the same AWS Region as the API Gateway (i.e., ca-central-1).
- [ ] Certificate Association: The imported public certificate from ACM is then associated with the custom domain name configuration in API Gateway.
- [ ] Route 53 Configuration: Finally, you create a DNS record (typically an Alias record for AWS resources) in Amazon Route 53 to point your custom domain name (e.g., api.yourcompany.com) to the API Gateway regional custom domain name target that API Gateway provides after you configure the custom domain.
- [ ] This sequence correctly sets up a custom domain with HTTPS for a Regional API Gateway endpoint.

Why are the other answers wrong?

- [ ] Option A is wrong because: Stage variables in API Gateway are used for passing configuration settings to your API stages (like different backend endpoints for dev vs. prod). They cannot be used to overwrite the default API Gateway URL with a custom domain name.
- [ ] Option B is wrong because: For a Regional API Gateway custom domain, the ACM certificate must be in the same Region as the API Gateway instance (ca-central-1 in this case). ACM certificates in us-east-1 are required for Edge-Optimized API Gateway endpoints (which use Amazon CloudFront) or for CloudFront distributions themselves, not for regional custom domains.
- [ ] Option D is wrong because: This option also incorrectly states that the ACM certificate for a Regional API Gateway custom domain should be in us-east-1. Additionally, creating an A record pointing the "company's domain name" to itself is a circular reference and incorrect. The DNS record needs to point the custom domain to the API Gateway's specific target domain name provided for custom domains.

</details>

<details>
  <summary>Question 57</summary>

A company is running a popular social media website. The website gives users the ability to upload images to share with other users. The company wants to make sure that the images do not contain inappropriate content. The company needs a solution that minimizes development effort.

What should a solutions architect do to meet these requirements?

- [ ] A. Use Amazon Comprehend to detect inappropriate content. Use human review for low-confidence predictions.
- [ ] B. Use Amazon Rekognition to detect inappropriate content. Use human review for low-confidence predictions.
- [ ] C. Use Amazon SageMaker to detect inappropriate content. Use ground truth to label low-confidence predictions.
- [ ] D. Use AWS Fargate to deploy a custom machine learning model to detect inappropriate content. Use ground truth to label low-confidence predictions.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use Amazon Rekognition to detect inappropriate content. Use human review for low-confidence predictions.

Why this is the correct answer:

- [ ] Amazon Rekognition for Image Moderation: Amazon Rekognition is an AWS service that provides pre-trained machine learning models for image and video analysis. It includes specific content moderation capabilities to detect inappropriate, unwanted, or offensive content (e.g., nudity, violence, suggestive content) in images and videos. This directly addresses the requirement to ensure images do not contain inappropriate content.
- [ ] Minimizes Development Effort: As a managed service with pre-trained models, Amazon Rekognition requires significantly less development effort compared to building, training, and deploying a custom machine learning model using services like Amazon SageMaker or a custom deployment on Fargate. You can integrate Rekognition into your application using its API.
- [ ] Human Review for Low-Confidence Predictions: Rekognition provides confidence scores for its moderation labels. For cases where the confidence score is low, it's a common best practice to implement a human review workflow (e.g., using Amazon Augmented AI - A2I, or a custom system) to ensure accuracy and handle ambiguous content appropriately.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Comprehend is a natural language processing (NLP) service designed to extract insights from text. It is not suitable for analyzing image content to detect inappropriate visuals.
- [ ] Option C is wrong because: Amazon SageMaker is a comprehensive machine learning platform for building, training, and deploying custom ML models. While you could theoretically build a custom image moderation model with SageMaker, this would involve substantial development effort (data collection, labeling, model training, and deployment), which contradicts the requirement to "minimize development effort." Rekognition offers this functionality out-of-the-box.
- [ ] Option D is wrong because: Developing a custom machine learning model and deploying it on AWS Fargate would also require significant development effort to create and train the model, similar to using SageMaker. This approach does not leverage pre-built AWS AI services for common tasks like content moderation and therefore does not minimize development effort.

</details>

<details>
  <summary>Question 58</summary>

A company wants to run its critical applications in containers to meet requirements for scalability and availability. The company prefers to focus on maintenance of the critical applications. The company does not want to be responsible for provisioning and managing the underlying infrastructure that runs the containerized workload.

What should a solutions architect do to meet these requirements?

- [ ] A. Use Amazon EC2 instances, and install Docker on the instances.
- [ ] B. Use Amazon Elastic Container Service (Amazon ECS) on Amazon EC2 worker nodes.
- [ ] C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.
- [ ] D. Use Amazon EC2 instances from an Amazon Elastic Container Service (Amazon ECS)-optimized Amazon Machine Image (AMI).

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.

Why this is the correct answer:

- [ ] Amazon Elastic Container Service (Amazon ECS) for Container Orchestration: ECS is a fully managed container orchestration service that simplifies deploying, managing, and scaling containerized applications. This helps meet the requirements for running applications in containers for scalability and availability.    
- [ ] AWS Fargate for Serverless Compute: AWS Fargate is a serverless, pay-as-you-go compute engine for containers that works with both Amazon ECS and Amazon EKS. When using Fargate with ECS, you do not need to provision, configure, scale, or manage the underlying virtual machines (EC2 instances). AWS handles all the infrastructure management. This directly addresses the company's desire to "not want to be responsible for provisioning and managing the underlying infrastructure" and allows them to "focus on maintenance of the critical applications."    
- [ ] Scalability and Availability: ECS with Fargate allows you to define your application, specify CPU and memory requirements, define networking and IAM policies, and launch. It handles scaling and availability aspects without requiring server management.    

Why are the other answers wrong?

- [ ] Option A is wrong because: Using Amazon EC2 instances and installing Docker manually means the company would be fully responsible for provisioning, managing, patching, and scaling the EC2 instances, as well as managing the Docker environment and potentially the container orchestration. This directly contradicts the requirement of not wanting to manage the underlying infrastructure.    
- [ ] Option B is wrong because: Using Amazon ECS with the EC2 launch type (on Amazon EC2 worker nodes) still requires the company to provision, manage, and scale the cluster of EC2 instances that run the containers. While ECS manages the containers, the underlying server infrastructure management remains the company's responsibility.    
- [ ] Option D is wrong because: Using EC2 instances from an ECS-optimized AMI simplifies the setup of EC2 instances for an ECS cluster (EC2 launch type), but it still means the company is responsible for managing those EC2 instances (patching, scaling, security). This does not remove the responsibility for the underlying infrastructure.

</details>

<details>
  <summary>Question 59</summary>

A company hosts more than 300 global websites and applications. The company requires a platform to analyze more than 30 TB of clickstream data each day.

What should a solutions architect do to transmit and process the clickstream data?

- [ ] A. Design an AWS Data Pipeline to archive the data to an Amazon S3 bucket and run an Amazon EMR cluster with the data to generate analytics.
- [ ] B. Create an Auto Scaling group of Amazon EC2 instances to process the data and send it to an Amazon S3 data lake for Amazon Redshift to use for analysis.
- [ ] C. Cache the data to Amazon CloudFront. Store the data in an Amazon S3 bucket. When an object is added to the S3 bucket. run an AWS Lambda function to process the data for analysis.
- [ ] D. Collect the data from Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to transmit the data to an Amazon S3 data lake. Load the data in Amazon Redshift for analysis.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Collect the data from Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to transmit the data to an Amazon S3 data lake. Load the data in Amazon Redshift for analysis.

Why this is the correct answer:

- [ ] Amazon Kinesis Data Streams for Real-Time Ingestion: For collecting high-volume (30 TB/day) clickstream data from hundreds of global sources in real-time, Amazon Kinesis Data Streams is a suitable and scalable ingestion service. It can handle a continuous flow of data.
- [ ] Amazon Kinesis Data Firehose for Delivery to S3 Data Lake: Kinesis Data Firehose can take data from Kinesis Data Streams (or directly from data producers) and reliably load it into destinations like Amazon S3. It can also batch, compress, and encrypt the data before delivery, which is efficient for building an S3 data lake with large volumes of clickstream data.
- [ ] Amazon Redshift for Analysis: Amazon Redshift is a petabyte-scale data warehouse service optimized for complex analytical queries on large datasets. Loading data from an S3 data lake into Redshift is a common and effective pattern for analyzing clickstream data to derive insights.
- [ ] Scalable and Managed Pipeline: This combination of services provides a robust, scalable, and largely managed pipeline for ingesting, storing, and analyzing large volumes of clickstream data with less operational overhead than building custom solutions.

Why are the other answers wrong?

- [ ] Option A is wrong because: AWS Data Pipeline is a service for orchestrating scheduled, batch data movement and transformation workflows. While it can involve S3 and EMR, Kinesis services are generally better suited for ingesting and preparing continuous, high-volume real-time data like clickstream data.
- [ ] Option B is wrong because: Building a custom solution with an Auto Scaling group of EC2 instances to process 30 TB of clickstream data daily would involve significant development and operational overhead for managing the instances, the data processing logic, and ensuring scalability and fault tolerance. Managed services like Kinesis offer these capabilities with less effort.
- [ ] Option C is wrong because: Amazon CloudFront is a content delivery network (CDN) used for caching and delivering web content to users with low latency; it's not a primary tool for collecting clickstream data for backend analysis. While AWS Lambda can process data from S3, processing 30 TB of data daily using Lambda triggered by S3 object creation might be inefficient for this scale compared to stream processing or batch analytics services like EMR or Redshift for the analysis part. The ingestion via Kinesis is also missing.

</details>

<details>
  <summary>Question 60</summary>

A company has a website hosted on AWS. The website is behind an Application Load Balancer (ALB) that is configured to handle HTTP and HTTPS separately. The company wants to forward all requests to the website so that the requests will use HTTPS.

What should a solutions architect do to meet this requirement?

- [ ] A. Update the ALB's network ACL to accept only HTTPS traffic.
- [ ] B. Create a rule that replaces the HTTP in the URL with HTTPS.
- [ ] C. Create a listener rule on the ALB to redirect HTTP traffic to HTTPS.
- [ ] D. Replace the ALB with a Network Load Balancer configured to use Server Name Indication (SNI).

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create a listener rule on the ALB to redirect HTTP traffic to HTTPS.

Why this is the correct answer:

- [ ] Application Load Balancer (ALB) Listener Rules: ALBs allow you to define listeners for different ports and protocols (e.g., HTTP on port 80 and HTTPS on port 443). For each listener, you can configure rules that determine how requests are routed.
- [ ] HTTP to HTTPS Redirection: A common and recommended practice is to configure the HTTP listener (on port 80) with a rule that performs a redirect action.
- [ ] This rule will automatically redirect all incoming HTTP requests to the corresponding HTTPS URL (on port 443).
- [ ] This ensures that users who try to access the site via HTTP are seamlessly forwarded to the secure HTTPS version. This is a built-in feature of ALBs designed for this exact purpose.

Why are the other answers wrong?

- [ ] Option A is wrong because: Network ACLs (NACLs) operate at the subnet level and are stateless firewalls. While you could use a NACL to block HTTP traffic, it would not "forward" or "redirect" requests to HTTPS; it would simply drop or deny the HTTP requests. The goal is to ensure users end up on HTTPS, not just block HTTP. Furthermore, ALB nodes are managed by AWS, and manipulating their direct NACLs is not the standard way to control ALB behavior.
- [ ] Option B is wrong because: This statement is too vague. While the outcome is to have the URL use HTTPS, it doesn't specify how or where this rule would be implemented. Option C provides the specific, AWS-native mechanism (ALB listener rule) to achieve this at the load balancer level, which is the appropriate place for this kind of infrastructure-level redirection.
- [ ] Option D is wrong because:
Network Load Balancers (NLBs) operate at Layer 4 (transport layer) and are not as feature-rich for HTTP/HTTPS traffic management as ALBs, which operate at Layer 7 (application layer). ALBs are better suited for tasks like HTTP to HTTPS redirection.
Server Name Indication (SNI) is a TLS extension that allows a server to present multiple certificates on the same IP address and port, enabling hosting of multiple SSL-secured websites on one server. While NLBs do support SNI (for TLS listeners), SNI itself is not a mechanism for redirecting HTTP traffic to HTTPS.

</details>

</details>

<details>
  <summary>==Questions 61-70==</summary>

<details>
  <summary>Question 61</summary>

A company is developing a two-tier web application on AWS. The company's developers have deployed the application on an Amazon EC2 instance that connects directly to a backend Amazon RDS database. The company must not hardcode database credentials in the application. The company must also implement a solution to automatically rotate the database credentials on a regular basis.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Store the database credentials in the instance metadata. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and instance metadata at the same time.
- [ ] B. Store the database credentials in a configuration file in an encrypted Amazon S3 bucket. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and the credentials in the configuration file at the same time. Use S3 Versioning to ensure the ability to fall back to previous values.
- [ ] C. Store the database credentials as a secret in AWS Secrets Manager. Turn on automatic rotation for the secret. Attach the required permission to the EC2 role to grant access to the secret.
- [ ] D. Store the database credentials as encrypted parameters in AWS Systems Manager Parameter Store. Turn on automatic rotation for the encrypted parameters. Attach the required permission to the EC2 role to grant access to the encrypted parameters.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Store the database credentials as a secret in AWS Secrets Manager. Turn on automatic rotation for the secret. Attach the required permission to the EC2 role to grant access to the secret.

Why this is the correct answer:

- [ ] AWS Secrets Manager for Secure Credential Management: AWS Secrets Manager is specifically designed to help you protect access to your applications, services, and IT resources without the upfront investment and ongoing maintenance costs of operating your own infrastructure. It allows you to store database credentials securely. This meets the requirement to not hardcode credentials.   
- [ ] Automatic Rotation: A key feature of Secrets Manager is its ability to automatically rotate secrets for supported services like Amazon RDS on a schedule you define. This directly addresses the requirement for automatic credential rotation with minimal operational overhead, as AWS manages the rotation process.
- [ ] Secure Access via IAM Roles: The application running on the EC2 instance can be granted permissions to retrieve the database credentials from Secrets Manager by attaching an appropriate IAM role to the EC2 instance. The application code would then fetch the credentials from Secrets Manager at runtime.
- [ ] Least Operational Overhead: This solution relies on a fully managed AWS service for both storing and automatically rotating credentials, which is the most straightforward approach with the least ongoing management effort.

Why are the other answers wrong?

- [ ] Option A is wrong because: Storing sensitive database credentials directly in EC2 instance metadata is not a secure practice, as instance metadata is accessible from within the instance. Building a custom rotation solution with EventBridge and Lambda adds operational overhead compared to the managed rotation offered by Secrets Manager.
- [ ] Option B is wrong because: While storing credentials in an encrypted S3 bucket is more secure than hardcoding, it still requires custom logic for the application to retrieve and decrypt the credentials. The rotation process via EventBridge and Lambda is also a custom solution, which means more development and maintenance effort (higher operational overhead) compared to using Secrets Manager's built-in rotation capabilities.
- [ ] Option D is wrong because: AWS Systems Manager Parameter Store can store secrets securely (as SecureString parameters) and integrate with IAM for access control. However, while Parameter Store can be integrated with AWS Lambda to build custom rotation logic, its native, out-of-the-box automatic rotation capabilities for RDS database credentials are not as direct or comprehensive as those provided by AWS Secrets Manager. Secrets Manager is the preferred service for fully managed, automatic rotation of RDS credentials, resulting in less operational overhead for this specific task.

</details>  

<details>
  <summary>Question 62</summary>
  
A company is deploying a new public web application to AWS. The application will run behind an Application Load Balancer (ALB). The application needs to be encrypted at the edge with an SSL/TLS certificate that is issued by an external certificate authority (CA). The certificate must be rotated each year before the certificate expires.

What should a solutions architect do to meet these requirements?

- [ ] A. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
- [ ] B. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Import the key material from the certificate. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
- [ ] C. Use AWS Certificate Manager (ACM) Private Certificate Authority to issue an SSL/TLS certificate from the root CA. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.
- [ ] D. Use AWS Certificate Manager (ACM) to import an SSL/TLS certificate. Apply the certificate to the ALB. Use Amazon EventBridge (Amazon CloudWatch Events) to send a notification when the certificate is nearing expiration. Rotate the certificate manually.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Use AWS Certificate Manager (ACM) to import an SSL/TLS certificate. Apply the certificate to the ALB. Use Amazon EventBridge (Amazon CloudWatch Events) to send a notification when the certificate is nearing expiration. Rotate the certificate manually.

Why this is the correct answer:

- [ ] Using an Externally Issued Certificate: The requirement states that the SSL/TLS certificate is "issued by an external certificate authority (CA)."
- [ ] Importing into ACM: AWS Certificate Manager (ACM) allows you to import third-party SSL/TLS certificates that you've obtained from an external CA. Once imported, these certificates can be used with integrated AWS services like Application Load Balancers (ALBs).
- [ ] Applying to ALB: After the certificate is imported into ACM, it can be associated with the ALB's HTTPS listener to enable SSL/TLS termination at the load balancer.
- [ ] Manual Rotation for Imported Certificates: ACM's automatic managed renewal feature only applies to public certificates that are issued by ACM. For certificates that are imported into ACM from an external CA, ACM does not manage their renewal. The company is responsible for renewing the certificate with the external CA and then re-importing the new certificate into ACM before the existing one expires.
- [ ] Notification for Expiration: To manage this manual rotation process, it's a best practice to set up notifications. Amazon EventBridge (formerly CloudWatch Events) can be used with ACM to detect when an imported certificate is nearing its expiration date. An EventBridge rule can then trigger a notification (e.g., via Amazon SNS to email administrators) to remind them to rotate the certificate manually.

Why are the other answers wrong?

- [ ] Option A is wrong because: This suggests using ACM to issue an SSL/TLS certificate. However, the requirement is to use a certificate "issued by an external certificate authority." While ACM can issue public certificates (which do support managed renewal), this option doesn't align with the given constraint about the certificate's origin.
- [ ] Option B is wrong because: This option is contradictory. It first says "Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate" and then "Import the key material from the certificate." If ACM issues the certificate, you don't separately import its key material in that manner. Furthermore, it still relies on managed renewal, which isn't available for externally issued certificates.
- [ ] Option C is wrong because: AWS Certificate Manager (ACM) Private Certificate Authority (PCA) is used to create and manage private SSL/TLS certificates for internal use cases (e.g., within your organization's private network, for encrypting traffic between internal services). For a "public web application" that needs to be trusted by users' browsers, a publicly trusted certificate is required, not one issued by a private CA (unless the private CA's root is distributed to all clients, which is not typical for public applications).

</details>

<details>
  <summary>Question 63</summary>
 
A company runs its infrastructure on AWS and has a registered base of 700,000 users for its document management application. The company intends to create a product that converts large .pdf files to .jpg image files. The .pdf files average 5 MB in size. The company needs to store the original files and the converted files.

A solutions architect must design a scalable solution to accommodate demand that will grow rapidly over time.

Which solution meets these requirements MOST cost-effectively?

- [ ] A. Save the .pdf files to Amazon S3. Configure an S3 PUT event to invoke an AWS Lambda function to convert the files to .jpg format and store them back in Amazon S3.
- [ ] B. Save the .pdf files to Amazon DynamoDB. Use the DynamoDB Streams feature to invoke an AWS Lambda function to convert the files to .jpg format and store them back in DynamoDB.
- [ ] C. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic Block Store (Amazon EBS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the files to .jpg format. Save the .pdf files and the .jpg files in the EBS store.
- [ ] D. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic File System (Amazon EFS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the file to .jpg format. Save the .pdf files and the .jpg files in the EBS store.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Save the .pdf files to Amazon S3. Configure an S3 PUT event to invoke an AWS Lambda function to convert the files to .jpg format and store them back in Amazon S3.

Why this is the correct answer:

- [ ] Amazon S3 for Scalable and Cost-Effective Storage: Amazon S3 is the ideal service for storing both the original .pdf files and the converted .jpg files. S3 offers virtually unlimited scalability, high durability, and is very cost-effective for storing files of any size.
- [ ] S3 Event Triggers for Automation: When a new .pdf file is uploaded to S3 (a PUT event), S3 can be configured to automatically trigger an AWS Lambda function. This provides an event-driven architecture.
- [ ] AWS Lambda for Serverless Conversion: AWS Lambda is a serverless compute service that can execute code in response to events. A Lambda function can be written to take the S3 object key of the uploaded .pdf, perform the conversion to .jpg format (using appropriate libraries), and then store the resulting .jpg file back into an S3 bucket (possibly a different bucket or prefix).
- [ ] Scalability and Cost-Effectiveness: This serverless approach is highly scalable. Lambda automatically scales based on the number of incoming S3 events (uploads). You only pay for the S3 storage used and the Lambda compute time consumed during the conversion. This is generally the MOST cost-effective solution for event-driven file processing, especially when demand can grow rapidly, as you don't pay for idle compute resources. It also has minimal operational overhead.

Why are the other answers wrong?

- [ ] Option B is wrong because: Amazon DynamoDB is a NoSQL key-value and document database. While it can store small binary objects (up to 400 KB per item), it is not designed or cost-effective for storing 5 MB .pdf files directly. S3 is the appropriate service for this type of file storage.
- [ ] Option C is wrong because: Using AWS Elastic Beanstalk with Amazon EC2 instances to perform the conversion introduces the overhead of managing servers, even with Elastic Beanstalk's automation. This solution is less cost-effective than Lambda because you pay for EC2 instances even when no files are being processed. Storing files on Amazon EBS volumes is also less scalable and durable for this use case compared to S3, especially if you need to share or access these files from multiple places or ensure their long-term persistence independently of EC2 instances.
- [ ] Option D is wrong because: Similar to option C, this relies on EC2 instances managed by Elastic Beanstalk, which is less cost-effective and has higher operational overhead than a serverless Lambda approach for this task. While Amazon EFS provides shared file storage, S3 is generally more cost-effective and scalable for storing large numbers of original and converted image files. The option also confusingly mentions saving files to EBS store at the end, despite mentioning EFS.

</details>

<details>
  <summary>Question 64</summary>

A company has more than 5 TB of file data on Windows file servers that run on premises. Users and applications interact with the data each day.

The company is moving its Windows workloads to AWS. As the company continues this process, the company requires access to AWS and on-premises file storage with minimum latency. The company needs a solution that minimizes operational overhead and requires no significant changes to the existing file access patterns. The company uses an AWS Site-to-Site VPN connection for connectivity to AWS.

What should a solutions architect do to meet these requirements?

- [ ] A. Deploy and configure Amazon FSx for Windows File Server on AWS. Move the on-premises file data to FSx for Windows File Server. Reconfigure the workloads to use FSx for Windows File Server on AWS.
- [ ] B. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to the S3 File Gateway. Reconfigure the on-premises workloads and the cloud workloads to use the S3 File Gateway.
- [ ] C. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to Amazon S3. Reconfigure the workloads to use either Amazon S3 directly or the S3 File Gateway. depending on each workload's location.
- [ ] D. Deploy and configure Amazon FSx for Windows File Server on AWS. Deploy and configure an Amazon FSx File Gateway on premises. Move the on-premises file data to the FSx File Gateway. Configure the cloud workloads to use FSx for Windows File Server on AWS. Configure the on-premises workloads to use the FSx File Gateway.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Deploy and configure Amazon FSx for Windows File Server on AWS. Deploy and configure an Amazon FSx File Gateway on premises. Move the on-premises file data to the FSx File Gateway. Configure the cloud workloads to use FSx for Windows File Server on AWS. Configure the on-premises workloads to use the FSx File Gateway.

Why this is the correct answer:

- [ ] Hybrid Access with Minimized Latency: The core requirement is to provide low-latency access to file storage for both on-premises users/applications and new workloads being moved to AWS during the migration process.
- [ ] Amazon FSx for Windows File Server in AWS: This provides a fully managed, native Windows file system in the AWS Cloud, suitable for the Windows workloads being migrated. It supports SMB, Active Directory integration, and other Windows-native features.
- [ ] Amazon FSx File Gateway On-Premises: The Amazon FSx File Gateway (a specific type of AWS Storage Gateway) is deployed on-premises. It provides low-latency access for on-premises users and applications to their file data that is stored and managed in Amazon FSx for Windows File Server in the cloud. It does this by caching frequently accessed data locally on the gateway.
- [ ] Preserves Access Patterns: On-premises users continue to access files via SMB through the FSx File Gateway, and cloud workloads access the data directly from FSx for Windows File Server via SMB. This requires "no significant changes to the existing file access patterns."
- [ ] Minimizes Operational Overhead: Both FSx for Windows File Server and FSx File Gateway are managed services, reducing the operational burden on the company.

Why are the other answers wrong?

- [ ] Option A is wrong because: This option describes a full migration of data and workloads to FSx for Windows File Server on AWS. It doesn't address the requirement for on-premises workloads to maintain low-latency access to the file storage during the transition period. It's a destination state, not a hybrid solution for the migration process.
- [ ] Option B is wrong because: An Amazon S3 File Gateway provides an SMB interface to data stored in Amazon S3. While it can offer on-premises caching, Amazon FSx for Windows File Server (as in option D) provides a richer set of Windows-native file system features (like DFS Namespaces, fine-grained ACLs, shadow copies) that are often critical for Windows workloads. The FSx File Gateway is specifically optimized to work with FSx for Windows File Server, making it a more tailored solution for this scenario.
- [ ] Option C is wrong because: This also uses an S3 File Gateway. Requiring workloads to use Amazon S3 directly (for some workloads) would change the file access pattern from SMB to S3 API calls, which contradicts the requirement of "no significant changes to the existing file access patterns."

</details>

<details>
  <summary>Question 65</summary>

A hospital recently deployed a RESTful API with Amazon API Gateway and AWS Lambda. The hospital uses API Gateway and Lambda to upload reports that are in PDF format and JPEG format. The hospital needs to modify the Lambda code to identify protected health information (PHI) in the reports.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Use existing Python libraries to extract the text from the reports and to identify the PHI from the extracted text.
- [ ] B. Use Amazon Textract to extract the text from the reports. Use Amazon SageMaker to identify the PHI from the extracted text.
- [ ] C. Use Amazon Textract to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.
- [ ] D. Use Amazon Rekognition to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use Amazon Textract to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.

Why this is the correct answer:

- [ ] Amazon Textract for Text Extraction: Amazon Textract is a machine learning (ML) service that automatically extracts text, handwriting, and data from scanned documents, PDFs, and images (like JPEGs). This is ideal for getting the textual content out of the "PDF format and JPEG format" reports.   
- [ ] Amazon Comprehend Medical for PHI Identification: Amazon Comprehend Medical is a HIPAA-eligible natural language processing (NLP) service that uses ML to extract relevant medical information, including Protected Health Information (PHI), from unstructured text. It is specifically trained for medical text and can identify entities like PHI, medical conditions, medications, etc. This directly addresses the need to "identify protected health information (PHI) in the reports."
- [ ] Least Operational Overhead: Using these two fully managed AWS AI services (Textract and Comprehend Medical) within the Lambda function requires significantly less development effort and ongoing operational overhead compared to building custom solutions. There's no need to train custom models or manage underlying infrastructure for these tasks.

Why are the other answers wrong?

- [ ] Option A is wrong because: Relying on existing Python libraries for both text extraction (from potentially complex PDFs and image-based JPEGs which would require OCR) and accurate PHI identification would involve significant development effort, library selection, integration, testing, and maintenance. Identifying PHI accurately with general-purpose libraries can be challenging. This approach does not offer the "LEAST operational overhead."
- [ ] Option B is wrong because: While Amazon Textract is appropriate for text extraction, Amazon SageMaker is a platform for building, training, and deploying custom machine learning models. Using SageMaker to identify PHI would mean the hospital needs to develop, train, and manage its own ML model for this purpose. This is a complex and time-consuming process with high development and operational overhead, directly contradicting the "LEAST operational overhead" requirement.
- [ ] Option D is wrong because: While Amazon Rekognition can detect text in images (and would work for JPEGs), Amazon Textract is generally more specialized and robust for extracting text from document formats like PDFs and JPEGs containing forms or dense text. Amazon Comprehend Medical is the correct choice for PHI identification. The combination of Textract and Comprehend Medical (as in option C) is more specifically tailored for extracting text from documents and then identifying medical information.

</details>

<details>
  <summary>Question 66</summary>

A company has an application that generates a large number of files, each approximately 5 MB in size. The files are stored in Amazon S3. Company policy requires the files to be stored for 4 years before they can be deleted. Immediate accessibility is always required as the files contain critical business data that is not easy to reproduce. The files are frequently accessed in the first 30 days of the object creation but are rarely accessed after the first 30 days.

Which storage solution is MOST cost-effective?

- [ ] A. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Glacier 30 days from object creation. Delete the files 4 years after object creation.
- [ ] B. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) 30 days from object creation. Delete the files 4 years after object creation.
- [ ] C. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Delete the files 4 years after object creation.
- [ ] D. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Move the files to S3 Glacier 4 years after object creation.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Delete the files 4 years after object creation.

Why this is the correct answer:

- [ ] Initial Storage in S3 Standard: For the first 30 days, when files are "frequently accessed," storing them in Amazon S3 Standard is appropriate as it offers low latency and high throughput performance for frequently accessed data.
- [ ] Transition to S3 Standard-Infrequent Access (S3 Standard-IA): After 30 days, the files are "rarely accessed," but "Immediate accessibility is always required." S3 Standard-IA is designed for data that is accessed less frequently but requires millisecond access when needed. It offers a lower storage cost than S3 Standard, making it cost-effective for this phase. It also maintains high durability by storing data across multiple Availability Zones.
- [ ] Lifecycle Policy for Automation: An S3 Lifecycle policy can be configured to automatically transition objects from S3 Standard to S3 Standard-IA after 30 days. The same policy can also be configured to delete the objects after 4 years, meeting the company's retention policy.
- [ ] Cost-Effectiveness and Requirements Met: This solution balances the need for immediate accessibility throughout the 4-year retention period with cost optimization by moving data to a lower-cost tier after the initial frequent access period.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon S3 Glacier (referring to S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive) is designed for archiving, and data retrieval typically takes minutes to hours. This does not meet the requirement that "Immediate accessibility is always required" for the files throughout their 4-year lifecycle. (Note: S3 Glacier Instant Retrieval offers immediate access from an archive tier, but S3 Standard-IA is generally more suitable for infrequently accessed data that isn't cold enough for a dedicated archive tier if immediate access is paramount and access is just infrequent, not archival-rare).
- [ ] Option B is wrong because: S3 One Zone-Infrequent Access (S3 One Zone-IA) stores data in only a single Availability Zone. While it is a lower-cost option, it is not suitable for "critical business data that is not easy to reproduce" because if that single Availability Zone fails, the data would be lost. This does not provide the necessary durability for critical data.
- [ ] Option D is wrong because: The company policy requires the files to be "deleted" after 4 years, not moved to another storage tier like S3 Glacier. This option introduces an unnecessary archival step instead of deletion after the 4-year retention period.

</details>

<details>
  <summary>Question 67</summary>

A company hosts an application on multiple Amazon EC2 instances. The application processes messages from an Amazon SQS queue, writes to an Amazon RDS table, and deletes the message from the queue. Occasional duplicate records are found in the RDS table. The SQS queue does not contain any duplicate messages.

What should a solutions architect do to ensure messages are being processed once only?

- [ ] A. Use the CreateQueue API call to create a new queue.
- [ ] B. Use the AddPermission API call to add appropriate permissions.
- [ ] C. Use the ReceiveMessage API call to set an appropriate wait time.
- [ ] D. Use the ChangeMessageVisibility API call to increase the visibility timeout.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Use the ChangeMessageVisibility API call to increase the visibility timeout.

Why this is the correct answer:

- [ ] Understanding SQS Message Lifecycle and Visibility Timeout: When a consumer (in this case, an EC2 instance) retrieves a message from an SQS queue, the message isn't immediately deleted. Instead, it becomes "invisible" for a period called the visibility timeout. During this time, other consumers cannot see or process this message. The consumer is expected to process the message and then explicitly delete it from the queue.
- [ ] Cause of Duplicate Processing: If the application takes longer to process the message (write to RDS and then delete from SQS) than the configured visibility timeout, the message will become visible again in the queue. Another EC2 instance (or even the same instance if it retries) might then pick up this "reappeared" message and process it again, leading to duplicate records in the RDS table. The problem states the queue itself doesn't have duplicates, so the issue is with the processing.
- [ ] Increasing Visibility Timeout: By using the ChangeMessageVisibility API call (or configuring it on the queue settings) to increase the visibility timeout to a value that is safely longer than the maximum expected time to process and delete a message, you ensure that a message does not become visible again prematurely. This gives the initial processing instance enough time to complete its work and delete the message, preventing other instances from picking it up and causing duplicate processing.

Why are the other answers wrong?

- [ ] Option A is wrong because: Creating a new queue does not solve the underlying problem of how messages are being processed from the existing queue. The issue is not with the queue itself containing duplicates but with the processing logic leading to messages being processed more than once.
- [ ] Option B is wrong because: The AddPermission API call is used to grant other AWS accounts or services permissions to use the SQS queue. Incorrect permissions would likely lead to an inability to process messages or access denied errors, rather than causing duplicate processing of successfully retrieved messages.
- [ ] Option C is wrong because: Setting an appropriate wait time for the ReceiveMessage API call enables long polling. Long polling reduces the number of empty responses when polling the queue and can make message retrieval more efficient and cost-effective. However, it does not directly prevent a message from being processed multiple times if the visibility timeout is too short for the actual processing duration.

</details>

<details>
  <summary>Question 68</summary>
 
A solutions architect is designing a new hybrid architecture to extend a company's on-premises infrastructure to AWS. The company requires a highly available connection with consistent low latency to an AWS Region. The company needs to minimize costs and is willing to accept slower traffic if the primary connection fails.

What should the solutions architect do to meet these requirements?

- [ ] A. Provision an AWS Direct Connect connection to a Region. Provision a VPN connection as a backup if the primary Direct Connect connection fails.
- [ ] B. Provision a VPN tunnel connection to a Region for private connectivity. Provision a second VPN tunnel for private connectivity and as a backup if the primary VPN connection fails.
- [ ] C. Provision an AWS Direct Connect connection to a Region. Provision a second Direct Connect connection to the same Region as a backup if the primary Direct Connect connection fails.
- [ ] D. Provision an AWS Direct Connect connection to a Region. Use the Direct Connect failover attribute from the AWS CLI to automatically create a backup connection if the primary Direct Connect connection fails.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Provision an AWS Direct Connect connection to a Region. Provision a VPN connection as a backup if the primary Direct Connect connection fails.

Why this is the correct answer:

- [ ] AWS Direct Connect for Primary, Low-Latency Connection: AWS Direct Connect provides a dedicated physical network connection from your on-premises environment to AWS. This is ideal for achieving "consistent low latency" and can be configured for high availability (e.g., using redundant connections or connections to different Direct Connect locations).
- [ ] VPN as a Cost-Effective Backup: An AWS Site-to-Site VPN connection routes traffic over the public internet. It is a more cost-effective option than a second Direct Connect connection and can serve as a reliable backup.
- [ ] Meeting Cost and Failover Requirements: The company "needs to minimize costs and is willing to accept slower traffic if the primary connection fails." Using a VPN as a backup to Direct Connect aligns with this. The VPN will typically have higher latency and lower throughput than Direct Connect, but it provides a failover path at a lower cost than a redundant Direct Connect link. Failover from Direct Connect to VPN can be configured using dynamic routing (BGP).

Why are the other answers wrong?

- [ ] Option B is wrong because: While using two VPN tunnels can provide redundancy, VPN connections rely on the public internet and may not consistently provide the "consistent low latency" required for the primary connection, especially compared to what Direct Connect offers.
- [ ] Option C is wrong because: Provisioning a second AWS Direct Connect connection for backup provides the highest level of availability and consistent performance for the backup path. However, it is also the most expensive option for a backup connection. This contradicts the requirement to "minimize costs," especially given the company's willingness to accept slower traffic on failover.
- [ ] Option D is wrong because: There is no such thing as a "Direct Connect failover attribute from the AWS CLI to automatically create a backup connection." Failover mechanisms for Direct Connect involve network routing configurations (typically using BGP) to a pre-provisioned backup path (like another Direct Connect connection or a VPN). The backup connection needs to exist beforehand; it's not automatically created upon failure by a CLI attribute.

</details>

<details>
  <summary>Question 69</summary>

A company is running a business-critical web application on Amazon EC2 instances behind an Application Load Balancer. The EC2 instances are in an Auto Scaling group. The application uses an Amazon Aurora PostgreSQL database that is deployed in a single Availability Zone. The company wants the application to be highly available with minimum downtime and minimum loss of data.

Which solution will meet these requirements with the LEAST operational effort?

- [ ] A. Place the EC2 instances in different AWS Regions. Use Amazon Route 53 health checks to redirect traffic. Use Aurora PostgreSQL Cross-Region Replication.
- [ ] B. Configure the Auto Scaling group to use multiple Availability Zones. Configure the database as Multi-AZ. Configure an Amazon RDS Proxy instance for the database.
- [ ] C. Configure the Auto Scaling group to use one Availability Zone. Generate hourly snapshots of the database. Recover the database from the snapshots in the event of a failure.
- [ ] D. Configure the Auto Scaling group to use multiple AWS Regions. Write the data from the application to Amazon S3. Use S3 Event Notifications to launch an AWS Lambda function to write the data to the database.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Configure the Auto Scaling group to use multiple Availability Zones. Configure the database as Multi-AZ. Configure an Amazon RDS Proxy instance for the database.

Why this is the correct answer:

- [ ] Auto Scaling Group Across Multiple Availability Zones: Distributing the EC2 instances across multiple Availability Zones (AZs) within the same region ensures that the application tier remains available even if one AZ experiences an outage. The Application Load Balancer will route traffic to healthy instances in the available AZs.
- [ ] Aurora PostgreSQL Multi-AZ Deployment: The current database is in a single AZ, which is a single point of failure. Configuring the Amazon Aurora PostgreSQL database as a Multi-AZ deployment is crucial for high availability. Aurora automatically provisions and maintains a synchronous standby replica in a different AZ. In case of a primary database failure or an AZ outage, Aurora performs an automatic failover to the standby replica, typically within minutes, thus minimizing downtime and ensuring minimum data loss (RPO is near zero for synchronous replication).
- [ ] Amazon RDS Proxy: Amazon RDS Proxy is a fully managed, highly available database proxy that sits between your application and your RDS/Aurora database. It helps improve application resilience by managing and sharing database connections. During a database failover (like an Aurora Multi-AZ failover), RDS Proxy can maintain open application connections and seamlessly connect to the newly promoted database instance, reducing the impact of failovers on the application and often leading to faster recovery times for the application. This further contributes to "minimum downtime."
- [ ] Least Operational Effort: This solution leverages managed AWS services (Auto Scaling, Aurora Multi-AZ, RDS Proxy) that automate many of the complexities associated with achieving high availability, requiring the least operational effort from the company.

Why are the other answers wrong?

- [ ] Option A is wrong because: While deploying across multiple AWS Regions with Cross-Region Replication provides disaster recovery capabilities, it is significantly more complex and involves higher operational effort than a Multi-AZ deployment within a single region for achieving high availability. Aurora Cross-Region Replication is asynchronous, which means there could be some data loss (RPO > 0) in a failover scenario. This is more of a DR strategy than a primary HA strategy aimed at "minimum downtime and minimum loss of data" which is first typically addressed by Multi-AZ.
- [ ] Option C is wrong because: Configuring the Auto Scaling group in a single Availability Zone makes the application tier vulnerable to an AZ failure. Generating hourly snapshots and recovering from them means a potential data loss of up to one hour (RPO = 1 hour) and a recovery time (RTO) that would likely be much longer than "minimum downtime" allows for a business-critical application.
- [ ] Option D is wrong because: Deploying across multiple AWS Regions for the Auto Scaling group is complex for high availability. Using Amazon S3 as an intermediary data store before writing to the database introduces unnecessary latency, complexity, and potential points of failure in the critical data path for a transactional application. This is not a standard or efficient pattern for achieving high database availability.

</details>

<details>
  <summary>Question 70</summary>

A company's HTTP application is behind a Network Load Balancer (NLB). The NLB's target group is configured to use an Amazon EC2 Auto Scaling group with multiple EC2 instances that run the web service. The company notices that the NLB is not detecting HTTP errors for the application. These errors require a manual restart of the EC2 instances that run the web service. The company needs to improve the application's availability without writing custom scripts or code.

What should a solutions architect do to meet these requirements?

- [ ] A. Enable HTTP health checks on the NLB, supplying the URL of the company's application.
- [ ] B. Add a cron job to the EC2 instances to check the local application's logs once each minute. If HTTP errors are detected. the application will restart.
- [ ] C. Replace the NLB with an Application Load Balancer. Enable HTTP health checks by supplying the URL of the company's application. Configure an Auto Scaling action to replace unhealthy instances.
- [ ] D. Create an Amazon CloudWatch alarm that monitors the UnhealthyHostCount metric for the NLB. Configure an Auto Scaling action to replace unhealthy instances when the alarm is in the ALARM state.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Replace the NLB with an Application Load Balancer. Enable HTTP health checks by supplying the URL of the company's application. Configure an Auto Scaling action to replace unhealthy instances.

Why this is the correct answer:

- [ ] Application-Level Health Checks with ALB: The problem states that the Network Load Balancer (NLB) is not detecting HTTP errors. NLBs operate at Layer 4 (transport layer) and perform health checks at the TCP level or basic HTTP checks. Application Load Balancers (ALBs), however, operate at Layer 7 (application layer) and can perform more sophisticated HTTP/HTTPS health checks. ALBs can check for specific HTTP status codes (e.g., expecting a 200 OK from a specific path) and can more reliably detect application-level errors. Replacing the NLB with an ALB allows for proper detection of HTTP errors.
- [ ] Auto Scaling Based on ALB Health Checks: Once an ALB is in place with appropriate HTTP health checks, it will mark instances as unhealthy if they return HTTP errors. The EC2 Auto Scaling group can then be configured to use these ELB (ALB) health checks. When an instance is marked unhealthy by the ALB, the Auto Scaling group can automatically terminate that instance and launch a new one to replace it. This improves application availability by automating the recovery from failing instances.
- [ ] No Custom Scripts or Code: This solution uses built-in AWS features (ALB health checks and Auto Scaling group actions) and does not require writing any custom scripts or code for health detection or instance remediation.

Why are the other answers wrong?

- [ ] Option A is wrong because: While NLBs do offer HTTP/HTTPS health checks, they are generally less granular than ALB health checks for detecting application-specific HTTP errors. If the NLB is currently "not detecting HTTP errors," simply enabling or tweaking its existing HTTP health checks might not be sufficient if the issue lies in the depth of checking required, which ALBs handle better.
- [ ] Option B is wrong because: Adding a cron job to check logs and restart the application involves writing custom scripts, which directly contradicts the requirement to achieve the solution "without writing custom scripts or code."
- [ ] Option D is wrong because: If the NLB's health checks are not correctly identifying the instances as unhealthy when they are returning HTTP errors (as stated in the problem: "NLB is not detecting HTTP errors"), then the UnhealthyHostCount metric for the NLB will not be accurate. An alarm based on this potentially inaccurate metric would not reliably trigger the Auto Scaling action to replace the genuinely unhealthy instances. The root cause of the health check inadequacy needs to be addressed first.

</details>

</details>

<details>
  <summary>==Questions 71-80==</summary>

<details>
  <summary>Question 71</summary>

A company runs a shopping application that uses Amazon DynamoDB to store customer information. In case of data corruption, a solutions architect needs to design a solution that meets a recovery point objective (RPO) of 15 minutes and a recovery time objective (RTO) of 1 hour.

What should the solutions architect recommend to meet these requirements?

- [ ] A. Configure DynamoDB global tables. For RPO recovery, point the application to a different AWS Region.
- [ ] B. Configure DynamoDB point-in-time recovery. For RPO recovery, restore to the desired point in time.
- [ ] C. Export the DynamoDB data to Amazon S3 Glacier on a daily basis. For RPO recovery, import the data from S3 Glacier to DynamoDB.
- [ ] D. Schedule Amazon Elastic Block Store (Amazon EBS) snapshots for the DynamoDB table every 15 minutes. For RPO recovery, restore the DynamoDB table by using the EBS snapshot.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Configure DynamoDB point-in-time recovery. For RPO recovery, restore to the desired point in time.

Why this is the correct answer:

- [ ] DynamoDB Point-in-Time Recovery (PITR): Amazon DynamoDB point-in-time recovery (PITR) provides continuous backups of your table data. When enabled, DynamoDB backs up your data with per-second granularity, and you can restore your table to any single second during the preceding 35 days.
- [ ] Meeting RPO of 15 minutes: Since PITR allows restoration to any second within the retention period, it can easily meet a Recovery Point Objective (RPO) of 15 minutes. If data corruption occurs, you can restore the table to a state just moments before the corruption event.
- [ ] Meeting RTO of 1 hour: Restoring a DynamoDB table using PITR creates a new table with the restored data. The time it takes to restore a table depends on its size and other factors, but for many common table sizes, the restoration process can be completed well within a 1-hour Recovery Time Objective (RTO).
- [ ] Designed for Data Corruption: PITR is specifically designed to protect against accidental writes or deletes (i.e., data corruption) by allowing you to revert the table to a previous state.

Why are the other answers wrong?

- [ ] Option A is wrong because: DynamoDB global tables provide a multi-region, multi-active database solution, primarily for disaster recovery from regional outages and for providing low-latency access to globally distributed users. If data corruption occurs in one region, that corrupted data will replicate to the other regions. Global tables do not inherently protect against logical data corruption within the table itself by allowing a rollback to a previous point in time.
- [ ] Option C is wrong because: Exporting DynamoDB data to Amazon S3 Glacier on a daily basis would result in an RPO of up to 24 hours, which does not meet the 15-minute RPO requirement. Additionally, retrieving data from S3 Glacier typically takes minutes to hours, and then importing it back into DynamoDB would add further to the recovery time, likely exceeding the 1-hour RTO.
- [ ] Option D is wrong because: Amazon DynamoDB is a fully managed NoSQL database service. Users do not directly manage its underlying storage with Amazon EBS volumes in a way that would allow them to take EBS snapshots. DynamoDB has its own native backup and restore mechanisms, such as on-demand backups and point-in-time recovery. This option describes an incorrect method for backing up DynamoDB.

</details>  

<details>
  <summary>Question 72</summary>
 
A company runs a photo processing application that needs to frequently upload and download pictures from Amazon S3 buckets that are located in the same AWS Region. A solutions architect has noticed an increased cost in data transfer fees and needs to implement a solution to reduce these costs.

How can the solutions architect meet this requirement?

- [ ] A. Deploy Amazon API Gateway into a public subnet and adjust the route table to route S3 calls through it.
- [ ] B. Deploy a NAT gateway into a public subnet and attach an endpoint policy that allows access to the S3 buckets.
- [ ] C. Deploy the application into a public subnet and allow it to route through an internet gateway to access the S3 buckets.
- [ ] D. Deploy an S3 VPC gateway endpoint into the VPC and attach an endpoint policy that allows access to the S3 buckets.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Deploy an S3 VPC gateway endpoint into the VPC and attach an endpoint policy that allows access to the S3 buckets.

Why this is the correct answer:

- [ ] Reducing S3 Data Transfer Costs within a VPC: When EC2 instances (or other resources within a VPC) access Amazon S3 in the same region, traffic might route through a NAT Gateway or an Internet Gateway if a private connection isn't established. This can lead to data transfer costs (specifically, NAT Gateway data processing charges).
- [ ] S3 VPC Gateway Endpoint: An S3 VPC gateway endpoint enables instances in your VPC to use their private IP addresses to access Amazon S3 directly, without needing an Internet Gateway, NAT device, VPN connection, or AWS Direct Connect connection. Traffic between your VPC and S3 does not leave the Amazon network.   
- [ ] Cost Savings: Crucially, data transferred between your EC2 instances and S3 through a gateway VPC endpoint within the same AWS Region does not incur data transfer charges or NAT gateway processing charges. This directly addresses the requirement to "reduce these costs."
Endpoint Policy for Security: An endpoint policy can be attached to the S3 gateway endpoint to control access to S3 buckets from your VPC, providing an additional layer of security.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon API Gateway is a service for creating, managing, and securing APIs. Using it as a proxy for S3 calls primarily to reduce data transfer costs for an application that directly interacts with S3 for uploads/downloads is an overly complex and inappropriate solution. It would likely introduce its own costs and latency.
- [ ] Option B is wrong because: If the application instances are in private subnets, they might already be using a NAT gateway to access S3 (which is causing the data transfer fees). Deploying or continuing to use a NAT gateway for S3 access will incur data processing charges for traffic passing through it. A VPC gateway endpoint for S3 bypasses the NAT gateway for S3 traffic, thus saving those costs.
- [ ] Option C is wrong because: If application instances are in a public subnet and use an Internet Gateway to access S3, this avoids NAT gateway charges. However, using a gateway VPC endpoint (Option D) is still the most direct and recommended way to ensure private connectivity to S3 and eliminate any potential data transfer costs for S3 access within the same region. It also allows instances to reside in private subnets for better security, while still accessing S3 cost-effectively.

</details>

<details>
  <summary>Question 73</summary>

A company recently launched Linux-based application instances on Amazon EC2 in a private subnet and launched a Linux-based bastion host on an Amazon EC2 instance in a public subnet of a VPC. A solutions architect needs to connect from the on-premises network, through the company's internet connection, to the bastion host, and to the application servers. The solutions architect must make sure that the security groups of all the EC2 instances will allow that access.

Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

- [ ] A. Replace the current security group of the bastion host with one that only allows inbound access from the application instances.
- [ ] B. Replace the current security group of the bastion host with one that only allows inbound access from the internal IP range for the company.
- [ ] C. Replace the current security group of the bastion host with one that only allows inbound access from the external IP range for the company.
- [ ] D. Replace the current security group of the application instances with one that allows inbound SSH access from only the private IP address of the bastion host.
- [ ] E. Replace the current security group of the application instances with one that allows inbound SSH access from only the public IP address of the bastion host.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Replace the current security group of the bastion host with one that only allows inbound access from the external IP range for the company.
- [ ] D. Replace the current security group of the application instances with one that allows inbound SSH access from only the private IP address of the bastion host.

Why these are the correct answers:

This question describes a typical bastion host setup for secure SSH access to instances in private subnets.

C. Replace the current security group of the bastion host with one that only allows inbound access from the external IP range for the company.
- [ ] Bastion Host Security: The bastion host is in a public subnet and serves as the entry point from the internet (or an on-premises network via the internet) into the VPC.
- [ ] Its security group should be configured to allow inbound SSH traffic (typically on port 22 for Linux) only from the known public IP address range of the company's on-premises network.
- [ ] This restricts access to the bastion host itself, preventing unauthorized connection attempts from the wider internet.

D. Replace the current security group of the application instances with one that allows inbound SSH access from only the private IP address of the bastion host.
- [ ] Application Instance Security: The application instances are in a private subnet and should not be directly accessible from the internet.
- [ ] Access to these instances should be routed through the bastion host. Therefore, the security group for the application instances should allow inbound SSH traffic (port 22) only from the bastion host.
- [ ] This is best achieved by specifying the private IP address of the bastion host as the source in the inbound rule (or, even more securely and flexibly, by referencing the security group ID of the bastion host, though that specific wording isn't an option here, the private IP achieves the same goal from a specific instance).

Why are the other answers wrong?

- [ ] Option A is wrong because: The bastion host is the entry point; it does not need inbound access from the application instances it is designed to protect. Traffic flows from the on-premises network to the bastion, then from the bastion to the application instances.
- [ ] Option B is wrong because: "Internal IP range for the company" usually refers to the company's on-premises private IP addresses. These private IPs are not routable over the public internet to reach the bastion host's public IP. The bastion host's security group needs to allow traffic from the company's external (public) IP range.
- [ ] Option E is wrong because: When the bastion host initiates an SSH connection to an application instance in a private subnet within the same VPC, the source IP address of that connection will be the bastion host's private IP address, not its public IP address. Therefore, the application instances' security group must allow inbound SSH from the bastion's private IP.

</details>

<details>
  <summary>Question 74</summary>

A solutions architect is designing a two-tier web application. The application consists of a public-facing web tier hosted on Amazon EC2 in public subnets. The database tier consists of Microsoft SQL Server running on Amazon EC2 in a private subnet. Security is a high priority for the company.

How should security groups be configured in this situation? (Choose two.)

- [ ] A. Configure the security group for the web tier to allow inbound traffic on port 443 from 0.0.0.0/0.
- [ ] B. Configure the security group for the web tier to allow outbound traffic on port 443 from 0.0.0.0/0.
- [ ] C. Configure the security group for the database tier to allow inbound traffic on port 1433 from the security group for the web tier.
- [ ] D. Configure the security group for the database tier to allow outbound traffic on ports 443 and 1433 to the security group for the web tier.
- [ ] E. Configure the security group for the database tier to allow inbound traffic on ports 443 and 1433 from the security group for the web tier.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Configure the security group for the web tier to allow inbound traffic on port 443 from 0.0.0.0/0.
- [ ] C. Configure the security group for the database tier to allow inbound traffic on port 1433 from the security group for the web tier.

Why these are the correct answers:

This setup describes a standard best practice for securing a two-tier web application using security groups:

A. Configure the security group for the web tier to allow inbound traffic on port 443 from 0.0.0.0/0.
- [ ] Public Access to Web Tier: The web tier is public-facing and needs to accept incoming HTTPS traffic from users on the internet.
- [ ] Port 443 is the standard port for HTTPS. 0.0.0.0/0 represents any IP address, allowing global access to the web application over HTTPS. (Often, HTTP on port 80 would also be allowed, typically with a redirect to HTTPS, but allowing 443 from anywhere is essential for public web access).

C. Configure the security group for the database tier to allow inbound traffic on port 1433 from the security group for the web tier.
- [ ] Restricted Access to Database Tier: The database tier is in a private subnet and should only be accessible by the application's web tier.
- [ ] Microsoft SQL Server typically listens on port 1433.
- [ ] Security Group Referencing: A security best practice is to configure the database tier's security group to allow inbound traffic on port 1433 only from the security group that is attached to the web tier EC2 instances.
- [ ] This ensures that only the web servers can initiate connections to the database, providing tight network isolation.

Why are the other answers wrong?

- [ ] Option B is wrong because: Security groups are stateful. If inbound traffic on port 443 is allowed (as in option A), the corresponding outbound response traffic is automatically allowed. Outbound rules are typically configured for traffic initiated by the instances in the security group. Specifying a source IP (0.0.0.0/0) for an outbound rule in this context is not how it's typically defined; you define the destination and port for outbound traffic.
- [ ] Option D is wrong because: The primary concern for connectivity between the web and database tiers is allowing the web tier to initiate connections to the database tier. While the database tier will send outbound traffic in response to these connections (which is statefully allowed), explicitly defining outbound rules from the database to the web tier's security group on ports 443 and 1433 is not the standard way to enable the required web-to-database communication. The critical rule is the inbound rule on the database security group.
- [ ] Option E is wrong because: The database tier (Microsoft SQL Server) listens for database connections on port 1433. It does not need to accept inbound traffic on port 443 (HTTPS) from the web tier for standard database communication. Port 443 is for web traffic.

</details>

<details>
  <summary>Question 75</summary>

A company wants to move a multi-tiered application from on premises to the AWS Cloud to improve the application's performance. The application consists of application tiers that communicate with each other by way of RESTful services. Transactions are dropped when one tier becomes overloaded. A solutions architect must design a solution that resolves these issues and modernizes the application.

Which solution meets these requirements and is the MOST operationally efficient?

- [ ] A. Use Amazon API Gateway and direct transactions to the AWS Lambda functions as the application layer. Use Amazon Simple Queue Service (Amazon SQS) as the communication layer between application services.
- [ ] B. Use Amazon CloudWatch metrics to analyze the application performance history to determine the servers' peak utilization during the performance failures. Increase the size of the application server's Amazon EC2 instances to meet the peak requirements.
- [ ] C. Use Amazon Simple Notification Service (Amazon SNS) to handle the messaging between application servers running on Amazon EC2 in an Auto Scaling group. Use Amazon CloudWatch to monitor the SNS queue length and scale up and down as required.
- [ ] D. Use Amazon Simple Queue Service (Amazon SQS) to handle the messaging between application servers running on Amazon EC2 in an Auto Scaling group. Use Amazon CloudWatch to monitor the SQS queue length and scale up when communication failures are detected.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Use Amazon API Gateway and direct transactions to the AWS Lambda functions as the application layer. Use Amazon Simple Queue Service (Amazon SQS) as the communication layer between application services.

Why this is the correct answer:

- [ ] Modernization with Serverless Components: This solution proposes a significant modernization by moving towards a serverless architecture.
- [ ] Amazon API Gateway and AWS Lambda: Using API Gateway to expose RESTful services and AWS Lambda functions to implement the application logic for each tier is highly scalable, resilient, and "operationally efficient." There are no servers to manage for this part of the application, as AWS handles the underlying infrastructure, scaling, and availability.
- [ ] Resolving Dropped Transactions with SQS: The problem states "Transactions are dropped when one tier becomes overloaded." Using Amazon Simple Queue Service (Amazon SQS) as the communication layer between these application services (implemented as Lambda functions or other components) provides a durable buffer. If a downstream service is overloaded, messages (transactions) can be reliably queued in SQS until the service can process them, preventing them from being dropped. This decouples the application tiers.
- [ ] Improved Performance and Scalability: API Gateway and Lambda can scale automatically to handle varying loads. SQS provides a scalable and reliable messaging backbone.
- [ ] MOST Operationally Efficient: A serverless architecture with managed services like API Gateway, Lambda, and SQS generally offers the highest operational efficiency because it minimizes infrastructure management tasks (provisioning, patching, scaling servers).

Why are the other answers wrong?

- [ ] Option B is wrong because: Simply increasing the size of EC2 instances (vertical scaling) is a traditional approach that might not be the most cost-effective or elastic solution for handling variable loads. It doesn't fundamentally "modernize" the application architecture or address the dropped transactions issue through decoupling in an optimal way. It can lead to over-provisioning and still doesn't prevent drops if synchronous calls between tiers remain the bottleneck.
- [ ] Option C is wrong because: While Amazon SNS is a messaging service, it's primarily a publish/subscribe system designed for fanning out messages to multiple subscribers. For buffering transactions between specific application tiers where processing order or guaranteed delivery to a worker pool is important, SQS is generally a more suitable choice. Also, SNS topics don't have a "queue length" in the same way SQS queues do for direct monitoring and scaling of worker tiers. This option still relies on EC2 instances, making it less operationally efficient than option A.
- [ ] Option D is wrong because: Using SQS between EC2-based application servers is an improvement over direct REST calls for preventing dropped transactions. However, option A offers a more complete modernization and higher operational efficiency by moving the application tiers themselves to serverless components (API Gateway and Lambda) in addition to using SQS for inter-service communication. Managing EC2 instances, even in an Auto Scaling group, involves more operational overhead than a fully serverless backend.

</details>

<details>
  <summary>Question 76</summary>

A company receives 10 TB of instrumentation data each day from several machines located at a single factory. The data consists of JSON files stored on a storage area network (SAN) in an on-premises data center located within the factory. The company wants to send this data to Amazon S3 where it can be accessed by several additional systems that provide critical near-real-time analytics. A secure transfer is important because the data is considered sensitive.

Which solution offers the MOST reliable data transfer?

- [ ] A. AWS DataSync over public internet
- [ ] B. AWS DataSync over AWS Direct Connect
- [ ] C. AWS Database Migration Service (AWS DMS) over public internet
- [ ] D. AWS Database Migration Service (AWS DMS) over AWS Direct Connect

</details>

<details>
  <summary>Answer</summary>

- [ ] B. AWS DataSync over AWS Direct Connect

Why this is the correct answer:

- [ ] AWS DataSync for File Transfer: AWS DataSync is a data transfer service specifically designed to simplify, automate, and accelerate moving large amounts of data between on-premises storage systems (like a SAN storing JSON files) and AWS storage services such as Amazon S3. It is well-suited for transferring 10 TB of data daily.
- [ ] AWS Direct Connect for Reliability and Security: AWS Direct Connect provides a dedicated, private network connection from your on-premises data center to AWS. Transferring data over Direct Connect offers:
- [ ] Reliability: More consistent network performance and higher throughput compared to the public internet, which is crucial for transferring large volumes of time-sensitive data reliably.
- [ ] Security: A private, dedicated path that does not traverse the public internet, enhancing security for sensitive data. DataSync encrypts data in transit, and using it over Direct Connect further secures the path.
- [ ] This combination addresses the need for a "secure transfer" and the "MOST reliable data transfer."
- [ ] Enabling Near-Real-Time Analytics: By efficiently and reliably transferring data to Amazon S3, downstream systems can then access this data for "critical near-real-time analytics."

Why are the other answers wrong?

- [ ] Option A is wrong because: While AWS DataSync is the correct service for the data transfer, sending 10 TB of sensitive data daily "over public internet" is generally less reliable (due to potential network congestion and variability) and less secure than using a dedicated connection like AWS Direct Connect.
- [ ] Option C is wrong because: AWS Database Migration Service (AWS DMS) is designed for migrating databases to or within AWS. It is not the appropriate tool for transferring JSON files from a storage area network (SAN) to Amazon S3. Using DMS over the public internet also shares the reliability and security concerns mentioned for option A.
- [ ] Option D is wrong because: As with option C, AWS Database Migration Service (AWS DMS) is not the correct tool for transferring file-based data from a SAN. The nature of the data (JSON files on a SAN) points to a file transfer solution like AWS DataSync.

</details>

<details>
  <summary>Question 77</summary>

A company needs to configure a real-time data ingestion architecture for its application. The company needs an API, a process that transforms data as the data is streamed, and a storage solution for the data.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Deploy an Amazon EC2 instance to host an API that sends data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.
- [ ] B. Deploy an Amazon EC2 instance to host an API that sends data to AWS Glue. Stop source/destination checking on the EC2 instance. Use AWS Glue to transform the data and to send the data to Amazon S3.
- [ ] C. Configure an Amazon API Gateway API to send data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.
- [ ] D. Configure an Amazon API Gateway API to send data to AWS Glue. Use AWS Lambda functions to transform the data. Use AWS Glue to send the data to Amazon S3.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Configure an Amazon API Gateway API to send data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.

Why this is the correct answer:

This solution provides a serverless and managed pipeline, minimizing operational overhead:

- [ ] API with Amazon API Gateway: Amazon API Gateway is a fully managed service that allows you to create, publish, maintain, monitor, and secure APIs at any scale. Using API Gateway for the API endpoint requires no server management.   
- [ ] Real-time Data Ingestion with Kinesis Data Streams: API Gateway can be configured to send the incoming data directly to an Amazon Kinesis data stream, which is designed for scalable and durable real-time data ingestion.
- [ ] Stream Transformation with Kinesis Data Firehose and Lambda: An Amazon Kinesis Data Firehose delivery stream can use the Kinesis data stream as its source.
- [ ] Firehose has a built-in feature to invoke an AWS Lambda function to perform data transformations on the records as they pass through.
- [ ] This meets the requirement to "transform data as the data is streamed."
- [ ] Storage with Amazon S3: Kinesis Data Firehose can then reliably deliver the transformed data to Amazon S3, which serves as a scalable and durable storage solution.
- [ ] Least Operational Overhead: This architecture primarily uses managed services (API Gateway, Kinesis Data Streams, Kinesis Data Firehose, Lambda, S3), which significantly reduces the need to provision, manage, or scale underlying infrastructure.

Why are the other answers wrong?

- [ ] Option A is wrong because: Deploying an Amazon EC2 instance to host the API introduces operational overhead for managing that instance (patching, scaling, availability). API Gateway (as in option C) is a serverless alternative that eliminates this overhead. The rest of the pipeline is similar to option C, but the EC2 component makes it less operationally efficient.
- [ ] Option B is wrong because: Similar to option A, using an EC2 instance for the API has higher operational overhead. Additionally, AWS Glue is primarily an ETL (extract, transform, load) service, generally used for batch or more complex streaming ETL jobs. While it can perform transformations, using Kinesis Data Firehose with Lambda for in-stream transformation of real-time API data is often a more lightweight and direct approach for "least operational overhead" in this specific scenario. Sending data directly from an API to AWS Glue for real-time streaming transformation is not as common or straightforward as using the Kinesis suite.
- [ ] Option D is wrong because: Sending data directly from API Gateway to AWS Glue for real-time data transformation is not a typical or directly supported high-throughput streaming pattern. Kinesis Data Streams and Kinesis Data Firehose are purpose-built for ingesting and processing streaming data from sources like API Gateway before it lands in a data store or is further processed by services like Glue for more complex ETL.


</details>

<details>
  <summary>Question 78</summary>

A company needs to keep user transaction data in an Amazon DynamoDB table. The company must retain the data for 7 years.

What is the MOST operationally efficient solution that meets these requirements?

- [ ] A. Use DynamoDB point-in-time recovery to back up the table continuously.
- [ ] B. Use AWS Backup to create backup schedules and retention policies for the table.
- [ ] C. Create an on-demand backup of the table by using the DynamoDB console. Store the backup in an Amazon S3 bucket. Set an S3 Lifecycle configuration for the S3 bucket.
- [ ] D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to invoke an AWS Lambda function. Configure the Lambda function to back up the table and to store the backup in an Amazon S3 bucket. Set an S3 Lifecycle configuration for the S3 bucket.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use AWS Backup to create backup schedules and retention policies for the table.

Why this is the correct answer:

- [ ] AWS Backup for Centralized and Automated Backups: AWS Backup is a fully managed backup service that makes it easy to centralize and automate data protection across AWS services, including Amazon DynamoDB.   
- [ ] Backup Schedules and Retention Policies: With AWS Backup, you can create backup plans that define automated backup schedules (e.g., daily, weekly) and, crucially, set retention policies.
- [ ] You can configure these retention policies to keep backups for extended periods, such as the required 7 years.
- [ ] AWS Backup will manage the lifecycle of these backups, including their expiration.
- [ ] MOST Operationally Efficient: Using AWS Backup is highly operationally efficient for long-term retention requirements. It automates the entire backup lifecycle, from creation to expiration, and provides a central place to manage and monitor backups. This avoids the need for manual processes or custom scripting.

Why are the other answers wrong?

- [ ] Option A is wrong because: DynamoDB point-in-time recovery (PITR) provides continuous backups and allows you to restore your table to any point in time within the last 35 days. While excellent for recovering from recent accidental writes or deletes, PITR itself is not designed for long-term retention beyond 35 days. It does not meet the 7-year retention requirement on its own.
- [ ] Option C is wrong because: Creating on-demand backups via the DynamoDB console and then manually managing their storage in Amazon S3 (including setting S3 Lifecycle configurations) involves manual steps and is less operationally efficient than an automated and centralized solution like AWS Backup. While DynamoDB backups are stored in S3, AWS Backup provides the management layer for scheduling and retention across services.
- [ ] Option D is wrong because: Building a custom solution using Amazon EventBridge to trigger an AWS Lambda function for backing up the table and managing its lifecycle in S3 requires custom development, testing, and ongoing maintenance. This is significantly more operational effort compared to using the managed capabilities of AWS Backup, which is designed for these tasks.

</details>

<details>
  <summary>Question 79</summary>

A company is planning to use an Amazon DynamoDB table for data storage. The company is concerned about cost optimization. The table will not be used on most mornings. In the evenings, the read and write traffic will often be unpredictable. When traffic spikes occur, they will happen very quickly.

What should a solutions architect recommend?

- [ ] A. Create a DynamoDB table in on-demand capacity mode.
- [ ] B. Create a DynamoDB table with a global secondary index.
- [ ] C. Create a DynamoDB table with provisioned capacity and auto scaling.
- [ ] D. Create a DynamoDB table in provisioned capacity mode, and configure it as a global table.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create a DynamoDB table in on-demand capacity mode.

Why this is the correct answer:

- [ ] DynamoDB On-Demand Capacity Mode: This capacity mode is specifically designed for workloads with unknown or unpredictable traffic patterns, and for applications where you want to pay only for what you use. DynamoDB on-demand automatically scales the read and write capacity to handle the traffic as it comes, without requiring manual capacity planning or adjustments.
- [ ] Handles Unpredictable and Spiky Traffic: The scenario describes traffic that is "often unpredictable" and where "traffic spikes occur, they will happen very quickly." On-demand mode excels in these situations by instantly accommodating the traffic up to previous peak levels (and then scaling further if needed), without throttling requests.
- [ ] Cost Optimization for Idle Periods: The table "will not be used on most mornings." With on-demand capacity mode, if there are no read or write requests to the table, you do not pay for any read/write throughput. This makes it very cost-effective for tables with significant idle periods.

Why are the other answers wrong?

- [ ] Option B is wrong because: A Global Secondary Index (GSI) is used to enable querying on attributes other than the table's primary key. While GSIs are an important feature for flexible data access, creating a GSI does not address the core requirements of managing unpredictable traffic patterns or optimizing costs related to read/write capacity.
- [ ] Option C is wrong because: While DynamoDB provisioned capacity with auto scaling can adjust throughput, it might not react instantaneously to "very quickly" occurring traffic spikes. Auto scaling typically adjusts capacity based on consumed capacity over a period, and there can be a delay before capacity is scaled up, potentially leading to throttling during sudden, sharp spikes. Furthermore, with provisioned capacity, you pay for the capacity you provision, even if it's not fully utilized during the idle morning periods, making it less cost-effective than on-demand for this specific usage pattern.
- [ ] Option D is wrong because: As explained for option C, provisioned capacity mode is less suitable for this workload pattern than on-demand mode.
DynamoDB global tables are designed for building multi-region, multi-active database solutions, typically for globally distributed applications or for disaster recovery across regions. The question does not mention any multi-region requirements. Configuring a global table adds complexity and cost that is not justified by the problem description.

</details>

<details>
  <summary>Question 80</summary>
  
A company recently signed a contract with an AWS Managed Service Provider (MSP) Partner for help with an application migration initiative. A solutions architect needs to share an Amazon Machine Image (AMI) from an existing AWS account with the MSP Partner's AWS account. The AMI is backed by Amazon Elastic Block Store (Amazon EBS) and uses an AWS Key Management Service (AWS KMS) customer managed key to encrypt EBS volume snapshots.

What is the MOST secure way for the solutions architect to share the AMI with the MSP Partner's AWS account?

- [ ] A. Make the encrypted AMI and snapshots publicly available. Modify the key policy to allow the MSP Partner's AWS account to use the key.
- [ ] B. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to allow the MSP Partner's AWS account to use the key.
- [ ] C. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to trust a new KMS key that is owned by the MSP Partner for encryption.
- [ ] D. Export the AMI from the source account to an Amazon S3 bucket in the MSP Partner's AWS account, Encrypt the S3 bucket with a new KMS key that is owned by the MSP Partner. Copy and launch the AMI in the MSP Partner's AWS account.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to allow the MSP Partner's AWS account to use the key.

Why this is the correct answer:

This solution follows the principle of least privilege and uses AWS-recommended mechanisms for sharing encrypted AMIs:

- [ ] Modify AMI Launch Permissions: To share a private AMI with another specific AWS account, you modify its launchPermission attribute. This allows you to specify the AWS account ID(s) that are permitted to launch instances from this AMI. Sharing it "with the MSP Partner's AWS account only" ensures that the AMI is not exposed to other unintended accounts.
- [ ] Share the KMS Key: Since the AMI's underlying EBS snapshots are encrypted with a customer-managed AWS KMS key, the MSP Partner's account will need permission to use this specific KMS key when launching an instance from the shared AMI (to decrypt the snapshots). This is achieved by modifying the key policy of the KMS key in the source account. The key policy must grant the MSP Partner's AWS account (or specific IAM principals within that account) the necessary permissions, such as kms:Decrypt, kms:ReEncrypt*, kms:CreateGrant, and kms:DescribeKey.
- [ ] MOST Secure Way: This method ensures that the AMI itself is not made public and that access to the encryption key is explicitly granted only to the trusted MSP partner account.

Why are the other answers wrong?

- [ ] Option A is wrong because: Making an AMI and its snapshots publicly available is a significant security risk, especially if they contain sensitive configurations or data. Even if the KMS key policy restricts usage, exposing the AMI publicly is not the "MOST secure way."
- [ ] Option C is wrong because: When the MSP partner launches an instance from the shared AMI, their account needs permission to use the original KMS key (owned by the source account) that was used to encrypt the snapshots. The source key policy grants usage permissions to the MSP account for the source key. It doesn't involve the source key policy "trusting a new KMS key owned by the MSP Partner for encryption" of the original snapshots. After launching, the MSP could choose to re-encrypt the resulting EBS volumes with their own key, but the initial launch requires access to the source key.
- [ ] Option D is wrong because: Exporting an AMI to S3 and then having the partner import it is a more cumbersome and less direct method than sharing the AMI via launch permissions. While it can be done, it involves more steps, data transfer, and potentially more complex permission management than the native AMI sharing mechanism. Direct AMI sharing combined with KMS key policy modification is generally considered more straightforward and secure for this use case.

</details>

</details>

<details>
  <summary>==Questions 81-90==</summary>

<details>
  <summary>Question 81</summary>

A solutions architect is designing the cloud architecture for a new application being deployed on AWS. The process should run in parallel while adding and removing application nodes as needed based on the number of jobs to be processed. The processor application is stateless. The solutions architect must ensure that the application is loosely coupled and the job items are durably stored.

Which design should the solutions architect use?

- [ ] A. Create an Amazon SNS topic to send the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch configuration that uses the AMI. Create an Auto Scaling group using the launch configuration. Set the scaling policy for the Auto Scaling group to add and remove nodes based on CPU usage.
- [ ] B. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch configuration that uses the AMI. Create an Auto Scaling group using the launch configuration. Set the scaling policy for the Auto Scaling group to add and remove nodes based on network usage.
- [ ] C. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of items in the SQS queue.
- [ ] D. Create an Amazon SNS topic to send the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of messages published to the SNS topic.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of items in the SQS queue.

Why this is the correct answer:

- [ ] Amazon SQS for Durable and Decoupled Job Queuing: Amazon Simple Queue Service (SQS) is designed to store messages (job items) durably and provide loose coupling between components. This meets the requirements for job items to be "durably stored" and the application to be "loosely coupled."
- [ ] Stateless EC2 Instances with Auto Scaling: The processor application is stateless and can run on Amazon EC2 instances. An Auto Scaling group, using a launch template with the application's AMI, can manage these instances.
- [ ] Scaling Based on Queue Depth (Number of Jobs): The crucial requirement is to scale "based on the number of jobs to be processed." EC2 Auto Scaling can be configured to use Amazon CloudWatch metrics from SQS, such as ApproximateNumberOfMessagesVisible (the number of items in the SQS queue), to trigger scaling actions.
- [ ] This ensures that the number of application nodes (EC2 instances) scales up when there are many jobs in the queue and scales down when the queue is short. This allows processing to "run in parallel."
- [ ] Launch Templates: Launch templates are the recommended way to specify launch parameters for EC2 Auto Scaling groups, offering more features and flexibility than older launch configurations.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon SNS (Simple Notification Service) is a publish/subscribe messaging service, primarily used for fanning out notifications to multiple subscribers. It is not designed as a queue for holding a backlog of jobs that need processing by a pool of workers in the way SQS is. Also, scaling based on CPU usage might not accurately reflect the number of pending jobs; jobs could be I/O-bound or numerous but individually lightweight on CPU.
- [ ] Option B is wrong because: While using SQS is correct for job queuing, scaling the Auto Scaling group based on "network usage" is generally not a direct or reliable indicator of the number of jobs waiting to be processed. Scaling based on the SQS queue depth is a much more direct and appropriate metric.
- [ ] Option D is wrong because: As with option A, SNS is not the appropriate service for durable job queuing. Furthermore, scaling based on the "number of messages published to the SNS topic" is not a standard or practical metric for an Auto Scaling group managing worker instances that process a backlog. SNS messages are pushed to subscribers and not typically stored in the topic itself for backlog processing.

</details>  

<details>
  <summary>Question 82</summary>

A company hosts its web applications in the AWS Cloud. The company configures Elastic Load Balancers to use certificates that are imported into AWS Certificate Manager (ACM). The company's security team must be notified 30 days before the expiration of each certificate.

What should a solutions architect recommend to meet this requirement?

- [ ] A. Add a rule in ACM to publish a custom message to an Amazon Simple Notification Service (Amazon SNS) topic every day, beginning 30 days before any certificate will expire.
- [ ] B. Create an AWS Config rule that checks for certificates that will expire within 30 days. Configure Amazon EventBridge (Amazon CloudWatch Events) to invoke a custom alert by way of Amazon Simple Notification Service (Amazon SNS) when AWS Config reports a noncompliant resource.
- [ ] C. Use AWS Trusted Advisor to check for certificates that will expire within 30 days. Create an Amazon CloudWatch alarm that is based on Trusted Advisor metrics for check status changes. Configure the alarm to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).
- [ ] D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to detect any certificates that will expire within 30 days. Configure the rule to invoke an AWS Lambda function. Configure the Lambda function to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to detect any certificates that will expire within 30 days. Configure the rule to invoke an AWS Lambda function. Configure the Lambda function to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).

Why this is the correct answer:

- [ ] ACM Events for Expiration: AWS Certificate Manager (ACM) automatically publishes events to Amazon EventBridge (formerly CloudWatch Events) when an imported certificate (which is the case here) is approaching its expiration date. Specifically, for certificates that ACM does not manage renewal for (like imported ones), an ACM Certificate Approaching Expiration event is sent at predefined intervals, including 30 days before expiration.
- [ ] EventBridge Rule: An EventBridge rule can be created to listen for this specific ACM Certificate Approaching Expiration event. The rule can be filtered to act only on the 30-day notification or process the event details to confirm the timeframe.
- [ ] Lambda for Custom Alerting: The EventBridge rule can be configured to invoke an AWS Lambda function. This Lambda function can then process the event details, format a custom notification message, and send this alert to an Amazon Simple Notification Service (Amazon SNS) topic. The security team would be subscribed to this SNS topic to receive the notifications.
- [ ] Event-Driven and Serverless: This solution is event-driven, directly utilizing the notifications provided by ACM, and is fully serverless (EventBridge, Lambda, SNS), minimizing operational overhead.

Why are the other answers wrong?

- [ ] Option A is wrong because: AWS Certificate Manager (ACM) itself does not have a feature to "add a rule" to directly publish custom messages to SNS based on certificate expiration. ACM integrates with EventBridge for such event notifications.
- [ ] Option B is wrong because: While AWS Config can be used with a managed rule (acm-certificate-expiration-check) to identify certificates nearing expiration and then trigger notifications via EventBridge and SNS, this approach adds an extra layer (AWS Config) primarily designed for broader configuration compliance and auditing. Directly reacting to ACM's native EventBridge events (as in option D) is a more direct and often simpler solution if the sole requirement is an expiration notification.
- [ ] Option C is wrong because: AWS Trusted Advisor does provide a check for ACM certificate expiration. However, Trusted Advisor checks run periodically, and relying on its refresh cycle and then setting up CloudWatch alarms based on Trusted Advisor metrics can be less immediate or direct than using the real-time events emitted by ACM to EventBridge. Additionally, programmatic access to Trusted Advisor check results or automated notifications based on them might have dependencies on the company's AWS Support plan.

</details>

<details>
  <summary>Question 83</summary>

A company's dynamic website is hosted using on-premises servers in the United States. The company is launching its product in Europe, and it wants to optimize site loading times for new European users. The site's backend must remain in the United States. The product is being launched in a few days, and an immediate solution is needed.

What should the solutions architect recommend?

- [ ] A. Launch an Amazon EC2 instance in us-east-1 and migrate the site to it.
- [ ] B. Move the website to Amazon S3. Use Cross-Region Replication between Regions.
- [ ] C. Use Amazon CloudFront with a custom origin pointing to the on-premises servers.
- [ ] D. Use an Amazon Route 53 geoproximity routing policy pointing to on-premises servers.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use Amazon CloudFront with a custom origin pointing to the on-premises servers.

Why this is the correct answer:

- [ ] Amazon CloudFront for Latency Reduction: Amazon CloudFront is a global content delivery network (CDN) service. It caches static assets (like images, CSS, JavaScript) at edge locations around the world, including many in Europe. When European users access the website, these static assets can be served from a nearby edge location, significantly reducing latency and improving site loading times.
- [ ] Custom Origin for On-Premises Backend: CloudFront can be configured to use a "custom origin," which can be your on-premises servers located in the United States. For dynamic content that cannot be cached, CloudFront routes requests back to this origin, often over optimized network paths within the AWS backbone for part of the journey, which can still offer performance improvements compared to users directly accessing the US servers over the public internet.
- [ ] Backend Remains in the United States: This solution allows the site's backend to remain in the United States, as required.
- [ ] Immediate Solution: Setting up a CloudFront distribution with a custom origin is a relatively quick process and can provide immediate performance benefits for European users, meeting the "immediate solution is needed" requirement.

Why are the other answers wrong?

- [ ] Option A is wrong because: Migrating the site to an Amazon EC2 instance in us-east-1 (a US East region) still means the website is hosted in the United States. This would not fundamentally optimize site loading times for European users compared to well-provisioned on-premises US servers, especially if the backend must remain in the US anyway. CloudFront is specifically designed for global content delivery with low latency.
- [ ] Option B is wrong because: Moving a "dynamic website" to Amazon S3 is only feasible if the dynamic aspects are handled elsewhere (e.g., via serverless functions) or if it can be converted to a static site. If the website truly relies on dynamic processing from the on-premises servers, S3 alone is not a suitable hosting solution for the dynamic parts. While S3 with Cross-Region Replication can distribute static assets, CloudFront is still the primary service for low-latency delivery of those assets.
- [ ] Option D is wrong because: An Amazon Route 53 geoproximity routing policy (or geolocation routing) directs users to specific resources based on their geographic location. However, if all requests are ultimately pointed back to the same on-premises servers in the United States, this routing policy alone will not improve the loading times for European users. CloudFront improves performance by caching content closer to the users, which Route 53 by itself does not do.

</details>

<details>
  <summary>Question 84</summary>

A company wants to reduce the cost of its existing three-tier web architecture. The web, application, and database servers are running on Amazon EC2 instances for the development, test, and production environments. The EC2 instances average 30% CPU utilization during peak hours and 10% CPU utilization during non-peak hours. The production EC2 instances run 24 hours a day. The development and test EC2 instances run for at least 8 hours each day. The company plans to implement automation to stop the development and test EC2 instances when they are not in use.

Which EC2 instance purchasing solution will meet the company's requirements MOST cost-effectively?

- [ ] A. Use Spot Instances for the production EC2 instances. Use Reserved Instances for the development and test EC2 instances.
- [ ] B. Use Reserved Instances for the production EC2 instances. Use On-Demand Instances for the development and test EC2 instances.
- [ ] C. Use Spot blocks for the production EC2 instances. Use Reserved Instances for the development and test EC2 instances.
- [ ] D. Use On-Demand Instances for the production EC2 instances. Use Spot blocks for the development and test EC2 instances.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use Reserved Instances for the production EC2 instances. Use On-Demand Instances for the development and test EC2 instances.

Why this is the correct answer:

- [ ] Production EC2 Instances (24/7 workload): Reserved Instances (RIs): The production instances run 24 hours a day. For such continuous, predictable workloads, Amazon EC2 Reserved Instances offer significant discounts (up to 72%) compared to On-Demand pricing in exchange for a 1-year or 3-year commitment. This is the most cost-effective approach for the always-on production environment. Even with CPU utilization averaging 10-30%, having a baseline covered by RIs is beneficial.
- [ ] Development and Test EC2 Instances (Intermittent workload): On-Demand Instances: The development and test instances run for at least 8 hours a day and will be stopped when not in use. On-Demand Instances are ideal for such workloads because you pay for compute capacity by the second (for Linux) or hour with no long-term commitments or upfront payments. When the instances are stopped, you do not pay for the compute hours. This flexibility makes On-Demand cost-effective for these intermittent environments.
- [ ] MOST Cost-Effective Combination: This strategy optimizes costs by leveraging RIs for the predictable, continuous production load and On-Demand pricing for the flexible, stop-start nature of development and test environments.

Why are the other answers wrong?

- [ ] Option A is wrong because:
Spot Instances for Production: Spot Instances can offer deep discounts but can be interrupted by AWS with only a two-minute warning if AWS needs the capacity back. For production workloads of a three-tier web architecture, relying solely on Spot Instances is generally too risky due to potential service interruptions, unless the application is specifically designed for fault tolerance against such interruptions.
Reserved Instances for Dev/Test: Using RIs for development and test instances that are frequently stopped is not cost-effective. You commit to paying for RIs for the entire term, regardless of whether the instance is running or stopped (for most RI types).
- [ ] Option C is wrong because:
Spot Blocks for Production: Spot Instances with a defined duration (Spot blocks) are for workloads that need to run uninterrupted for a finite duration (1 to 6 hours). This is not suitable for production instances that run 24/7.
Reserved Instances for Dev/Test: Same issue as in option A.
- [ ] Option D is wrong because:
On-Demand Instances for Production: Using On-Demand Instances for production workloads that run 24/7 is the most expensive option. Significant savings can be achieved with RIs or Savings Plans for such continuous usage.
Spot Blocks for Dev/Test: While Spot (or Spot blocks if the "at least 8 hours" fits within the block duration and interruptions are acceptable) can be cheaper than On-Demand, the key is that On-Demand provides flexibility to stop and start without long-term commitment, making it suitable. However, the primary flaw in this option is using On-Demand for 24/7 production. Option B provides a better overall cost optimization strategy.

</details>

<details>
  <summary>Question 85</summary>

A company has a production web application in which users upload documents through a web interface or a mobile app. According to a new regulatory requirement. new documents cannot be modified or deleted after they are stored.

What should a solutions architect do to meet this requirement?

- [ ] A. Store the uploaded documents in an Amazon S3 bucket with S3 Versioning and S3 Object Lock enabled.
- [ ] B. Store the uploaded documents in an Amazon S3 bucket. Configure an S3 Lifecycle policy to archive the documents periodically.
- [ ] C. Store the uploaded documents in an Amazon S3 bucket with S3 Versioning enabled. Configure an ACL to restrict all access to read-only.
- [ ] D. Store the uploaded documents on an Amazon Elastic File System (Amazon EFS) volume. Access the data by mounting the volume in read-only mode.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Store the uploaded documents in an Amazon S3 bucket with S3 Versioning and S3 Object Lock enabled.

Why this is the correct answer:

- [ ] Amazon S3 for Document Storage: S3 is a highly durable and scalable service for storing documents.
- [ ] S3 Object Lock for Immutability: The core requirement is that documents "cannot be modified or deleted after they are stored." Amazon S3 Object Lock provides Write-Once-Read-Many (WORM) protection for objects. When Object Lock is enabled with a retention period and mode (governance or compliance), it prevents objects from being overwritten or deleted for that specified duration.
- [ ] For the strictest requirement, compliance mode would be used, which prevents even the root user from deleting or overwriting the object version until the retention period expires.
- [ ] S3 Versioning Requirement: S3 Object Lock can only be enabled on buckets that have S3 Versioning enabled. Versioning itself helps protect against accidental overwrites (by creating new versions) and deletions (by creating delete markers), and Object Lock then protects specific object versions from being changed or deleted.
- [ ] This combination directly meets the regulatory requirement for immutability.

Why are the other answers wrong?

- [ ] Option B is wrong because: An S3 Lifecycle policy is used to manage the storage class of objects (e.g., moving them to archive tiers for cost savings) or to define when objects expire (are deleted). It does not prevent modification or deletion of active objects before the lifecycle rules apply. It's for managing data over its lifetime, not for making it immutable upon creation.
- [ ] Option C is wrong because: While enabling S3 Versioning helps protect against accidental overwrites and allows recovery from deletions (by removing delete markers), it does not by itself prevent a user with delete permissions from deleting object versions. Configuring an ACL (or bucket policy) to restrict access to read-only would prevent modifications for users subject to that ACL/policy, but users with higher privileges could still potentially modify or delete objects or change the permissions. S3 Object Lock provides a stronger guarantee of immutability at the object level.
- [ ] Option D is wrong because: Mounting an Amazon EFS volume in read-only mode is a client-side setting. It does not prevent users or processes with write access to the EFS file system (e.g., from a different EC2 instance or with administrative privileges on the EFS mount) from modifying or deleting the files directly on the EFS file system. EFS does not have an equivalent to S3 Object Lock for file-level WORM protection.

</details>

<details>
  <summary>Question 86</summary>

A company has several web servers that need to frequently access a common Amazon RDS MySQL Multi-AZ DB instance. The company wants a secure method for the web servers to connect to the database while meeting a security requirement to rotate user credentials frequently.

Which solution meets these requirements?

- [ ] A. Store the database user credentials in AWS Secrets Manager. Grant the necessary IAM permissions to allow the web servers to access AWS Secrets Manager.
- [ ] B. Store the database user credentials in AWS Systems Manager OpsCenter. Grant the necessary IAM permissions to allow the web servers to access OpsCenter.
- [ ] C. Store the database user credentials in a secure Amazon S3 bucket. Grant the necessary IAM permissions to allow the web servers to retrieve credentials and access the database.
- [ ] D. Store the database user credentials in files encrypted with AWS Key Management Service (AWS KMS) on the web server file system. The web server should be able to decrypt the files and access the database.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Store the database user credentials in AWS Secrets Manager. Grant the necessary IAM permissions to allow the web servers to access AWS Secrets Manager.

Why this is the correct answer:

- [ ] AWS Secrets Manager for Secure Storage and Rotation: AWS Secrets Manager is a service specifically designed to manage, retrieve, and rotate secrets such as database credentials, API keys, and other sensitive information. It securely stores the credentials and integrates with services like Amazon RDS MySQL to enable automatic credential rotation on a schedule. This directly meets the requirement to "rotate user credentials frequently" with minimal operational overhead.
- [ ] Secure Access for Applications: Web servers (running on EC2 instances or containers) can be granted IAM permissions (typically via an IAM role attached to the compute resource) to securely retrieve the database credentials from Secrets Manager at runtime. This avoids hardcoding credentials in application code or configuration files.
- [ ] Centralized Management: Secrets Manager provides a central place to manage and audit access to secrets.

Why are the other answers wrong?

- [ ] Option B is wrong because: AWS Systems Manager OpsCenter is a service used for aggregating, investigating, and resolving operational issues (OpsItems) related to your AWS resources. It is not designed or intended for storing or managing sensitive data like database credentials.
- [ ] Option C is wrong because: While storing credentials in an encrypted S3 bucket is possible, it is generally less secure and less manageable than using a dedicated secrets management service like AWS Secrets Manager. More importantly, this approach does not provide an automated mechanism for frequent credential rotation; this would have to be custom-built, increasing operational overhead.
- [ ] Option D is wrong because: Storing encrypted credential files directly on the web server's file system still poses risks if the server is compromised. Managing the encryption keys, the decryption process within the application, and especially the frequent rotation of these credentials across multiple web servers would be operationally complex and error-prone. AWS Secrets Manager centralizes and automates these tasks.

</details>

<details>
  <summary>Question 87</summary>

A company hosts an application on AWS Lambda functions that are invoked by an Amazon API Gateway API. The Lambda functions save customer data to an Amazon Aurora MySQL database. Whenever the company upgrades the database, the Lambda functions fail to establish database connections until the upgrade is complete. The result is that customer data is not recorded for some of the event.

A solutions architect needs to design a solution that stores customer data that is created during database upgrades.

Which solution will meet these requirements?

- [ ] A. Provision an Amazon RDS proxy to sit between the Lambda functions and the database. Configure the Lambda functions to connect to the RDS proxy.
- [ ] B. Increase the run time of the Lambda functions to the maximum. Create a retry mechanism in the code that stores the customer data in the database.
- [ ] C. Persist the customer data to Lambda local storage. Configure new Lambda functions to scan the local storage to save the customer data to the database.
- [ ] D. Store the customer data in an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Create a new Lambda function that polls the queue and stores the customer data in the database.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Store the customer data in an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Create a new Lambda function that polls the queue and stores the customer data in the database.

Why this is the correct answer:

- [ ] Decoupling with Amazon SQS: The primary issue is data loss when the database is unavailable during upgrades. By introducing an Amazon SQS queue, the Lambda functions invoked by API Gateway can send the customer data as messages to the SQS queue instead of trying to write directly to the database. SQS acts as a durable buffer.
- [ ] Ensuring Data Durability: Messages sent to SQS are stored durably until they are successfully processed. This means if the database is down for an upgrade, the incoming customer data is not lost but is queued safely.
- [ ] Asynchronous Processing by a Second Lambda: A separate AWS Lambda function can be configured to poll the SQS queue. This second Lambda function would be responsible for reading messages from the queue and writing the customer data to the Amazon Aurora MySQL database. This processing happens asynchronously. If the database is unavailable, the messages remain in the SQS queue, and the Lambda function can retry processing them later (SQS allows messages to become visible again after a timeout if not deleted).
- [ ] FIFO Queue for Order Preservation: Using an SQS FIFO (First-In, First-Out) queue ensures that customer data is processed and stored in the database in the same order it was received, which can be important for transactional data.
- [ ] This solution directly addresses the problem by ensuring data created during database upgrades is durably stored and processed once the database is available.

Why are the other answers wrong?

- [ ] Option A is wrong because: While Amazon RDS Proxy is excellent for managing database connections (pooling) and improving application resilience to database failovers (e.g., in a Multi-AZ setup), it doesn't inherently solve the problem of the database being completely unavailable for an extended period during an upgrade. If the RDS Proxy cannot connect to the underlying database because it's undergoing an upgrade, the Lambda functions would still fail to save data through the proxy. RDS Proxy doesn't queue the data itself if the database is offline.
- [ ] Option B is wrong because: Increasing Lambda run time and implementing retries within the Lambda code might help with very short, transient database unavailability. However, database upgrades can sometimes take several minutes or longer, potentially exceeding even the maximum Lambda execution time (15 minutes). Continuous retries during an extended upgrade would be inefficient, costly, and could still result in data loss if all retries fail or the Lambda times out. It doesn't provide a durable store for data arriving during the entire upgrade window.
- [ ] Option C is wrong because: Lambda local storage (/tmp directory) is ephemeral and non-persistent. It is tied to a specific execution environment of a Lambda invocation and is not suitable for durably storing customer data, especially if the Lambda function fails or new invocations are used for subsequent requests. Data stored there would be lost.

</details>

<details>
  <summary>Question 88</summary>

A survey company has gathered data for several years from areas in the United States. The company hosts the data in an Amazon S3 bucket that is 3 TB in size and growing. The company has started to share the data with a European marketing firm that has S3 buckets. The company wants to ensure that its data transfer costs remain as low as possible.

Which solution will meet these requirements?

- [ ] A. Configure the Requester Pays feature on the company's S3 bucket.
- [ ] B. Configure S3 Cross-Region Replication from the company's S3 bucket to one of the marketing firm's S3 buckets.
- [ ] C. Configure cross-account access for the marketing firm so that the marketing firm has access to the company's S3 bucket.
- [ ] D. Configure the company's S3 bucket to use S3 Intelligent-Tiering. Sync the S3 bucket to one of the marketing firm's S3 buckets.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Configure the Requester Pays feature on the company's S3 bucket.

Why this is the correct answer:

- [ ] S3 Requester Pays Feature: The Amazon S3 Requester Pays feature allows the bucket owner to specify that the requester (the party downloading the data) will pay for the data transfer out costs, instead of the bucket owner.
- [ ] Minimizing the Survey Company's Costs: By enabling Requester Pays on their S3 bucket, the survey company ensures that when the European marketing firm accesses and downloads the data, the marketing firm incurs the associated data transfer costs. This directly meets the requirement for the survey company to keep "its data transfer costs remain as low as possible."
- [ ] Data Sharing: This solution still allows the marketing firm to access the necessary data from the survey company's S3 bucket, provided they are authenticated and authorized, and are willing to accept the data transfer charges.

Why are the other answers wrong?

- [ ] Option B is wrong because: If the survey company configures S3 Cross-Region Replication to replicate data to the marketing firm's S3 bucket (presumably in a European region), the survey company (as the owner of the source bucket and the replication configuration) would be responsible for the inter-region data transfer costs associated with the replication itself. This would likely increase the survey company's data transfer costs, not minimize them.
- [ ] Option C is wrong because: Configuring cross-account access allows the marketing firm to access the data in the survey company's S3 bucket. However, unless Requester Pays is also enabled, the survey company (as the bucket owner) would typically still be billed for the data transfer out costs when the marketing firm downloads the data from their bucket. This does not inherently minimize the survey company's data transfer costs.
- [ ] Option D is wrong because: S3 Intelligent-Tiering is a storage class that optimizes storage costs by automatically moving data between access tiers based on usage patterns. It does not directly affect or reduce data transfer costs. Syncing the S3 bucket to the marketing firm's bucket would involve data transfer, and the costs for this transfer would likely be borne by the survey company if they initiate the sync, similar to replication.

</details>

<details>
  <summary>Question 89</summary>

A company uses Amazon S3 to store its confidential audit documents. The S3 bucket uses bucket policies to restrict access to audit team IAM user credentials according to the principle of least privilege. Company managers are worried about accidental deletion of documents in the S3 bucket and want a more secure solution.

What should a solutions architect do to secure the audit documents?

- [ ] A. Enable the versioning and MFA Delete features on the S3 bucket.
- [ ] B. Enable multi-factor authentication (MFA) on the IAM user credentials for each audit team IAM user account.
- [ ] C. Add an S3 Lifecycle policy to the audit team's IAM user accounts to deny the s3:DeleteObject action during audit dates.
- [ ] D. Use AWS Key Management Service (AWS KMS) to encrypt the S3 bucket and restrict audit team IAM user accounts from accessing the KMS key.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Enable the versioning and MFA Delete features on the S3 bucket.

Why this is the correct answer:

- [ ] S3 Versioning for Recovery: Enabling S3 Versioning on the bucket means that multiple versions of each document (object) are preserved. If a document is "deleted," S3 actually inserts a delete marker instead of permanently removing the object's content. This allows for easy recovery from accidental deletions by simply removing the delete marker or restoring a previous version. This directly addresses the concern about "accidental deletion."
- [ ] MFA Delete for Enhanced Deletion Protection: Enabling MFA (Multi-Factor Authentication) Delete on a versioning-enabled bucket adds another layer of security. For certain operations, such as permanently deleting an object version or changing the versioning state of the bucket, the bucket owner must provide their standard AWS credentials plus a valid code from their MFA device. This makes it significantly harder to accidentally (or even maliciously, without MFA device access) permanently delete documents. This provides a "more secure solution."

Why are the other answers wrong?

- [ ] Option B is wrong because: Enabling MFA for IAM user accounts is a good security practice for protecting user identities and preventing unauthorized access to those accounts. However, it does not directly prevent an authorized user (who has successfully authenticated with MFA) from accidentally deleting a document if their IAM permissions allow deletion. MFA Delete on the S3 bucket (Option A) specifically protects the S3 delete operations themselves.
- [ ] Option C is wrong because: S3 Lifecycle policies are used to manage the lifecycle of objects (e.g., transitioning them to different storage classes or expiring/deleting them after a defined period). You cannot attach S3 Lifecycle policies to IAM user accounts. While a bucket policy could be used to deny delete actions, this option misrepresents how lifecycle policies function and what they are applied to.
- [ ] Option D is wrong because: Using AWS KMS to encrypt the S3 bucket is essential for protecting the confidentiality of the audit documents (data at rest). However, encryption does not prevent the documents from being deleted. If an authorized user (or an attacker who gains access) deletes an encrypted document, the document is still gone. The primary concern stated is "accidental deletion," not unauthorized reading of the content.

</details>

<details>
  <summary>Question 90</summary>

A company is using a SQL database to store movie data that is publicly accessible. The database runs on an Amazon RDS Single-AZ DB instance. A script runs queries at random intervals each day to record the number of new movies that have been added to the database. The script must report a final total during business hours.

The company's development team notices that the database performance is inadequate for development tasks when the script is running. A solutions architect must recommend a solution to resolve this issue.

Which solution will meet this requirement with the LEAST operational overhead?

- [ ] A. Modify the DB instance to be a Multi-AZ deployment.
- [ ] B. Create a read replica of the database. Configure the script to query only the read replica.
- [ ] C. Instruct the development team to manually export the entries in the database at the end of each day.
- [ ] D. Use Amazon ElastiCache to cache the common queries that the script runs against the database.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create a read replica of the database. Configure the script to query only the read replica.

Why this is the correct answer:

- [ ] Issue: Read Load Impacting Primary DB: The problem states that the script's queries (which are read operations to count new movies) are degrading database performance for development tasks. This indicates that the read load from the script is impacting the primary (and only) DB instance.
- [ ] Amazon RDS Read Replicas: Amazon RDS allows you to create one or more read replicas of your source DB instance. Read replicas are asynchronously replicated copies of the primary instance and are designed to offload read-heavy traffic.
- [ ] Offloading Script Queries: By creating a read replica and modifying the script to direct its queries to this read replica, the read load generated by the script is removed from the primary DB instance. This allows the primary instance to dedicate its resources to serving the development team's tasks and other application writes without performance degradation caused by the script.
- [ ] Least Operational Overhead: Creating a read replica in RDS is a managed operation. Modifying the script's connection string to point to the read replica's endpoint is a relatively small change. This solution effectively isolates the read traffic with minimal ongoing management effort.

Why are the other answers wrong?

- [ ] Option A is wrong because: Modifying the DB instance to a Multi-AZ deployment creates a synchronous standby replica in a different Availability Zone. The primary purpose of Multi-AZ is to provide high availability and data durability by enabling automatic failover. The standby instance in a Multi-AZ setup does not serve read traffic (except during a failover event). Therefore, this will not alleviate the performance issue caused by the script's read queries on the primary instance.
- [ ] Option C is wrong because: Instructing the development team to perform manual exports is an operational burden and does not solve the problem of the script itself impacting database performance when it runs. The script still needs to run its queries to get the count of new movies.
- [ ] Option D is wrong because: Amazon ElastiCache is an in-memory caching service. While it can reduce database load by caching frequently accessed query results, the script is querying for "new movies added." Such queries, which seek new or changing data, are often not good candidates for caching, as the cache would frequently be stale or miss the latest additions. A read replica directly offloads the entire query load, regardless of whether the results are cacheable.

</details>

</details>

<details>
  <summary>==Questions 91-100==</summary>

<details>
  <summary>Question 91</summary>

A company has applications that run on Amazon EC2 instances in a VPC. One of the applications needs to call the Amazon S3 API to store and read objects. According to the company's security regulations, no traffic from the applications is allowed to travel across the internet.

Which solution will meet these requirements?

- [ ] A. Configure an S3 gateway endpoint.
- [ ] B. Create an S3 bucket in a private subnet.
- [ ] C. Create an S3 bucket in the same AWS Region as the EC2 instances.
- [ ] D. Configure a NAT gateway in the same subnet as the EC2 instances.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Configure an S3 gateway endpoint.

Why this is the correct answer:

- [ ] S3 Gateway Endpoint for Private Connectivity: An Amazon S3 gateway endpoint (a type of VPC endpoint) enables your EC2 instances within a VPC to connect to Amazon S3 services directly, without requiring traffic to traverse an Internet Gateway, NAT Gateway, VPN connection, or AWS Direct Connect connection.
- [ ] No Internet Traffic: Traffic between your VPC and Amazon S3 using a gateway endpoint stays entirely within the AWS network. This ensures that "no traffic from the applications is allowed to travel across the internet," meeting the company's security regulation.
- [ ] Cost and Performance: Using a gateway endpoint for S3 can also reduce data transfer costs, as data transferred to S3 within the same region via a gateway endpoint is typically free, and it avoids NAT gateway processing charges. It can also improve performance due to the direct private connection.

Why are the other answers wrong?

- [ ] Option B is wrong because: Amazon S3 buckets are not created "in a private subnet." S3 is a regional service, and buckets are accessed via regional or global endpoints. The concept of placing an S3 bucket within a VPC subnet is incorrect.
- [ ] Option C is wrong because: While creating an S3 bucket in the same AWS Region as the EC2 instances is a general best practice for performance and cost, it does not, by itself, ensure that traffic between the EC2 instances and S3 will not traverse the internet (e.g., if the instances are in a private subnet and use a NAT gateway to reach public S3 endpoints). The security regulation specifically requires traffic not to travel across the internet.
- [ ] Option D is wrong because: A NAT gateway is used to allow instances in a private subnet to initiate outbound connections to the internet or other AWS public services (like S3 public endpoints) while preventing unsolicited inbound connections from the internet. If EC2 instances use a NAT gateway to access S3, the traffic is routed through the NAT gateway to S3's public endpoints. While this traffic typically stays within the AWS network for same-region S3 access, a gateway endpoint (Option A) provides a more direct, private path within the VPC without relying on NAT gateways and ensures traffic doesn't use public S3 endpoints via the internet path. The requirement is explicitly "no traffic... across the internet."

</details>

<details>
  <summary>Question 92</summary>

A company is storing sensitive user information in an Amazon S3 bucket. The company wants to provide secure access to this bucket from the application tier running on Amazon EC2 instances inside a VPC.

Which combination of steps should a solutions architect take to accomplish this? (Choose two.)

- [ ] A. Configure a VPC gateway endpoint for Amazon S3 within the VPC.
- [ ] B. Create a bucket policy to make the objects in the S3 bucket public.
- [ ] C. Create a bucket policy that limits access to only the application tier running in the VPC.
- [ ] D. Create an IAM user with an S3 access policy and copy the IAM credentials to the EC2 instance.
- [ ] E. Create a NAT instance and have the EC2 instances use the NAT instance to access the S3 bucket.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Configure a VPC gateway endpoint for Amazon S3 within the VPC.
- [ ] C. Create a bucket policy that limits access to only the application tier running in the VPC.

Why these are the correct answers:

This question focuses on providing secure access from EC2 instances within a VPC to an S3 bucket containing sensitive information.

A. Configure a VPC gateway endpoint for Amazon S3 within the VPC.
- [ ] Private Connectivity: A VPC gateway endpoint for S3 allows your EC2 instances within the VPC to communicate with S3 without traffic leaving the AWS network (i.e., not going over the public internet via an Internet Gateway or NAT Gateway). This enhances security by keeping the network path private.
- [ ] Cost Savings: It can also reduce data transfer costs as traffic to S3 via a gateway endpoint within the same region does not incur data transfer charges or NAT gateway processing fees.

C. Create a bucket policy that limits access to only the application tier running in the VPC.
- [ ] Principle of Least Privilege: An S3 bucket policy allows you to define fine-grained permissions on your S3 bucket and its objects. To secure sensitive data, you should create a bucket policy that explicitly grants access only to the necessary principals. In this case, it would be the IAM role(s) associated with the EC2 instances in the application tier.
- [ ] Restricting Access: The policy can be written to allow actions (like s3:GetObject, s3:PutObject) only if the request originates from the EC2 instances belonging to the application tier (e.g., by referencing their IAM role or the VPC endpoint). This ensures that only the authorized application tier can access the sensitive information.
- [ ] Combining a private network path (VPC gateway endpoint) with resource-based permissions (S3 bucket policy) provides a robust and secure access mechanism.

Why are the other answers wrong?

- [ ] Option B is wrong because: Making an S3 bucket that stores "sensitive user information" public is a severe security risk and directly contradicts the goal of providing secure access.
- [ ] Option D is wrong because: Creating an IAM user and embedding its long-term access keys (access key ID and secret access key) onto EC2 instances is not a security best practice. These credentials can be compromised if the instance is breached. The recommended approach for EC2 instances to securely access AWS services is by using IAM roles, which provide temporary, automatically rotated credentials.
- [ ] Option E is wrong because: A NAT instance (or NAT Gateway) is used to enable instances in a private subnet to initiate outbound connections to the internet or public AWS services. While this allows access to S3's public endpoints, it does not provide the same level of private connectivity as a VPC gateway endpoint. Using a VPC gateway endpoint is more secure for this use case as it keeps traffic off the internet path and can also be more cost-effective.

</details>

<details>
  <summary>Question 93</summary>

A company runs an on-premises application that is powered by a MySQL database. The company is migrating the application to AWS to increase the application's elasticity and availability. The current architecture shows heavy read activity on the database during times of normal operation. Every 4 hours, the company's development team pulls a full export of the production database to populate a database in the staging environment. During this period, users experience unacceptable application latency. The development team is unable to use the staging environment until the procedure completes.

A solutions architect must recommend replacement architecture that alleviates the application latency issue. The replacement architecture also must give the development team the ability to continue using the staging environment without delay.

Which solution meets these requirements?

- [ ] A. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Populate the staging database by implementing a backup and restore process that uses the mysqldump utility.
- [ ] B. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Use database cloning to create the staging database on-demand.
- [ ] C. Use Amazon RDS for MySQL with a Multi-AZ deployment and read replicas for production. Use the standby instance for the staging database.
- [ ] D. Use Amazon RDS for MySQL with a Multi-AZ deployment and read replicas for production. Populate the staging database by implementing a backup and restore process that uses the mysqldump utility.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Use database cloning to create the staging database on-demand.

Why this is the correct answer:

- [ ] Amazon Aurora MySQL for Production: Amazon Aurora MySQL is a high-performance, MySQL-compatible database designed for the cloud. Using it with Multi-AZ Aurora Replicas addresses the "heavy read activity" by allowing read traffic to be offloaded to the replicas, and provides high availability for the production environment.
- [ ] Database Cloning for Staging: A key feature of Amazon Aurora is its ability to perform fast database cloning. Cloning creates a new, independent copy of your database cluster very quickly (often in minutes), regardless of the database size. This is because it's initially a copy-on-write clone. This process has minimal impact on the performance of the production database.
- [ ] Alleviates Latency and Staging Delay: By using database cloning instead of a full export (like mysqldump), the "unacceptable application latency" experienced on the production database during the export process is eliminated. The staging database can be created quickly "on-demand," allowing the development team to use the staging environment "without delay."

Why are the other answers wrong?

- [ ] Option A is wrong because: Implementing a backup and restore process using the mysqldump utility to populate the staging database is similar to the current problematic method of pulling a full export. This would likely still impose a significant load on the production database and take considerable time, leading to the same latency issues and delays for the staging environment that the company wants to avoid.
- [ ] Option C is wrong because: In an Amazon RDS Multi-AZ deployment, the standby instance is a passive replica maintained for failover purposes. It cannot be directly accessed or used to serve read traffic or as a source for a staging database. Attempting to use the standby instance for staging is not a supported or viable approach. Read replicas are used for offloading read traffic, not the standby.
- [ ] Option D is wrong because: Similar to option A, using mysqldump for backup and restore to populate the staging database does not solve the core problem of performance degradation on the production instance during the export or the time it takes to make the staging environment available. While Amazon RDS for MySQL with read replicas is a good solution for read-heavy production workloads, the method for creating the staging environment is flawed in this option.

</details>

<details>
  <summary>Question 94</summary>

- [ ] A.  Turn  
A company is designing an application where users upload small files into Amazon S3. After a user uploads a file, the file requires one-time simple processing to transform the data and save the data in JSON format for later analysis. Each file must be processed as quickly as possible after it is uploaded. Demand will vary. On some days, users will upload a high number of files. On other days, users will upload a few files or no files.

Which solution meets these requirements with the LEAST operational overhead?

- [ ] A. Configure Amazon EMR to read text files from Amazon S3. Run processing scripts to transform the data. Store the resulting JSON file in an Amazon Aurora DB cluster.
- [ ] B. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use Amazon EC2 instances to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.
- [ ] C. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use an AWS Lambda function to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.
- [ ] D. Configure Amazon EventBridge (Amazon CloudWatch Events) to send an event to Amazon Kinesis Data Streams when a new file is uploaded. Use an AWS Lambda function to consume the event from the stream and process the data. Store the resulting JSON file in an Amazon Aurora DB cluster.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use an AWS Lambda function to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.

Why this is the correct answer:

This solution offers a serverless, event-driven architecture that is efficient and has low operational overhead:

- [ ] Amazon S3 for File Uploads: S3 is the ideal place to store the uploaded small files.
- [ ] S3 Event Notification to SQS: When a new file is uploaded to S3, an S3 event notification can automatically send a message (containing details about the uploaded file) to an Amazon SQS queue. SQS decouples the file upload from the processing step and provides a durable buffer, which is excellent for handling varying demand ("high number of files" or "few files").
- [ ] AWS Lambda for Processing: An AWS Lambda function can be configured to use the SQS queue as an event source. Lambda will automatically poll the queue and invoke the function to process each file. Since the processing is "one-time simple processing," Lambda is well-suited. It scales automatically based on the number of messages in the queue, ensuring files are processed "as quickly as possible." As a serverless service, it minimizes operational overhead.
- [ ] Amazon DynamoDB for JSON Storage: After processing, the transformed data (in JSON format) can be stored in Amazon DynamoDB. DynamoDB is a fully managed NoSQL database that is excellent for storing JSON documents and scales seamlessly, suitable for "later analysis."
- [ ] Least Operational Overhead: This entire pipeline (S3 -> SQS -> Lambda -> DynamoDB) consists of managed and serverless services, requiring minimal infrastructure management from the company.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon EMR is a big data platform designed for large-scale distributed data processing using frameworks like Spark and Hadoop. Using EMR for "one-time simple processing" of "small files" is overly complex, not cost-effective for this scale, and incurs significant operational overhead compared to Lambda. Storing simple JSON results in an Amazon Aurora DB cluster might also be more overhead than DynamoDB if the analysis needs don't strictly require a relational database.
- [ ] Option B is wrong because: While using S3 event notifications to SQS is a good start, using Amazon EC2 instances to read from the queue and process data requires managing those instances (provisioning, patching, scaling). AWS Lambda (as in option C) offers a serverless alternative that eliminates this instance management overhead and scales automatically.
- [ ] Option D is wrong because: Using Amazon EventBridge to send an event to Amazon Kinesis Data Streams for individual small file uploads adds an unnecessary layer of complexity. Kinesis Data Streams is designed for continuous, real-time data streaming, not typically for discrete file processing events initiated by S3 uploads in this manner. A direct S3 event to SQS (or even S3 to Lambda directly for very quick tasks) is simpler. Also, as with option A, Aurora might be more overhead than DynamoDB for storing the resulting JSON for analysis.

</details>

<details>
  <summary>Question 95</summary>
 
An application allows users at a company's headquarters to access product data. The product data is stored in an Amazon RDS MySQL DB instance. The operations team has isolated an application performance slowdown and wants to separate read traffic from write traffic. A solutions architect needs to optimize the application's performance quickly.

What should the solutions architect recommend?

- [ ] A. Change the existing database to a Multi-AZ deployment. Serve the read requests from the primary Availability Zone.
- [ ] B. Change the existing database to a Multi-AZ deployment. Serve the read requests from the secondary Availability Zone.
- [ ] C. Create read replicas for the database. Configure the read replicas with half of the compute and storage resources as the source database.
- [ ] D. Create read replicas for the database. Configure the read replicas with the same compute and storage resources as the source database.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create read replicas for the database. Configure the read replicas with the same compute and storage resources as the source database.

Why this is the correct answer:

- [ ] Separating Read and Write Traffic: The core problem is an application performance slowdown, and the desired solution is to "separate read traffic from write traffic." Amazon RDS Read Replicas are specifically designed for this purpose. They are asynchronously replicated copies of the primary database instance that can offload read-intensive workloads.
- [ ] Optimizing Performance Quickly: Creating one or more read replicas for an existing RDS instance is a relatively quick and standard procedure. Once created, the application can be configured to direct its read queries to the read replica endpoint(s) and write queries to the primary instance endpoint. This immediately reduces the load on the primary instance, improving its performance for write operations and overall application responsiveness.
- [ ] Sizing of Read Replicas: To effectively handle the read traffic that was previously causing slowdowns on the primary, the read replicas should have adequate resources. Configuring them with the "same compute and storage resources as the source database" (or resources appropriately sized for the read workload) ensures they can handle the diverted read traffic without becoming a new bottleneck. Under-provisioning read replicas could lead to poor read performance.

Why are the other answers wrong?

- [ ] Option A is wrong because: A Multi-AZ deployment for Amazon RDS provides high availability and data durability by creating a synchronous standby replica in a different Availability Zone. However, this standby replica does not serve read traffic during normal operations (it only becomes active during a failover). All read and write traffic still goes to the primary instance. Thus, this does not separate read traffic or alleviate read-related performance issues on the primary.
- [ ] Option B is wrong because: Similar to option A, the secondary (standby) instance in a Multi-AZ deployment is not used for serving read traffic during normal operations. It is strictly for failover. Attempting to serve read requests from the secondary Availability Zone in a standard Multi-AZ setup is incorrect.
- [ ] Option C is wrong because: While creating read replicas is the correct approach, configuring them with "half of the compute and storage resources as the source database" might not be sufficient to handle the existing read load effectively, especially if that read load is significant enough to cause a performance slowdown on the primary. If the read replicas are under-provisioned, they could simply become a new performance bottleneck for read queries, failing to optimize overall application performance.

</details>

<details>
  <summary>Question 96</summary>

An Amazon EC2 administrator created the following policy associated with an IAM group containing several users:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:TerminateInstances",
      "Resource": "*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "10.100.100.0/24"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "ec2:Region": "us-east-1"
        }
      }
    }
  ]
}
```

What is the effect of this policy?

- [ ] A. Users can terminate an EC2 instance in any AWS Region except us-east-1.
- [ ] B. Users can terminate an EC2 instance with the IP address 10.100.100.1 in the us-east-1 Region.
- [ ] C. Users can terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.
- [ ] D. Users cannot terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Users can terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.

Why this is the correct answer:

Let's break down the IAM policy:

- [ ] First Statement (Allow TerminateInstances):

"Effect": "Allow"
"Action": "ec2:TerminateInstances"
"Resource": "*" (applies to all EC2 instances)
"Condition": {"IpAddress": {"aws:SourceIp": "10.100.100.0/24"}} This statement allows users to terminate EC2 instances only if their request originates from an IP address within the 10.100.100.0/24 range (i.e., 10.100.100.0 to 10.100.100.255).
Second Statement (Deny EC2 actions outside us-east-1):

"Effect": "Deny"
"Action": "ec2:*" (applies to all EC2 actions)
"Resource": "*" (applies to all EC2 resources)
"Condition": {"StringNotEquals": {"ec2:Region": "us-east-1"}} This statement denies all EC2 actions if the action is performed in any region other than us-east-1. If the region is us-east-1, the StringNotEquals condition is false, and this Deny statement does not take effect. If the region is, for example, eu-west-1, the condition is true, and the action is denied.

Combined Effect:

- [ ] An explicit Deny in an IAM policy always overrides an Allow.
- [ ] Because of the second statement, users can only perform any EC2 actions (including ec2:TerminateInstances) if those actions are targeted at resources within the us-east-1 Region. All EC2 actions in other regions are denied.
- [ ] For the ec2:TerminateInstances action to be allowed (it must be in us-east-1 due to the second statement), the condition in the first statement must also be met: the user's source IP address must be within the 10.100.100.0/24 range.
- [ ] Evaluating Option C: "Users can terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254."
- [ ] Region Check: The action is in us-east-1. The second statement's Deny condition (StringNotEquals: {"ec2:Region": "us-east-1"}) is false, so the Deny does not apply.
- [ ] Source IP Check: The user's source IP is 10.100.100.254. This IP address is within the 10.100.100.0/24 range specified in the first statement's Allow condition.
- [ ] Conclusion: Both conditions for the Allow are met, and the overriding Deny (for other regions) does not apply. Therefore, users can terminate instances in us-east-1 if their IP is 10.100.100.254.

Why are the other answers wrong?

- [ ] Option A is wrong because: The second statement explicitly denies all EC2 actions in any region except us-east-1. Therefore, users cannot terminate instances outside of us-east-1.
- [ ] Option B is wrong because: The Allow condition in the first statement is based on the user's source IP address (aws:SourceIp), not the IP address of the EC2 instance being terminated. The IP address 10.100.100.1 refers to an instance's potential IP, which is not what the policy conditions on for allowing the action.
- [ ] Option D is wrong because: This directly contradicts the correct analysis of option C. If the conditions in option C are met (action in us-east-1 and source IP is 10.100.100.254), the termination is allowed.

</details>

<details>
  <summary>Question 97</summary>

A company has a large Microsoft SharePoint deployment running on-premises that requires Microsoft Windows shared file storage. The company wants to migrate this workload to the AWS Cloud and is considering various storage options. The storage solution must be highly available and integrated with Active Directory for access control.

Which solution will satisfy these requirements?

- [ ] A. Configure Amazon EFS storage and set the Active Directory domain for authentication.
- [ ] B. Create an SMB file share on an AWS Storage Gateway file gateway in two Availability Zones.
- [ ] C. Create an Amazon S3 bucket and configure Microsoft Windows Server to mount it as a volume.
- [ ] D. Create an Amazon FSx for Windows File Server file system on AWS and set the Active Directory domain for authentication.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create an Amazon FSx for Windows File Server file system on AWS and set the Active Directory domain for authentication.

Why this is the correct answer:

- [ ] Amazon FSx for Windows File Server: This service is specifically designed to provide fully managed, native Microsoft Windows file systems. It supports the SMB (Server Message Block) protocol, Windows NTFS permissions, and Active Directory (AD) integration, which are all critical requirements for a Microsoft SharePoint deployment.
- [ ] Active Directory Integration: FSx for Windows File Server can be joined to your existing Microsoft Active Directory domain (either AWS Managed Microsoft AD or your self-managed AD on-premises or in AWS). This allows you to use your existing AD users, groups, and permissions for access control to the file shares, meeting the requirement for AD integration.
- [ ] High Availability: FSx for Windows File Server supports Multi-AZ deployments, which provide high availability and durability by automatically replicating data across multiple Availability Zones and handling failover. This meets the "highly available" requirement.
- [ ] Suitable for SharePoint: SharePoint heavily relies on Windows shared file storage with specific features and performance characteristics that FSx for Windows File Server is built to provide.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon EFS (Elastic File System) provides file storage primarily using the NFS (Network File System) protocol. While Windows clients can connect to NFS shares, SharePoint typically requires and is optimized for SMB/CIFS file shares with native Windows NTFS permissions and deep Active Directory integration. FSx for Windows File Server is the more appropriate AWS service for this. EFS does not offer the same level of native Windows compatibility and AD integration for NTFS ACLs.
- [ ] Option B is wrong because: An AWS Storage Gateway file gateway can provide an SMB interface, typically with Amazon S3 as the backend storage. While it can integrate with Active Directory, using it as the primary storage solution for a SharePoint deployment being migrated to AWS might not offer the same performance or full feature set as a native Windows file system like FSx for Windows File Server. FSx provides a fully managed Windows Server-based file system directly in the cloud. Deploying a file gateway "in two Availability Zones" refers to the on-premises gateway instances for HA, not typically how the cloud storage itself is made HA for this use case.
- [ ] Option C is wrong because: Amazon S3 is an object storage service. While there are ways (often involving third-party tools or specific configurations like AWS File Gateway) to present S3 storage as a mountable volume to Windows Server, S3 does not natively provide the file system semantics (like file locking, hierarchical directory structures with NTFS permissions) or the performance characteristics required by demanding applications like SharePoint for its core content databases and file storage. This is not a recommended or robust solution for SharePoint's shared file storage needs.

</details>

<details>
  <summary>Question 98</summary>

An image-processing company has a web application that users use to upload images. The application uploads the images into an Amazon S3 bucket. The company has set up S3 event notifications to publish the object creation events to an Amazon Simple Queue Service (Amazon SQS) standard queue. The SQS queue serves as the event source for an AWS Lambda function that processes the images and sends the results to users through email.

Users report that they are receiving multiple email messages for every uploaded image. A solutions architect determines that SQS messages are invoking the Lambda function more than once, resulting in multiple email messages.

What should the solutions architect do to resolve this issue with the LEAST operational overhead?

- [ ] A. Set up long polling in the SQS queue by increasing the ReceiveMessage wait time to 30 seconds.
- [ ] B. Change the SQS standard queue to an SQS FIFO queue. Use the message deduplication ID to discard duplicate messages.
- [ ] C. Increase the visibility timeout in the SQS queue to a value that is greater than the total of the function timeout and the batch window timeout.
- [ ] D. Modify the Lambda function to delete each message from the SQS queue immediately after the message is read before processing.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Increase the visibility timeout in the SQS queue to a value that is greater than the total of the function timeout and the batch window timeout.

Why this is the correct answer:

- [ ] Understanding SQS Visibility Timeout: When an AWS Lambda function (or any consumer) retrieves a message from an SQS queue, the message is not immediately deleted.
- [ ] Instead, it becomes temporarily invisible to other consumers for a configurable period called the "visibility timeout." The Lambda function is expected to process the message and then explicitly delete it from the queue within this timeout period.
- [ ] Cause of Multiple Invocations: If the Lambda function takes longer to process the image and send the email than the current visibility timeout allows, or if it fails during processing and doesn't delete the message, the message will become visible again in the SQS queue after the timeout expires.
- [ ] Another Lambda invocation (or a retry of the same function if configured) will then pick up the same message, leading to it being processed again and a duplicate email being sent.
- [ ] Solution with Visibility Timeout: Increasing the visibility timeout to a value that is comfortably longer than the maximum time the Lambda function needs to successfully process a message (including image processing, sending email, and deleting the message from SQS) will prevent the message from becoming visible again prematurely. This gives a single Lambda invocation enough time to complete its work, ensuring the message is processed only once. This is often the simplest fix with the "LEAST operational overhead" for this specific problem.

Why are the other answers wrong?

- [ ] Option A is wrong because: Long polling (increasing the ReceiveMessage wait time) is a feature that helps reduce the number of empty API calls to SQS, making message retrieval more efficient, especially for queues that are often empty. It does not prevent a message from being processed multiple times if the visibility timeout is too short.
- [ ] Option B is wrong because: While changing to an SQS FIFO queue with message deduplication could prevent duplicate processing, it's a more significant architectural change than simply adjusting the visibility timeout on a standard queue. FIFO queues have different characteristics (e.g., stricter ordering, potentially lower throughput limits without careful design) and might be overkill if the problem is solely due to the visibility timeout. Adjusting the visibility timeout is a simpler first step with less operational overhead.
- [ ] Option D is wrong because: Deleting the message from the SQS queue before processing is complete is a bad practice. If the Lambda function fails during the image processing or email sending steps after the message has been deleted, the event (and the task to process the image) will be lost permanently, leading to unprocessed images rather than duplicate emails. Messages should only be deleted after successful completion of the task.

</details>

<details>
  <summary>Question 99</summary>

A company is implementing a shared storage solution for a gaming application that is hosted in an on-premises data center. The company needs the ability to use Lustre clients to access data. The solution must be fully managed.

Which solution meets these requirements?

- [ ] A. Create an AWS Storage Gateway file gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.
- [ ] B. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.
- [ ] C. Create an Amazon Elastic File System (Amazon EFS) file system, and configure it to support Lustre. Attach the file system to the origin server. Connect the application server to the file system.
- [ ] D. Create an Amazon FSx for Lustre file system. Attach the file system to the origin server. Connect the application server to the file system.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create an Amazon FSx for Lustre file system. Attach the file system to the origin server. Connect the application server to the file system.

Why this is the correct answer:

- [ ] Amazon FSx for Lustre: This AWS service provides fully managed, high-performance file systems that are optimized for compute-intensive workloads. It is based on Lustre, a popular open-source parallel file system. This directly meets the requirement for a storage solution that "Lustre clients" can access.
- [ ] Fully Managed: Amazon FSx for Lustre is a fully managed service. AWS handles the setup, patching, and maintenance of the file system hardware and software, which aligns with the requirement that "The solution must be fully managed."
- [ ] Access from On-Premises: FSx for Lustre file systems can be accessed from on-premises clients (like the gaming application servers in the company's data center) over an AWS Direct Connect connection or an AWS VPN. The phrase "Attach the file system to the origin server" implies making it accessible to the on-premises application server.

Why are the other answers wrong?

- [ ] Option A is wrong because: An AWS Storage Gateway file gateway provides a file interface (NFS or SMB) to data stored in Amazon S3. It does not provide a Lustre file system interface or support Lustre clients.
- [ ] Option B is wrong because: Creating a Windows file share on an Amazon EC2 instance uses the SMB/CIFS protocol, not Lustre. Furthermore, this solution is not "fully managed" in the same way as FSx for Lustre; the company would be responsible for managing the EC2 instance, the Windows operating system, and the file share configuration.
- [ ] Option C is wrong because: Amazon Elastic File System (EFS) provides scalable file storage primarily using the NFS protocol. EFS does not natively support the Lustre file system or protocol, nor can it be "configured to support Lustre." For Lustre-specific needs, Amazon FSx for Lustre is the appropriate AWS service.

</details>

<details>
  <summary>Question 100</summary>

A company's containerized application runs on an Amazon EC2 instance. The application needs to download security certificates before it can communicate with other business applications. The company wants a highly secure solution to encrypt and decrypt the certificates in near real time. The solution also needs to store data in highly available storage after the data is encrypted.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Create AWS Secrets Manager secrets for encrypted certificates. Manually update the certificates as needed. Control access to the data by using fine-grained IAM access.
- [ ] B. Create an AWS Lambda function that uses the Python cryptography library to receive and perform encryption operations. Store the function in an Amazon S3 bucket.
- [ ] C. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon S3.
- [ ] D. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon Elastic Block Store (Amazon EBS) volumes.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon S3.

Why this is the correct answer:

- [ ] AWS Key Management Service (AWS KMS) for Secure Encryption/Decryption: AWS KMS is a managed service that makes it easy to create and control the encryption keys used to encrypt your data. Using a customer-managed key (CMK) in KMS gives you control over the key material and its usage policies. KMS performs cryptographic operations (encrypt, decrypt) in a secure manner, often utilizing FIPS 140-2 validated hardware security modules. This addresses the need for a "highly secure solution to encrypt and decrypt the certificates in near real time."
- [ ] IAM Role for EC2 Access to KMS: The EC2 instance running the containerized application can be granted permissions to use the specific KMS key for encryption and decryption operations via an IAM role attached to the instance. This is a secure way to grant permissions without hardcoding credentials.
- [ ] Amazon S3 for Highly Available Storage: After the certificates (or sensitive data derived from them) are encrypted using KMS, they can be stored in Amazon S3. S3 is designed for high durability (99.999999999%) and availability, automatically storing data across multiple Availability Zones within a region. This meets the requirement to "store data in highly available storage after the data is encrypted."
- [ ] Least Operational Overhead: This solution leverages fully managed AWS services (KMS for key management and cryptographic operations, S3 for storage, IAM for permissions). This significantly minimizes the operational overhead compared to building and managing custom cryptographic solutions or less available storage options.

Why are the other answers wrong?

- [ ] Option A is wrong because: While AWS Secrets Manager is excellent for storing and managing secrets like full certificates or private keys, the question emphasizes the need to "encrypt and decrypt the certificates in near real time," suggesting the application might be handling sensitive components of certificates or data related to them that require cryptographic operations. KMS is the service for performing these encryption/decryption operations using managed keys. Secrets Manager is more about storing the final secret value. "Manually update the certificates as needed" also points to operational overhead that might be reduced with other automated solutions for certificate lifecycle, though that's not the primary focus of the encryption/decryption requirement here.
- [ ] Option B is wrong because: Creating a custom AWS Lambda function with a Python cryptography library to perform encryption operations means the company would be responsible for managing the cryptographic implementation, key management, security of the keys, and the operational aspects of the Lambda function. This introduces significant development and operational overhead and potential security risks compared to using a purpose-built, managed, and validated service like AWS KMS.
- [ ] Option D is wrong because: While using AWS KMS with an EC2 role for encryption operations is correct, storing the encrypted data on Amazon EBS volumes is less ideal for "highly available storage" compared to Amazon S3 for this use case. Standard EBS volumes are zonal resources (tied to a specific Availability Zone). While EBS is reliable, achieving high availability across AZs for EBS-stored data requires additional mechanisms like replication or frequent snapshots and restoration procedures, which adds operational overhead. S3 provides high availability and durability across multiple AZs by default for object storage.

</details>

</details>

<details>
  <summary>==Questions 101-110==</summary>

<details>
  <summary>Question 101</summary>
 
A solutions architect is designing a VPC with public and private subnets. The VPC and subnets use IPv4 CIDR blocks. There is one public subnet and one private subnet in each of three Availability Zones (AZs) for high availability. An internet gateway is used to provide internet access for the public subnets. The private subnets require access to the internet to allow Amazon EC2 instances to download software updates.

What should the solutions architect do to enable Internet access for the private subnets?

- [ ] A. Create three NAT gateways, one for each public subnet in each AZ. Create a private route table for each AZ that forwards non-VPC traffic to the NAT gateway in its AZ.
- [ ] B. Create three NAT instances, one for each private subnet in each AZ. Create a private route table for each AZ that forwards non-VPC traffic to the NAT instance in its AZ.
- [ ] C. Create a second internet gateway on one of the private subnets. Update the route table for the private subnets that forward non-VPC traffic to the private internet gateway.
- [ ] D. Create an egress-only internet gateway on one of the public subnets. Update the route table for the private subnets that forward non-VPC traffic to the egress-only Internet gateway.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create three NAT gateways, one for each public subnet in each AZ. Create a private route table for each AZ that forwards non-VPC traffic to the NAT gateway in it

Why this is the correct answer:

- [ ] NAT Gateways for Private Subnet Outbound Access: Amazon EC2 instances in private subnets need a way to access the internet for tasks like downloading software updates without being directly exposed to incoming traffic from the internet. NAT (Network Address Translation) gateways are designed for this purpose. They allow instances in private subnets to initiate outbound IPv4 traffic to the internet but prevent unsolicited inbound connections.
- [ ] High Availability Across Availability Zones: The architecture has three Availability Zones (AZs) for high availability. To ensure that internet access for private subnets remains available even if one AZ has an issue, it's a best practice to deploy a NAT gateway in a public subnet in each AZ.
- [ ] Zonal Routing for Resilience and Cost: Each private subnet (or the group of private subnets within a specific AZ) should have its route table configured to direct internet-bound traffic (0.0.0.0/0) to the NAT gateway located in its own AZ. This avoids cross-AZ data transfer costs for NAT gateway traffic and ensures that the failure of a NAT gateway in one AZ does not impact the internet connectivity of private subnets in other AZs.

Why are the other answers wrong?

- [ ] Option B is wrong because: While NAT instances can also provide NAT functionality, NAT gateways are AWS-managed, offering higher availability, greater bandwidth, and less administrative effort (no need to patch or manage the instance OS) compared to self-managed NAT instances. For a new design emphasizing high availability, NAT gateways are preferred. Also, placing NAT instances in private subnets is incorrect; they need to be in a public subnet with a route to the internet gateway.
- [ ] Option C is wrong because: A VPC can have only one Internet Gateway (IGW) attached to it. It's not possible to create a "second internet gateway." Furthermore, Internet Gateways are for providing direct, two-way internet access, typically for public subnets, not for providing outbound-only access for private subnets.
- [ ] Option D is wrong because: An Egress-Only Internet Gateway is used specifically for IPv6 traffic. It allows instances in your VPC to initiate outbound connections over IPv6 to the internet while preventing the internet from initiating IPv6 connections to your instances. The question specifies that the VPC and subnets use "IPv4 CIDR blocks," and the common need for software updates implies IPv4 internet access. An egress-only internet gateway does not provide IPv4 outbound connectivity.

</details>  

<details>
  <summary>Question 102</summary>

A company wants to migrate an on-premises data center to AWS. The data center hosts an SFTP server that stores its data on an NFS-based file system. The server holds 200 GB of data that needs to be transferred. The server must be hosted on an Amazon EC2 instance that uses an Amazon Elastic File System (Amazon EFS) file system.

Which combination of steps should a solutions architect take to automate this task? (Choose two.)

- [ ] A. Launch the EC2 instance into the same Availability Zone as the EFS file system.
- [ ] B. Install an AWS DataSync agent in the on-premises data center.
- [ ] C. Create a secondary Amazon Elastic Block Store (Amazon EBS) volume on the EC2 instance for the data.
- [ ] D. Manually use an operating system copy command to push the data to the EC2 instance.
- [ ] E. Use AWS DataSync to create a suitable location configuration for the on-premises SFTP server.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Install an AWS DataSync agent in the on-premises data center.
- [ ] E. Use AWS DataSync to create a suitable location configuration for the on-premises SFTP server.

Why these are the correct answers:

This solution focuses on automating the transfer of 200 GB of data from an on-premises NFS file system (backing an SFTP server) to an Amazon EFS file system in AWS.

B. Install an AWS DataSync agent in the on-premises data center.
- [ ] AWS DataSync Agent: To use AWS DataSync for transferring data from an on-premises location, you need to deploy a DataSync agent (a virtual machine) in your on-premises environment.
- [ ] This agent will access your on-premises NFS file system and manage the data transfer to AWS.

E. Use AWS DataSync to create a suitable location configuration for the on-premises SFTP server.
- [ ] DataSync Locations and Task: Once the agent is deployed and activated, you configure AWS DataSync by creating a source location that points to your on-premises NFS file system (which backs the SFTP server) and a destination location that points to your Amazon EFS file system in AWS.
- [ ] Then, you create a DataSync task to manage the automated transfer of data between these locations. DataSync handles the scheduling, monitoring, data validation, and optimization of the transfer.
- [ ] This combination provides an automated, efficient, and managed way to transfer the data.

Why are the other answers wrong?

- [ ] Option A is wrong because: While an EC2 instance needs to be in a VPC with mount targets for the EFS file system (which exist in specific AZs), EFS itself is a regional service designed to be accessible from any AZ within the region where it's created. This step is related to how the EC2 instance accesses EFS after migration, not directly a step in automating the data transfer from on-premises. The core of the automation is DataSync.
- [ ] Option C is wrong because: The requirement clearly states that the new SFTP server hosted on an EC2 instance must use an Amazon EFS file system, not an EBS volume, for its data. Creating an EBS volume contradicts this requirement.
- [ ] Option D is wrong because: Manually using operating system copy commands (like scp, rsync, or cp over an NFS mount) is not an "automated task" in the context of a managed migration service like DataSync. Manual copies lack the built-in scheduling, monitoring, error handling, data validation, and transfer optimization features that DataSync provides, and they would require more manual intervention, especially for 200 GB of data.

</details>

<details>
  <summary>Question 103</summary>

A company has an AWS Glue extract, transform, and load (ETL) job that runs every day at the same time. The job processes XML data that is in an Amazon S3 bucket. New data is added to the S3 bucket every day. A solutions architect notices that AWS Glue is processing all the data during each run.

What should the solutions architect do to prevent AWS Glue from reprocessing old data?

- [ ] A. Edit the job to use job bookmarks.
- [ ] B. Edit the job to delete data after the data is processed.
- [ ] C. Edit the job by setting the NumberOfWorkers field to 1.
- [ ] D. Use a FindMatches machine learning (ML) transform.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Edit the job to use job bookmarks.

Why this is the correct answer:

- [ ] AWS Glue Job Bookmarks: AWS Glue job bookmarks are a feature that allows AWS Glue to keep track of data that has already been processed during previous runs of an ETL job. When job bookmarks are enabled and used correctly, on subsequent runs, the Glue job will only process new data that has arrived in the Amazon S3 bucket (or other source) since the last successful run.
- [ ] Preventing Reprocessing of Old Data: This directly addresses the problem described: "AWS Glue is processing all the data during each run." By enabling job bookmarks, the job will skip the old, already processed data and only focus on the new XML data added daily.
- [ ] Suitable for Incremental Processing: This is the standard mechanism in AWS Glue for achieving incremental processing of data from sources like S3 where new files are added over time.

Why are the other answers wrong?

- [ ] Option B is wrong because: Editing the job to delete data after it is processed might not be a desirable or safe approach. The source data in S3 might be needed for other purposes, auditing, re-running the ETL job in case of issues, or historical record-keeping. Deleting source data is a destructive action and not the primary method for preventing reprocessing.
- [ ] Option C is wrong because: The NumberOfWorkers field in an AWS Glue job configuration relates to the number of Data Processing Units (DPUs) allocated to the job, which affects its processing capacity and parallelism. Setting this field to 1 would reduce the job's performance and make it run slower, but it would not, by itself, prevent the job from reprocessing old data if job bookmarks are not enabled.
- [ ] Option D is wrong because: The FindMatches ML transform in AWS Glue is used for identifying duplicate or matching records within a dataset (e.g., for data deduplication or record linkage). It is not a feature designed to prevent an ETL job from reprocessing input files that have already been processed in previous runs.

</details>

<details>
  <summary>Question 104</summary>
 
A solutions architect must design a highly available infrastructure for a website. The website is powered by Windows web servers that run on Amazon EC2 instances. The solutions architect must implement a solution that can mitigate a large-scale DDoS attack that originates from thousands of IP addresses. Downtime is not acceptable for the website.

Which actions should the solutions architect take to protect the website from such an attack? (Choose two.)

- [ ] A. Use AWS Shield Advanced to stop the DDoS attack.
- [ ] B. Configure Amazon GuardDuty to automatically block the attackers.
- [ ] C. Configure the website to use Amazon CloudFront for both static and dynamic content.
- [ ] D. Use an AWS Lambda function to automatically add attacker IP addresses to VPC network ACLS.
- [ ] E. Use EC2 Spot Instances in an Auto Scaling group with a target tracking scaling policy that is set to 80% CPU utilization.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Use AWS Shield Advanced to stop the DDoS attack.
- [ ] C. Configure the website to use Amazon CloudFront for both static and dynamic content.

Why these are the correct answers:

This solution combines a CDN with advanced DDoS protection services:

A. Use AWS Shield Advanced to stop the DDoS attack.
- [ ] Enhanced DDoS Protection: AWS Shield Advanced provides significantly more robust protection against larger and more sophisticated Distributed Denial of Service (DDoS) attacks compared to the default AWS Shield Standard.
- [ ] It offers detection and mitigation against volumetric network layer attacks, state-exhaustion attacks, and application layer attacks (when used with AWS WAF).
- [ ] Access to DDoS Response Team (DRT): Shield Advanced customers get 24/7 access to the AWS DDoS Response Team (DRT) for assistance during an attack, which is crucial when "Downtime is not acceptable."
- [ ] Cost Protection: It also provides cost protection against usage spikes on protected resources like CloudFront, ELB, and EC2 that might result from a DDoS attack.

C. Configure the website to use Amazon CloudFront for both static and dynamic content.
- [ ] Distributed Attack Absorption: Amazon CloudFront is a global content delivery network (CDN) with a large network of edge locations.
- [ ] This distributed infrastructure can absorb many common infrastructure-level DDoS attacks (like SYN floods or UDP reflection attacks) at the edge, preventing them from reaching your origin servers.
- [ ] Reduced Attack Surface: By serving content from the edge, CloudFront reduces the direct exposure of your origin EC2 instances.
- [ ] Integration with WAF and Shield: CloudFront integrates seamlessly with AWS WAF (for application-layer filtering) and AWS Shield, providing a layered security approach at the edge.
- [ ] This combination is highly effective in mitigating DDoS attacks.

Why are the other answers wrong?

- [ ] Option B is wrong because: Amazon GuardDuty is a threat detection service that monitors for malicious activity and unauthorized behavior within your AWS environment. While it can help detect indicators of a DDoS attack or compromised resources, GuardDuty itself does not automatically block attackers or provide DDoS mitigation. It provides findings that can then be used to trigger other responses, but it's not a primary mitigation tool.
- [ ] Option D is wrong because: While it's possible to build a custom solution using AWS Lambda to dynamically update VPC Network ACLs (NACLs) based on detected malicious IP addresses, this approach has limitations for "large-scale DDoS attacks originating from thousands of IP addresses." NACLs have rule limits, and managing dynamic updates at this scale can be complex, slow to react, and operationally burdensome compared to dedicated DDoS mitigation services like AWS Shield and AWS WAF.
- [ ] Option E is wrong because: EC2 Spot Instances can be terminated by AWS with a two-minute notice if AWS needs the capacity back. Relying on Spot Instances for a critical website where "Downtime is not acceptable" is risky, especially during a DDoS attack which could cause unpredictable load and instance behavior. While Auto Scaling helps with legitimate traffic, Spot Instances are not a DDoS mitigation strategy.

</details>

<details>
  <summary>Question 105</summary>

A company is preparing to deploy a new serverless workload. A solutions architect must use the principle of least privilege to configure permissions that will be used to run an AWS Lambda function. An Amazon EventBridge (Amazon CloudWatch Events) rule will invoke the function.

Which solution meets these requirements?

- [ ] A. Add an execution role to the function with lambda:InvokeFunction as the action and * as the principal.
- [ ] B. Add an execution role to the function with lambda:InvokeFunction as the action and Service: https://www.google.com/url?sa=E&amp;source=gmail&amp;q=lambda.amazonaws.com as the principal.
- [ ] C. Add a resource-based policy to the function with lambda:* as the action and Service: https://www.google.com/url?sa=E&amp;source=gmail&amp;q=events.amazonaws.com as the principal.
- [ ] D. Add a resource-based policy to the function with lambda:InvokeFunction as the action and Service: https://www.google.com/url?sa=E&amp;source=gmail&amp;q=events.amazonaws.com as the principal.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Add a resource-based policy to the function with lambda:InvokeFunction as the action and Service: https://www.google.com/url?sa=E&amp;source=gmail&amp;q=events.amazonaws.com as the principal.

Why this is the correct answer:

- [ ] Resource-Based Policy for Lambda Invocation: To allow an AWS service like Amazon EventBridge to invoke an AWS Lambda function, you need to grant permission to the EventBridge service principal in the Lambda function's resource-based policy (also known as the function policy).
- [ ] Principle of Least Privilege - Action: The specific action that EventBridge needs to invoke the Lambda function is lambda:InvokeFunction. Granting only this action, rather than a wildcard like lambda:*, adheres to the principle of least privilege.
- [ ] Principle of Least Privilege - Principal: The principal that needs this permission is the EventBridge service itself, which is specified as Service: events.amazonaws.com.
- [ ] This configuration specifically allows only the EventBridge service to invoke this particular Lambda function, which is the most secure and least privileged setup for this scenario.

Why are the other answers wrong?

- [ ] Option A is wrong because: It describes configuring an IAM execution role, which defines what the Lambda function can do (its permissions to access other AWS services), not who can invoke the Lambda function. The Principal in an execution role's trust policy should be Service: lambda.amazonaws.com (allowing the Lambda service to assume the role), not * (which is too permissive). lambda:InvokeFunction is an action granted to an invoker, not typically an action performed by the function itself through its execution role in this context.
- [ ] Option B is wrong because: This also incorrectly focuses on the execution role for granting invocation permission. The principal Service: lambda.amazonaws.com is correct for the trust policy of an execution role, but this doesn't grant EventBridge permission to invoke the function.
- [ ] Option C is wrong because: While using a resource-based policy for the Lambda function and specifying Service: events.amazonaws.com as the principal are correct steps, granting the lambda:* action is too permissive. This would allow EventBridge to perform all possible Lambda actions on the function (e.g., delete the function, update its configuration), which violates the principle of least privilege. Only lambda:InvokeFunction is required.

</details>

<details>
  <summary>Question 106</summary>

A company is preparing to store confidential data in Amazon S3. For compliance reasons, the data must be encrypted at rest. Encryption key usage must be logged for auditing purposes. Keys must be rotated every year.

Which solution meets these requirements and is the MOST operationally efficient?

- [ ] A. Server-side encryption with customer-provided keys (SSE-C)
- [ ] B. Server-side encryption with Amazon S3 managed keys (SSE-S3)
- [ ] C. Server-side encryption with AWS KMS keys (SSE-KMS) with manual rotation
- [ ] D. Server-side encryption with AWS KMS keys (SSE-KMS) with automatic rotation

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Server-side encryption with AWS KMS keys (SSE-KMS) with automatic rotation

Why this is the correct answer:

- [ ] Server-Side Encryption with AWS KMS keys (SSE-KMS): This option encrypts data at rest in Amazon S3 using encryption keys managed in AWS Key Management Service (KMS). This provides strong encryption and allows for centralized key management.
- [ ] Logging of Key Usage: AWS KMS is integrated with AWS CloudTrail. All API calls made to KMS, including when S3 uses a KMS key for encryption or decryption (as with SSE-KMS), are logged in CloudTrail. This provides an audit trail of key usage, meeting the requirement for logging.
- [ ] Automatic Key Rotation: AWS KMS supports automatic annual rotation of the key material for customer master keys (CMKs) that are customer managed (you create them in KMS, and AWS rotates the backing key material). This meets the requirement that "Keys must be rotated every year" with minimal manual intervention.
- [ ] MOST Operationally Efficient: Using SSE-KMS with automatic key rotation is highly operationally efficient. AWS manages the encryption process, the secure storage of the keys, the automatic rotation of key material, and the logging of key usage through CloudTrail. This significantly reduces the operational burden on the company compared to managing keys and rotation manually.

Why are the other answers wrong?

- [ ] Option A is wrong because: With Server-Side Encryption with Customer-Provided Keys (SSE-C), the company is responsible for managing the encryption keys themselves. This includes key generation, secure storage, rotation, and providing the key with every request to S3. This involves significant operational overhead and complexity, and AWS does not manage or log these customer-provided keys in KMS/CloudTrail in the same way.
- [ ] Option B is wrong because: Server-Side Encryption with Amazon S3 Managed Keys (SSE-S3) uses encryption keys that are fully managed by Amazon S3. While this is very simple and encrypts data at rest, it provides less control and less detailed audit information about key usage compared to SSE-KMS. The rotation of these S3-managed keys is handled by AWS, but the customer has no direct control or visibility into this process, and the auditing of key usage for compliance might not be as robust as with SSE-KMS.
- [ ] Option C is wrong because: While SSE-KMS is the correct encryption method, relying on "manual rotation" of KMS keys introduces operational overhead. The administrator would need to remember to rotate the keys annually and perform the rotation steps. Automatic rotation (as in option D) is more operationally efficient and less prone to human error for meeting the annual rotation requirement.

</details>

<details>
  <summary>Question 107</summary>

A bicycle sharing company is developing a multi-tier architecture to track the location of its bicycles during peak operating hours. The company wants to use these data points in its existing analytics platform. A solutions architect must determine the most viable multi-tier option to support this architecture. The data points must be accessible from the REST API.

Which action meets these requirements for storing and retrieving location data?

- [ ] A. Use Amazon Athena with Amazon S3.
- [ ] B. Use Amazon API Gateway with AWS Lambda.
- [ ] C. Use Amazon QuickSight with Amazon Redshift.
- [ ] D. Use Amazon API Gateway with Amazon Kinesis Data Analytics.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use Amazon API Gateway with AWS Lambda.

Why this is the correct answer:

This question focuses on the components needed for storing and retrieving location data that must be accessible via a REST API, and then subsequently used in an analytics platform.

- [ ] Amazon API Gateway for REST API: Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. This directly addresses the requirement that "The data points must be accessible from the REST API." API Gateway would serve as the frontend for these API calls.
- [ ] AWS Lambda for Backend Logic (Storing and Retrieving): AWS Lambda functions can be integrated with API Gateway to provide the backend logic. When API Gateway receives a request (e.g., to store a new location or retrieve a location), it can invoke a Lambda function. This Lambda function would then contain the code to interact with a suitable database or storage service (like Amazon DynamoDB, which is often used for such use cases due to its scalability and low latency, though the specific database isn't named in this option) to perform the actual "storing and retrieving location data."
- [ ] Multi-Tier Architecture and Viability: This combination forms a scalable, serverless backend, fitting a multi-tier architecture. The data, once stored by this mechanism, can then be fed into the "existing analytics platform."

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Athena is a serverless query service primarily used for analyzing data in Amazon S3 using standard SQL. While S3 could store location data and Athena could analyze it (fitting the "analytics platform" part), Athena is not designed for real-time transactional storing and retrieving of individual data points via a low-latency REST API, especially for tracking active bicycle locations.
- [ ] Option C is wrong because: Amazon QuickSight is a business intelligence (BI) service used for creating visualizations and dashboards. Amazon Redshift is a data warehousing service. Both are components of an analytics platform but are not the primary tools for building a REST API to store and retrieve individual, real-time location data points for a tracking system.
- [ ] Option D is wrong because: While Amazon API Gateway is correct for the REST API frontend, Amazon Kinesis Data Analytics is designed for real-time processing and analysis of streaming data. It's not a storage service from which individual data points would typically be retrieved via an API in the context of a simple store/retrieve pattern. Data would flow through Kinesis Data Analytics for analysis, not be stored in it for API-based retrieval of specific records.

</details>

<details>
  <summary>Question 108</summary>

A company has an automobile sales website that stores its listings in a database on Amazon RDS. When an automobile is sold, the listing needs to be removed from the website and the data must be sent to multiple target systems.

Which design should a solutions architect recommend?

- [ ] A. Create an AWS Lambda function triggered when the database on Amazon RDS is updated to send the information to an Amazon Simple Queue Service (Amazon SQS) queue for the targets to consume.
- [ ] B. Create an AWS Lambda function triggered when the database on Amazon RDS is updated to send the information to an Amazon Simple Queue Service (Amazon SQS) FIFO queue for the targets to consume.
- [ ] C. Subscribe to an RDS event notification and send an Amazon Simple Queue Service (Amazon SQS) queue fanned out to multiple Amazon Simple Notification Service (Amazon SNS) topics. Use AWS Lambda functions to update the targets.
- [ ] D. Subscribe to an RDS event notification and send an Amazon Simple Notification Service (Amazon SNS) topic fanned out to multiple Amazon Simple Queue Service (Amazon SQS) queues. Use AWS Lambda functions to update the targets.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Create an AWS Lambda function triggered when the database on Amazon RDS is updated to send the information to an Amazon Simple Queue Service (Amazon SQS) queue for the targets to consume.

Why this is the correct answer:

This question describes a scenario where an event (an automobile sale, resulting in a database update) needs to trigger multiple downstream actions: removing the listing from the website and sending data to several target systems.

- [ ] Triggering Mechanism (Interpreted): The phrase "triggered when the database on Amazon RDS is updated" likely implies that the application logic that records the sale in the RDS database will also initiate the trigger for an AWS Lambda function. (Direct RDS triggers for data manipulation events to Lambda are not a standard out-of-the-box feature; it's usually application-driven or via more complex Change Data Capture setups).
- [ ] AWS Lambda for Orchestration: The triggered Lambda function can perform the immediate necessary actions. This includes:
- [ ] Logic to remove the listing from the website (e.g., by updating a cache, calling a website API, or modifying another data store that the website reads from).
- [ ] Sending the relevant sales data as a message to an Amazon SQS queue.
- [ ] Amazon SQS for Decoupling and Multiple Consumers: The SQS queue acts as a durable and reliable buffer. "Multiple target systems" can then have their own consumer applications (which could also be other Lambda functions or EC2-based applications) that independently poll this SQS queue to receive and process the sales data. This decouples the initial sales event processing from the individual target systems, allowing them to process data at their own pace and providing resilience.
- [ ] This approach provides a decoupled architecture where the initial Lambda handles primary post-sale actions and SQS ensures reliable delivery of sales information to various downstream systems.

Why are the other answers wrong?

- [ ] Option B is wrong because: While using an SQS queue is good, specifying an SQS FIFO (First-In, First-Out) queue might be overly restrictive if strict ordering of sales data processing by all target systems is not explicitly required. Standard SQS queues offer higher throughput. If ordering is critical for all consumers, FIFO would be considered, but the question doesn't stress this as a primary requirement over the general mechanism. The core solution in A (Lambda + SQS) is sound.
- [ ] Option C is wrong because:
RDS event notifications typically relate to operational events of the DB instance (e.g., backups, failovers, parameter group changes), not to data modification events within tables like a sale being recorded.
The fan-out pattern is described incorrectly: SQS fanning out to multiple SNS topics is not the standard way to distribute a single message to multiple distinct processing queues. The typical pattern is SNS fanning out to multiple SQS queues.
- [ ] Option D is wrong because: Similar to option C, relying on "RDS event notification" to capture a business event like a sale (which is a data change) is generally not how RDS event notifications work. While the subsequent pattern of SNS fanning out to multiple SQS queues for different target systems (each consumed by Lambda) is a valid and robust fan-out architecture, the initial trigger mechanism via RDS operational event notifications is likely incorrect for this use case. Option A's implied application-driven Lambda trigger is more plausible for acting on a data update.

Given the options, Option A provides a viable (with interpretation of the trigger) and straightforward approach for handling the post-sale tasks using Lambda and SQS for distribution to multiple systems.

</details>

<details>
  <summary>Question 109</summary>

A company needs to store data in Amazon S3 and must prevent the data from being changed. The company wants new objects that are uploaded to Amazon S3 to remain unchangeable for a nonspecific amount of time until the company decides to modify the objects. Only specific users in the company's AWS account can have the ability to delete the objects.

What should a solutions architect do to meet these requirements?

- [ ] A. Create an S3 Glacier vault. Apply a write-once, read-many (WORM) vault lock policy to the objects.
- [ ] B. Create an S3 bucket with S3 Object Lock enabled. Enable versioning. Set a retention period of 100 years. Use governance mode as the S3 bucket's default retention mode for new objects.
- [ ] C. Create an S3 bucket. Use AWS CloudTrail to track any S3 API events that modify the objects. Upon notification, restore the modified objects from any backup versions that the company has.
- [ ] D. Create an S3 bucket with S3 Object Lock enabled. Enable versioning. Add a legal hold to the objects. Add the s3:PutObjectLegalHold permission to the IAM policies of users who need to delete the objects.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create an S3 bucket with S3 Object Lock enabled. Enable versioning. Add a legal hold to the objects. Add the s3:PutObjectLegalHold permission to the IAM policies of users who need to delete the objects.

Why this is the correct answer:

- [ ] S3 Object Lock for Immutability: The requirement is to "prevent the data from being changed." S3 Object Lock provides Write-Once-Read-Many (WORM) capabilities to protect objects from being deleted or overwritten.
- [ ] Versioning Prerequisite: S3 Object Lock requires S3 Versioning to be enabled on the bucket.
- [ ] Legal Hold for Indefinite, Controllable Retention: The company wants objects to remain unchangeable "for a nonspecific amount of time until the company decides to modify the objects." A legal hold, which is a feature of S3 Object Lock, provides this exact functionality. A legal hold prevents an object version from being overwritten or deleted, but unlike a retention period, a legal hold does not have an expiration date. It remains in effect until explicitly removed.
- [ ] Controlled Deletion/Modification via Legal Hold Removal: "Only specific users... can have the ability to delete the objects." To delete an object under a legal hold, the legal hold must first be removed. The permission s3:PutObjectLegalHold allows users to place and remove legal holds on objects. By granting this permission only to the specific users, the company controls who can effectively make an object deletable (by first removing the legal hold).
- [ ] This solution meets all requirements: data is unchangeable, the unchangeable period is indefinite until a decision is made, and specific users control the ability to lift this protection.

Why are the other answers wrong?

- [ ] Option A is wrong because: While S3 Glacier Vault Lock can enforce WORM policies, it applies to S3 Glacier vaults, which are for archival storage. The question implies uploads to standard "Amazon S3" buckets and doesn't necessarily dictate archival from the outset. S3 Object Lock is a feature for standard S3 buckets.
- [ ] Option B is wrong because: Setting a fixed retention period (e.g., 100 years) with S3 Object Lock does not meet the requirement for the objects to remain unchangeable for a "nonspecific amount of time until the company decides to modify the objects." A legal hold provides this indefinite protection that can be explicitly removed when needed. Governance mode, while allowing overrides with special permissions, is tied to a specific retention period.
- [ ] Option C is wrong because: Using AWS CloudTrail to track modifications and then restoring from backups is a reactive approach (detect and recover), not a preventative one. The requirement is to "prevent the data from being changed" in the first place.

</details>

<details>
  <summary>Question 110</summary>

A social media company allows users to upload images to its website. The website runs on Amazon EC2 instances. During upload requests, the website resizes the images to a standard size and stores the resized images in Amazon S3. Users are experiencing slow upload requests to the website. The company needs to reduce coupling within the application and improve website performance. A solutions architect must design the most operationally efficient process for image uploads.

Which combination of actions should the solutions architect take to meet these requirements? (Choose two.)

- [ ] A. Configure the application to upload images to S3 Glacier.
- [ ] B. Configure the web server to upload the original images to Amazon S3.
- [ ] C. Configure the application to upload images directly from each user's browser to Amazon S3 through the use of a presigned URL
- [ ] D. Configure S3 Event Notifications to invoke an AWS Lambda function when an image is uploaded. Use the function to resize the image.
- [ ] E. Create an Amazon EventBridge (Amazon CloudWatch Events) rule that invokes an AWS Lambda function on a schedule to resize uploaded images.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Configure the application to upload images directly from each user's browser to Amazon S3 through the use of a presigned URL
- [ ] D. Configure S3 Event Notifications to invoke an AWS Lambda function when an image is uploaded. Use the function to resize the image.

Why these are the correct answers:

This solution aims to offload work from the EC2 instances and process images asynchronously, improving website performance and operational efficiency.

C. Configure the application to upload images directly from each user's browser to Amazon S3 through the use of a presigned URL
- [ ] Improved Website Performance: Currently, the EC2 instances handle the image uploads, which contributes to slow requests.
- [ ] By allowing users' browsers to upload images directly to an Amazon S3 bucket using a presigned URL, the web servers (EC2 instances) are bypassed for the actual file transfer.
- [ ] This significantly reduces the load on the web servers, freeing them up to handle other requests and improving the perceived performance for the user during the upload process.
- [ ] Reduced Coupling: This decouples the web servers from the responsibility of handling the raw image uploads.

D. Configure S3 Event Notifications to invoke an AWS Lambda function when an image is uploaded. Use the function to resize the image.
- [ ] Asynchronous Image Resizing: Instead of the EC2 instances resizing images synchronously during the upload request (which causes slowness), this approach processes images asynchronously.
- [ ] When an original image is uploaded to S3 (as per option C), an S3 event notification can automatically trigger an AWS Lambda function.
- [ ] Serverless and Scalable Resizing: The Lambda function can then perform the image resizing.
- [ ] Lambda is serverless, scales automatically with the number of uploads, and is operationally efficient as you don't manage servers for this processing.
- [ ] The resized image can then be stored back in S3. This decouples the resizing logic from the web application.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon S3 Glacier is a low-cost storage class designed for data archiving where retrieval times of minutes to hours are acceptable. It is not suitable for storing images that need to be processed (resized) and potentially accessed shortly after upload.
- [ ] Option B is wrong because: While having the web server upload the original images to S3 is a step towards using S3, it doesn't fundamentally solve the problem of "slow upload requests" if the web server is still the intermediary handling the full upload from the user's browser and potentially doing synchronous resizing. Option C, direct browser upload to S3, is more effective at offloading the web server.
- [ ] Option E is wrong because: Using a scheduled EventBridge rule to trigger a Lambda function for image resizing means that images would not be processed immediately after upload. Instead, they would be processed in batches when the schedule runs. This could lead to delays in users seeing their resized images and doesn't fit the typical expectation of quick processing after an upload. S3 event notifications (as in option D) provide near real-time, event-driven processing.


</details>

</details>

<details>
  <summary>==Questions 111-120==</summary>

<details>
  <summary>Question 111</summary>

A company recently migrated a message processing system to AWS. The system receives messages into an ActiveMQ queue running on an Amazon EC2 instance. Messages are processed by a consumer application running on Amazon EC2. The consumer application processes the messages and writes results to a MySQL database running on Amazon EC2. The company wants this application to be highly available with low operational complexity.

Which architecture offers the HIGHEST availability?

- [ ] A. Add a second ActiveMQ server to another Availability Zone. Add an additional consumer EC2 instance in another Availability Zone. Replicate the MySQL database to another Availability Zone.
- [ ] B. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an additional consumer EC2 instance in another Availability Zone. Replicate the MySQL database to another Availability Zone.
- [ ] C. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an additional consumer EC2 instance in another Availability Zone. Use Amazon RDS for MySQL with Multi-AZ enabled.
- [ ] D. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an Auto Scaling group for the consumer EC2 instances across two Availability Zones. Use Amazon RDS for MySQL with Multi-AZ enabled.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Use Amazon MQ with active/standby brokers configured across two Availability Zones. Add an Auto Scaling group for the consumer EC2 instances across two Availability Zones. Use Amazon RDS for MySQL with Multi-AZ enabled.

Why this is the correct answer:

This solution leverages managed services and Auto Scaling for the highest availability and reduced operational complexity:

- [ ] Amazon MQ for Message Broker: Migrating the self-managed ActiveMQ on EC2 to Amazon MQ (which supports ActiveMQ as an engine) provides a managed message broker service. Configuring Amazon MQ with active/standby brokers across two - [ ] Availability Zones ensures high availability for the message queue itself, with automatic failover. This significantly reduces operational complexity compared to managing ActiveMQ on EC2.
- [ ] Auto Scaling Group for Consumer EC2 Instances: Placing the consumer EC2 instances into an Auto Scaling group that spans across two Availability Zones ensures that the message processing tier is both highly available and scalable. If an instance or an AZ fails, the Auto Scaling group can launch new instances in healthy AZs to maintain processing capacity.
- [ ] Amazon RDS for MySQL with Multi-AZ: Migrating the self-managed MySQL database on EC2 to Amazon RDS for MySQL with the Multi-AZ deployment option provides a highly available and durable database. RDS Multi-AZ automatically creates and maintains a synchronous standby replica in a different AZ. In the event of a primary database failure or AZ outage, RDS automatically fails over to the standby replica, minimizing downtime. This is far less operationally complex than self-managing database replication and failover.
- [ ] This combination provides high availability across all critical components (message queue, consumer application, database) using managed services where possible, thus reducing operational burden.

Why are the other answers wrong?

- [ ] Option A is wrong because: While it attempts to introduce redundancy, it relies on self-managing the ActiveMQ servers and self-managing MySQL database replication across Availability Zones. This involves significant operational overhead for setup, maintenance, patching, and ensuring reliable failover compared to using managed AWS services.
- [ ] Option B is wrong because: It correctly suggests using Amazon MQ for the message broker. However, for the consumer application, simply adding "an additional consumer EC2 instance in another Availability Zone" does not provide the same level of automated scaling, health checking, and instance replacement as an Auto Scaling group. More critically, "Replicate the MySQL database to another Availability Zone" implies a self-managed replication for MySQL on EC2, which is less available and more operationally complex than using Amazon RDS Multi-AZ.
- [ ] Option C is wrong because: This option is better as it includes Amazon MQ and Amazon RDS for MySQL with Multi-AZ, which are good choices. However, for the consumer EC2 instances, only "Add an additional consumer EC2 instance in another Availability Zone" is mentioned. An Auto Scaling group (as in option D) provides superior availability and scalability for the consumer tier by automatically managing the desired number of instances and replacing unhealthy ones across AZs.

</details>
  
<details>
  <summary>Question 112</summary>

A company hosts a containerized web application on a fleet of on-premises servers that process incoming requests. The number of requests is growing quickly. The on-premises servers cannot handle the increased number of requests. The company wants to move the application to AWS with minimum code changes and minimum development effort.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Use AWS Fargate on Amazon Elastic Container Service (Amazon ECS) to run the containerized web application with Service Auto Scaling. Use an Application Load Balancer to distribute the incoming requests.
- [ ] B. Use two Amazon EC2 instances to host the containerized web application. Use an Application Load Balancer to distribute the incoming requests.
- [ ] C. Use AWS Lambda with a new code that uses one of the supported languages. Create multiple Lambda functions to support the load. Use Amazon API Gateway as an entry point to the Lambda functions.
- [ ] D. Use a high performance computing (HPC) solution such as AWS ParallelCluster to establish an HPC cluster that can process the incoming requests at the appropriate scale.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Use AWS Fargate on Amazon Elastic Container Service (Amazon ECS) to run the containerized web application with Service Auto Scaling. Use an Application Load Balancer to distribute the incoming requests.

Why this is the correct answer:

- [ ] Leveraging Existing Containerization: The application is already containerized. Amazon Elastic Container Service (ECS) is a fully managed container orchestration service that makes it easy to run, stop, and manage Docker containers on a cluster. This aligns with "minimum code changes."   
- [ ] AWS Fargate for Serverless Compute: AWS Fargate is a serverless compute engine for containers that works with Amazon ECS. When using Fargate, you do not need to provision, configure, or manage the underlying EC2 instances. AWS handles the server management. This directly addresses the requirement for the "LEAST operational overhead" and simplifies scaling.
- [ ] Service Auto Scaling: Amazon ECS services running on Fargate can be configured with Service Auto Scaling to automatically adjust the number of running container tasks based on demand (e.g., CPU or memory utilization, or custom metrics). This handles the "growing quickly" number of requests.
- [ ] Application Load Balancer (ALB): An ALB is well-suited to distribute incoming HTTP/HTTPS traffic across the container tasks managed by ECS on Fargate, providing a scalable and highly available frontend.
- [ ] Minimum Development Effort: Since the application is already containerized, deploying it to ECS on Fargate generally requires minimal development effort beyond defining the task definitions and service configurations.

Why are the other answers wrong?

- [ ] Option B is wrong because: Simply using "two Amazon EC2 instances" to host the application does not offer a scalable solution for a rapidly growing number of requests and involves higher operational overhead for managing the instances, container runtime, and scaling compared to Fargate. It's not designed to handle quick growth efficiently.
- [ ] Option C is wrong because: Migrating a containerized web application to AWS Lambda would likely require significant code changes and refactoring to fit Lambda's event-driven, stateless execution model. This contradicts the "minimum code changes and minimum development effort" requirement. While Lambda is serverless, it's a different paradigm than running existing containers.
- [ ] Option D is wrong because: AWS ParallelCluster is designed for orchestrating and managing High-Performance Computing (HPC) clusters, typically for batch processing, scientific research, or complex simulations. It is not an appropriate or cost-effective solution for hosting a general "containerized web application" that processes incoming user requests.

</details>

<details>
  <summary>Question 113</summary>

A company uses 50 TB of data for reporting. The company wants to move this data from on premises to AWS. A custom application in the company's data center runs a weekly data transformation job. The company plans to pause the application until the data transfer is complete and needs to begin the transfer process as soon as possible. The data center does not have any available network bandwidth for additional workloads. A solutions architect must transfer the data and must configure the transformation job to continue to run in the AWS Cloud.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Use AWS DataSync to move the data. Create a custom transformation job by using AWS Glue.
- [ ] B. Order an AWS Snowcone device to move the data. Deploy the transformation application to the device.
- [ ] C. Order an AWS Snowball Edge Storage Optimized device. Copy the data to the device. Create a custom transformation job by using AWS Glue.
- [ ] D. Order an AWS Snowball Edge Storage Optimized device that includes Amazon EC2 compute. Copy the data to the device. Create a new EC2 instance on AWS to run the transformation application.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Order an AWS Snowball Edge Storage Optimized device. Copy the data to the device. Create a custom transformation job by using AWS Glue.

Why this is the correct answer:

This solution addresses the key constraints: large data volume (50 TB), no available network bandwidth for transfer, and the need to run transformations in AWS with minimal operational overhead.

- [ ] AWS Snowball Edge Storage Optimized for Data Transfer: Given that the "data center does not have any available network bandwidth for additional workloads," an online transfer method is not feasible. For 50 TB of data, an AWS Snowball Edge Storage Optimized device is a suitable offline data transfer solution. The company can order the device, copy the 50 TB of data to it on-premises, and then ship it to AWS for ingestion into Amazon S3. This allows the transfer to "begin as soon as possible" without impacting existing network bandwidth.
- [ ] AWS Glue for Data Transformation in the Cloud: Once the data is in Amazon S3, AWS Glue can be used to perform the "weekly data transformation job." AWS Glue is a fully managed ETL (extract, transform, and load) service that is serverless. This means there are no servers to manage, and it aligns with the requirement for the "LEAST operational overhead" for running the transformation job in the AWS Cloud.

Why are the other answers wrong?

- [ ] Option A is wrong because: AWS DataSync is an online data transfer service. The problem statement explicitly says, "The data center does not have any available network bandwidth for additional workloads," making DataSync unsuitable for the initial large data transfer.
- [ ] Option B is wrong because: An AWS Snowcone device has a limited storage capacity (typically up to 8 TB for HDD or 14 TB for SSD). For 50 TB of data, multiple Snowcone devices would be required, which is less efficient than a single Snowball Edge device. While Snowcone supports edge compute, deploying and running the transformation application directly on the device is not the most straightforward way to have the job "continue to run in the AWS Cloud" long-term with least operational overhead. AWS Glue is better for cloud-based ETL.
- [ ] Option D is wrong because: While using an AWS Snowball Edge Storage Optimized device for the transfer is correct, setting up and managing a new EC2 instance on AWS to run the existing transformation application involves more operational overhead (patching, OS management, scaling) compared to using a serverless ETL service like AWS Glue. The requirement is for the "LEAST operational overhead" for the transformation job in the cloud.

</details>

<details>
  <summary>Question 114</summary>

A company has created an image analysis application in which users can upload photos and add photo frames to their images. The users upload images and metadata to indicate which photo frames they want to add to their images. The application uses a single Amazon EC2 instance and Amazon DynamoDB to store the metadata. The application is becoming more popular, and the number of users is increasing. The company expects the number of concurrent users to vary significantly depending on the time of day and day of week. The company must ensure that the application can scale to meet the needs of the growing user base.

Which solution meets these requirements?

- [ ] A. Use AWS Lambda to process the photos. Store the photos and metadata in DynamoDB.
- [ ] B. Use Amazon Kinesis Data Firehose to process the photos and to store the photos and metadata.
- [ ] C. Use AWS Lambda to process the photos. Store the photos in Amazon S3. Retain DynamoDB to store the metadata.
- [ ] D. Increase the number of EC2 instances to three. Use Provisioned IOPS SSD (io2) Amazon Elastic Block Store (Amazon EBS) volumes to store the photos and metadata.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Use AWS Lambda to process the photos. Store the photos in Amazon S3. Retain DynamoDB to store the metadata.

Why this is the correct answer:

This solution leverages serverless and managed services for scalability and efficiency:

- [ ] AWS Lambda for Photo Processing: AWS Lambda is a serverless compute service that can execute code in response to events (e.g., an image upload). It automatically scales based on the number of incoming requests, making it ideal for handling the "varying significantly" number of concurrent users and the growing user base. Using Lambda to process photos (add frames) offloads this compute-intensive task from a fixed EC2 instance.
- [ ] Amazon S3 for Storing Photos: Amazon S3 is the most appropriate service for storing image files. It is highly scalable, durable, and cost-effective for storing large amounts of user-generated content like photos.
- [ ] Retain DynamoDB for Metadata: The question states the application already uses Amazon DynamoDB to store metadata, and DynamoDB is excellent for this purpose due to its scalability, low latency, and flexible schema. Retaining DynamoDB for metadata is a sound choice.
- [ ] Scalability: This architecture (Lambda for processing, S3 for photo storage, DynamoDB for metadata) is inherently scalable. Each component can scale independently to meet demand.

Why are the other answers wrong?

- [ ] Option A is wrong because: While using AWS Lambda for processing and DynamoDB for metadata is good, storing entire photo files (which can be several megabytes or more) directly in Amazon DynamoDB is generally not recommended or cost-effective. DynamoDB has an item size limit (400 KB), and storing large binary objects is better suited for Amazon S3.
- [ ] Option B is wrong because: Amazon Kinesis Data Firehose is a service for loading streaming data into data lakes, data stores, and analytics services. It is not designed for interactive image processing tasks like adding photo frames based on user requests, nor is it a primary storage solution for individual image files and their metadata in the context of a user-facing application.
- [ ] Option D is wrong because: Simply increasing the number of EC2 instances to a fixed number (three) is not a solution that "can scale to meet the needs of the growing user base" dynamically or handle significantly varying demand cost-effectively. It can lead to either under-provisioning (poor performance during peaks) or over-provisioning (wasted cost during lulls). Storing photos and metadata on EBS volumes attached to EC2 instances is also less scalable and more operationally intensive than using S3 and DynamoDB.

</details>

<details>
  <summary>Question 115</summary>

A medical records company is hosting an application on Amazon EC2 instances. The application processes customer data files that are stored on Amazon S3. The EC2 instances are hosted in public subnets. The EC2 instances access Amazon S3 over the internet, but they do not require any other network access. A new requirement mandates that the network traffic for file transfers take a private route and not be sent over the internet.

Which change to the network architecture should a solutions architect recommend to meet this requirement?

- [ ] A. Create a NAT gateway. Configure the route table for the public subnets to send traffic to Amazon S3 through the NAT gateway.
- [ ] B. Configure the security group for the EC2 instances to restrict outbound traffic so that only traffic to the S3 prefix list is permitted.
- [ ] C. Move the EC2 instances to private subnets. Create a VPC endpoint for Amazon S3, and link the endpoint to the route table for the private subnets.
- [ ] D. Remove the internet gateway from the VPC. Set up an AWS Direct Connect connection, and route traffic to Amazon S3 over the Direct Connect connection.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Move the EC2 instances to private subnets. Create a VPC endpoint for Amazon S3, and link the endpoint to the route table for the private subnets.

Why this is the correct answer:

This solution addresses both security best practices and the specific requirement for private S3 access:

- [ ] Move EC2 Instances to Private Subnets: Hosting application instances, especially those processing sensitive data like medical records, in public subnets is generally not recommended if they do not need direct inbound internet access. Moving them to private subnets enhances their security posture by removing direct internet exposure.
- [ ] VPC Endpoint for Amazon S3 (Gateway Endpoint): A VPC gateway endpoint for S3 allows instances within your VPC to communicate with Amazon S3 without traffic traversing the public internet. Traffic routes privately within the AWS network. This directly meets the requirement that "network traffic for file transfers take a private route and not be sent over the internet."
- [ ] Link Endpoint to Route Table: The gateway endpoint is associated with the route tables of the private subnets where the EC2 instances reside. This ensures that traffic destined for S3 from these instances is routed through the private endpoint.
- [ ] No Other Network Access Needed: The problem states the EC2 instances "do not require any other network access" (implying general internet access beyond S3 is not needed). This makes the private subnet with a VPC endpoint for S3 an ideal and secure configuration.

Why are the other answers wrong?

- [ ] Option A is wrong because: A NAT gateway is used to enable instances in private subnets to initiate outbound connections to the internet. If the instances are already in public subnets, they would typically use an Internet Gateway for internet access. Routing traffic from public subnets through a NAT gateway to access S3 is an incorrect use of NAT gateways and doesn't ensure a private route in the intended way; a VPC endpoint is the proper solution.
- [ ] Option B is wrong because: Security groups act as firewalls at the instance level. Restricting outbound traffic to the S3 prefix list is a good security measure to limit what the EC2 instances can connect to. However, it does not change the network path of the traffic. If the instances are in public subnets and an Internet Gateway is present, traffic to S3 public endpoints might still traverse the internet or AWS public network space. This does not guarantee a "private route."
- [ ] Option D is wrong because:
Removing the internet gateway from the VPC would cut off all direct internet access for resources that might legitimately need it (though the question states these EC2s don't need other internet access).
AWS Direct Connect is a service for establishing a dedicated private network connection from an on-premises environment to AWS. It is not the primary mechanism for ensuring EC2 instances within a VPC access S3 privately. A VPC endpoint is the direct solution for intra-AWS private connectivity to S3 from a VPC.

</details>

<details>
  <summary>Question 116</summary>

A company uses a popular content management system (CMS) for its corporate website. However, the required patching and maintenance are burdensome. The company is redesigning its website and wants a new solution. The website will be updated four times a year and does not need to have any dynamic content available. The solution must provide high scalability and enhanced security.

Which combination of changes will meet these requirements with the LEAST operational overhead? (Choose two.)

- [ ] A. Configure Amazon CloudFront in front of the website to use HTTPS functionality.
- [ ] B. Deploy an AWS WAF web ACL in front of the website to provide HTTPS functionality.
- [ ] C. Create and deploy an AWS Lambda function to manage and serve the website content.
- [ ] D. Create the new website and an Amazon S3 bucket. Deploy the website on the S3 bucket with static website hosting enabled.
- [ ] E. Create the new website. Deploy the website by using an Auto Scaling group of Amazon EC2 instances behind an Application Load Balancer.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Configure Amazon CloudFront in front of the website to use HTTPS functionality.
- [ ] D. Create the new website and an Amazon S3 bucket. Deploy the website on the S3 bucket with static website hosting enabled.

Why these are the correct answers:

This solution leverages serverless and managed services for a static website, which is ideal for low operational overhead, high scalability, and enhanced security.

D. Create the new website and an Amazon S3 bucket. Deploy the website on the S3 bucket with static website hosting enabled.
- [ ] Static Content Hosting on S3: The requirement states the website "does not need to have any dynamic content available" and is updated infrequently (four times a year). Amazon S3 is an excellent choice for hosting static websites (HTML, CSS, JavaScript, images). It is highly durable, scalable, and very cost-effective.
- [ ] Least Operational Overhead: S3 static website hosting is serverless, meaning there are no servers to manage, patch, or scale, thus minimizing operational overhead.

A. Configure Amazon CloudFront in front of the website to use HTTPS functionality.
- [ ] HTTPS and Enhanced Security: Amazon CloudFront is a global content delivery network (CDN). You can configure it with an SSL/TLS certificate (e.g., a free one from AWS Certificate Manager) to serve your website content over HTTPS, which enhances security. CloudFront also helps protect against common DDoS attacks by absorbing traffic at its distributed edge locations.
- [ ] High Scalability and Performance: CloudFront caches your static content from the S3 origin at edge locations around the world, closer to your users. This provides high scalability to handle traffic spikes and significantly improves website loading performance by reducing latency.
- [ ] Integration: CloudFront integrates seamlessly with S3 as an origin.

Why are the other answers wrong?

- [ ] Option B is wrong because: AWS WAF (Web Application Firewall) is a service that helps protect web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources. While WAF is important for security and often used with CloudFront or an Application Load Balancer, it does not directly "provide HTTPS functionality." HTTPS termination is handled by services like CloudFront or ALBs using SSL/TLS certificates.   
- [ ] Option C is wrong because: Using AWS Lambda to manage and serve the content of a purely static website is an overly complex and less efficient solution compared to S3 static website hosting. While Lambda can be used to serve dynamic content or as part of a serverless backend, for hosting static files, S3 is more straightforward, cost-effective, and operationally simpler.
- [ ] Option E is wrong because: Deploying a static website using an Auto Scaling group of Amazon EC2 instances behind an Application Load Balancer is a solution designed for dynamic applications or those requiring server-side compute. For a website with no dynamic content, this approach incurs unnecessary costs (for EC2 instances, ALB) and significantly higher operational overhead (managing instances, patching, OS updates) compared to the S3 and CloudFront serverless model.

</details>

<details>
  <summary>Question 117</summary>

A company stores its application logs in an Amazon CloudWatch Logs log group. A new policy requires the company to store all application logs in Amazon OpenSearch Service (Amazon Elasticsearch Service) in near-real time.

Which solution will meet this requirement with the LEAST operational overhead?

- [ ] A. Configure a CloudWatch Logs subscription to stream the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).
- [ ] B. Create an AWS Lambda function. Use the log group to invoke the function to write the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).
- [ ] C. Create an Amazon Kinesis Data Firehose delivery stream. Configure the log group as the delivery stream's sources. Configure Amazon OpenSearch Service (Amazon Elasticsearch Service) as the delivery stream's destination.
- [ ] D. Install and configure Amazon Kinesis Agent on each application server to deliver the logs to Amazon Kinesis Data Streams. Configure Kinesis Data Streams to deliver the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Configure a CloudWatch Logs subscription to stream the logs to Amazon OpenSearch Service (Amazon Elasticsearch Service).

Why this is the correct answer:

- [ ] CloudWatch Logs Subscriptions: Amazon CloudWatch Logs allows you to create subscription filters. These filters can match log events from a log group and stream them in near real-time to various AWS destinations, including Amazon Kinesis Data Streams, AWS Lambda, and directly to an Amazon OpenSearch Service (formerly Amazon Elasticsearch Service) domain.
- [ ] Direct Streaming to OpenSearch Service: Configuring a CloudWatch Logs subscription to stream directly to an Amazon OpenSearch Service domain is a built-in, fully managed capability. This is the most straightforward and direct method for this specific requirement.
- [ ] Near-Real Time: Subscription filters process and deliver data in near-real time.
- [ ] Least Operational Overhead: This approach requires minimal setup and no ongoing management of intermediary services like custom Lambda functions or Kinesis Data Firehose delivery streams (if their specific features aren't needed). AWS manages the data streaming pipeline from CloudWatch Logs to OpenSearch Service.

Why are the other answers wrong?

- [ ] Option B is wrong because: While you can configure a CloudWatch Logs subscription to trigger an AWS Lambda function, which then writes the logs to OpenSearch Service, this introduces custom code that needs to be developed, deployed, monitored, and maintained. A direct subscription (as in Option A) avoids this custom Lambda logic, resulting in less operational overhead.
- [ ] Option C is wrong because: This proposes a path: CloudWatch Logs -> Amazon Kinesis Data Firehose -> Amazon OpenSearch Service. While this is a valid and robust way to get data into OpenSearch Service (and Firehose offers features like data transformation, batching, compression, and retries), it involves an additional service (Kinesis Data Firehose) to configure and manage. If the only requirement is to stream logs from CloudWatch Logs to OpenSearch Service without needing the specific transformation or buffering capabilities of Firehose, a direct subscription (Option A) is simpler and has less operational overhead.
- [ ] Option D is wrong because: This solution suggests changing the log collection mechanism by installing and configuring the Amazon Kinesis Agent on each application server to send logs to Amazon Kinesis Data Streams, which then deliver to OpenSearch Service. The problem states the logs are already being collected in CloudWatch Logs. This option introduces unnecessary complexity and operational overhead by requiring agent installation and management on application servers and doesn't leverage the existing CloudWatch Logs setup.

</details>

<details>
  <summary>Question 118</summary>

A company is building a web-based application running on Amazon EC2 instances in multiple Availability Zones. The web application will provide access to a repository of text documents totaling about 900 TB in size. The company anticipates that the web application will experience periods of high demand. A solutions architect must ensure that the storage component for the text documents can scale to meet the demand of the application at all times. The company is concerned about the overall cost of the solution.

Which storage solution meets these requirements MOST cost-effectively?

- [ ] A. Amazon Elastic Block Store (Amazon EBS)
- [ ] B. Amazon Elastic File System (Amazon EFS)
- [ ] C. Amazon OpenSearch Service (Amazon Elasticsearch Service)
- [ ] D. Amazon S3

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Amazon S3

Why this is the correct answer:

- [ ] Amazon S3 for Scalability and Cost-Effectiveness: Amazon S3 (Simple Storage Service) is designed for storing and retrieving massive amounts of data (like 900 TB) with high durability and availability. It scales automatically to handle storage needs and request loads, which is crucial for an application experiencing periods of high demand. Critically, S3 is generally the MOST cost-effective AWS storage option for large object storage like text documents, especially when considering the scale of 900 TB.
- [ ] Accessibility for Web Applications: The text documents stored in S3 can be easily accessed by the web application running on EC2 instances, either directly via the S3 API or by serving them through services like Amazon CloudFront for optimized delivery.
- [ ] Meeting Demand: S3's inherent scalability ensures it can handle the fluctuating demand of the application.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon Elastic Block Store (Amazon EBS) provides block-level storage volumes for use with EC2 instances. While EBS offers various performance tiers, storing and managing 900 TB of data directly on EBS volumes across multiple EC2 instances would be significantly more expensive and operationally complex than using S3. EBS volumes are also zonal, so sharing this amount of data across a fleet of instances in multiple Availability Zones would require additional mechanisms if a shared repository is needed, adding complexity and cost. S3 is better suited as a central repository for this volume of data.
- [ ] Option B is wrong because: Amazon Elastic File System (Amazon EFS) provides scalable, shared file storage that can be accessed by multiple EC2 instances. While EFS can scale to petabytes, its cost per GB is typically higher than Amazon S3. For storing a large repository of 900 TB of text documents where cost-effectiveness is a major concern, S3 is usually the more economical choice. EFS is often preferred when applications require a POSIX-compliant file system interface.
- [ ] Option C is wrong because: Amazon OpenSearch Service (Amazon Elasticsearch Service) is a managed service for search, analytics, and visualization of data. While it could be used to index the content of the text documents to make them searchable, OpenSearch Service itself is not designed or cost-effective as a primary storage solution for 900 TB of raw text documents. It would typically be used in conjunction with a primary storage service like S3, where the documents are stored, and OpenSearch Service stores the search index.

</details>

<details>
  <summary>Question 119</summary>

A global company is using Amazon API Gateway to design REST APIs for its loyalty club users in the us-east-1 Region and the ap-southeast-2 Region. A solutions architect must design a solution to protect these API Gateway managed REST APIs across multiple accounts from SQL injection and cross-site scripting attacks.

Which solution will meet these requirements with the LEAST amount of administrative effort?

- [ ] A. Set up AWS WAF in both Regions. Associate Regional web ACLs with an API stage.
- [ ] B. Set up AWS Firewall Manager in both Regions. Centrally configure AWS WAF rules.
- [ ] C. Set up AWS Shield in bath Regions. Associate Regional web ACLs with an API stage.
- [ ] D. Set up AWS Shield in one of the Regions. Associate Regional web ACLs with an API stage.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Set up AWS Firewall Manager in both Regions. Centrally configure AWS WAF rules.

Why this is the correct answer:

- [ ] AWS WAF for Application Layer Protection: AWS WAF (Web Application Firewall) is the appropriate service to protect web applications and APIs (like those managed by Amazon API Gateway) from common web exploits such as SQL injection and cross-site scripting (XSS) attacks.
- [ ] AWS Firewall Manager for Centralized Management: The company has APIs in multiple regions (us-east-1, ap-southeast-2) and potentially "across multiple accounts." AWS Firewall Manager is a security management service that allows you to centrally configure and manage firewall rules (including AWS WAF rules) across your AWS accounts and applications within an AWS Organization. This means you can define a common security policy (e.g., WAF rules for SQL injection and XSS protection) and automatically apply it to all your API Gateway instances in different regions and accounts.
- [ ] Least Amount of Administrative Effort: Using Firewall Manager to centrally deploy and manage AWS WAF rules significantly reduces the administrative effort compared to manually configuring and maintaining WAF web ACLs in each region and for each account individually. It ensures consistency and simplifies policy updates.

Why are the other answers wrong?

- [ ] Option A is wrong because: While setting up AWS WAF in both regions and associating web ACLs with the API stages is necessary for protection, doing this manually in each region (and potentially each account if "across multiple accounts" implies different AWS accounts) does not represent the "LEAST amount of administrative effort." AWS Firewall Manager (Option B) is designed for centralized management, which reduces this effort.
- [ ] Option C is wrong because: AWS Shield is a managed Distributed Denial of Service (DDoS) protection service. While AWS Shield Advanced works in conjunction with AWS WAF for comprehensive protection, Shield itself primarily focuses on mitigating DDoS attacks, not specifically SQL injection or cross-site scripting. AWS WAF is the service tailored for protecting against these application-layer attacks.
- [ ] Option D is wrong because: Similar to option C, AWS Shield is for DDoS protection. Furthermore, protections like WAF rules need to be applied in each region where the API Gateway endpoints exist, not just one region, to be effective for all APIs.


</details>

<details>
  <summary>Question 120</summary>
 
A company has implemented a self-managed DNS solution on three Amazon EC2 instances behind a Network Load Balancer (NLB) in the us-west-2 Region. Most of the company's users are located in the United States and Europe. The company wants to improve the performance and availability of the solution. The company launches and configures three EC2 instances in the eu-west-1 Region and adds the EC2 instances as targets for a new NLB.

Which solution can the company use to route traffic to all the EC2 instances?

- [ ] A. Create an Amazon Route 53 geolocation routing policy to route requests to one of the two NLBs. Create an Amazon CloudFront distribution. Use the Route 53 record as the distribution's origin.
- [ ] B. Create a standard accelerator in AWS Global Accelerator. Create endpoint groups in us-west-2 and eu-west-1. Add the two NLBs as endpoints for the endpoint groups.
- [ ] C. Attach Elastic IP addresses to the six EC2 instances. Create an Amazon Route 53 geolocation routing policy to route requests to one of the six EC2 instances. Create an Amazon CloudFront distribution. Use the Route 53 record as the distribution's origin.
- [ ] D. Replace the two NLBs with two Application Load Balancers (ALBs). Create an Amazon Route 53 latency routing policy to route requests to one of the two ALBs. Create an Amazon CloudFront distribution. Use the Route 53 record as the distribution's origin.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create a standard accelerator in AWS Global Accelerator. Create endpoint groups in us-west-2 and eu-west-1. Add the two NLBs as endpoints for the endpoint groups.

Why this is the correct answer:

- [ ] AWS Global Accelerator for Global Traffic Management: AWS Global Accelerator is a service that improves the availability and performance of your applications with local or global users. It provides static IP addresses that act as a fixed entry point to your application endpoints in one or more AWS Regions. Global Accelerator directs user traffic to the optimal endpoint based on factors like user location, endpoint health, and configured weights.   
- [ ] Suitable for Non-HTTP (DNS) Traffic: DNS primarily uses UDP (and sometimes TCP) on port 53. Global Accelerator supports both TCP and UDP traffic, making it suitable for routing DNS requests.
- [ ] Improved Performance and Availability: By creating endpoint groups in us-west-2 and eu-west-1 and adding the respective NLBs as endpoints, Global Accelerator will route users to the nearest healthy regional deployment of the DNS service. This reduces latency and improves availability through health checks and automatic failover.
- [ ] Uses Existing NLBs: This solution leverages the existing Network Load Balancers, which are appropriate for handling DNS traffic to the EC2 instances.

Why are the other answers wrong?

- [ ] Option A is wrong because: Amazon CloudFront is a content delivery network (CDN) designed primarily for caching and delivering web content (HTTP/HTTPS). It is not suitable for distributing or routing DNS query traffic. While Amazon Route 53 geolocation routing can direct users to the NLB in the geographically closest region, Global Accelerator offers additional benefits like routing traffic over the optimized AWS global network and providing static anycast IP addresses.
- [ ] Option C is wrong because: Attaching Elastic IP addresses to individual EC2 instances and using Route 53 to route directly to these instances bypasses the Network Load Balancers. This would eliminate the load balancing and health checking benefits provided by the NLBs, reducing availability and scalability. Also, CloudFront is not suitable for DNS traffic.
- [ ] Option D is wrong because:
Application Load Balancers (ALBs) operate at Layer 7 and are designed for HTTP/HTTPS traffic. They are not suitable for handling DNS traffic, which primarily uses UDP/TCP on port 53. Network Load Balancers are the correct choice for this protocol.

Again, CloudFront is not appropriate for DNS traffic distribution.

</details>

</details>

<details>
  <summary>==Questions 121-130==</summary>

<details>
  <summary>Question 121</summary>

A company is running an online transaction processing (OLTP) workload on AWS. This workload uses an unencrypted Amazon RDS DB instance in a Multi-AZ deployment. Daily database snapshots are taken from this instance.

What should a solutions architect do to ensure the database and snapshots are always encrypted moving forward?

- [ ] A. Encrypt a copy of the latest DB snapshot. Replace existing DB instance by restoring the encrypted snapshot.
- [ ] B. Create a new encrypted Amazon Elastic Block Store (Amazon EBS) volume and copy the snapshots to it. Enable encryption on the DB instance.
- [ ] C. Copy the snapshots and enable encryption using AWS Key Management Service (AWS KMS) Restore encrypted snapshot to an existing DB instance.
- [ ] D. Copy the snapshots to an Amazon S3 bucket that is encrypted using server-side encryption with AWS Key Management Service (AWS KMS) managed keys (SSE-KMS).

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Encrypt a copy of the latest DB snapshot. Replace existing DB instance by restoring the encrypted snapshot.

Why this is the correct answer:

- [ ] Process for Encrypting an Unencrypted RDS Instance: You cannot directly encrypt an existing Amazon RDS DB instance that was originally created unencrypted. The standard procedure to achieve encryption for such an instance involves using snapshots:
- [ ] Take a final snapshot of the existing unencrypted DB instance.
- [ ] Copy this snapshot. During the copy process, you can specify an AWS Key Management Service (AWS KMS) key to encrypt the copied snapshot.
- [ ] Restore a new DB instance from this newly created encrypted snapshot. The new DB instance will have its underlying storage encrypted with the specified KMS key.
- [ ] Once the new encrypted DB instance is running and tested, update your application to connect to the new instance, and then you can delete the old unencrypted DB instance.
- [ ] Encryption of Future Snapshots: When an RDS DB instance is encrypted (because it was restored from an encrypted snapshot), all subsequent automated and manual snapshots taken from this instance will also be encrypted using the same KMS key by default. This ensures that snapshots are "always encrypted moving forward."
- [ ] This approach ensures both the live database and all its future snapshots are encrypted.

Why are the other answers wrong?

- [ ] Option B is wrong because: Amazon RDS manages its own underlying EBS storage. You do not directly create EBS volumes and copy RDS snapshots to them to achieve RDS encryption. Furthermore, you cannot simply "Enable encryption on the DB instance" if it was initially created as unencrypted. The snapshot copy-and-restore method is required.
- [ ] Option C is wrong because: While copying snapshots and enabling encryption during the copy is correct, you cannot "Restore encrypted snapshot to an existing DB instance." An RDS snapshot restore operation always creates a new DB instance. You then migrate your application to use this new, encrypted instance.
- [ ] Option D is wrong because: While RDS snapshots are stored in Amazon S3 managed by AWS, copying these snapshots to your own S3 bucket and encrypting that bucket does not encrypt the actual RDS DB instance or ensure that its future automated RDS snapshots are encrypted. The encryption needs to be applied at the RDS level during the instance creation (from an encrypted snapshot).

</details>  

<details>
  <summary>Question 122</summary>

- [ ] A. Use multi-factor authentication (MFA) to protect the encryption keys.ations.

What should a solutions architect do to reduce the operational burden?

- [ ] A. Use multi-factor authentication (MFA) to protect the encryption keys.
- [ ] B. Use AWS Key Management Service (AWS KMS) to protect the encryption keys.
- [ ] C. Use AWS Certificate Manager (ACM) to create, store, and assign the encryption keys.
- [ ] D. Use an IAM policy to limit the scope of users who have access permissions to protect the encryption keys.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use AWS Key Management Service (AWS KMS) to protect the encryption keys.

Why this is the correct answer:

- [ ] AWS Key Management Service (AWS KMS): AWS KMS is a managed service specifically designed for creating, managing, and controlling cryptographic keys. It provides a highly available and durable infrastructure for key storage and performs encryption and decryption operations.
- [ ] Reduces Operational Burden: Because KMS is a fully managed service, AWS handles the operational aspects of the key management infrastructure, such as hardware provisioning, patching, maintenance, availability, and scalability. This significantly "reduces the operational burden" on the company compared to building and maintaining an on-premises or custom key management solution. Developers can easily integrate with KMS using the AWS SDK to encrypt and decrypt data in their applications.
- [ ] Scalable Infrastructure: KMS is designed to scale to meet the needs of various applications and developers requiring encryption capabilities.

Why are the other answers wrong?

- [ ] Option A is wrong because: Multi-factor authentication (MFA) is an important security measure for protecting access to AWS accounts and sensitive operations (including the management of KMS keys via IAM users or roles). However, MFA itself is an authentication mechanism, not a "key management infrastructure" or a service that protects the encryption keys at rest or manages their lifecycle.
- [ ] Option C is wrong because: AWS Certificate Manager (ACM) is a service for provisioning, managing, and deploying public and private SSL/TLS certificates. Certificates are used for authenticating identities and securing network communications (e.g., HTTPS). ACM is not primarily designed for managing symmetric or asymmetric encryption keys that developers would use for general data encryption within applications. AWS KMS is the appropriate service for data encryption keys.
- [ ] Option D is wrong because: Using IAM (Identity and Access Management) policies is crucial for controlling who has access to encryption keys (e.g., keys managed by AWS KMS) and what actions they can perform. This is a vital part of securing a key management infrastructure. However, IAM policies are an access control mechanism, not the key management infrastructure itself. You still need a service like KMS to create, store, and manage the actual encryption keys.

</details>

<details>
  <summary>Question 123</summary>

A company has a dynamic web application hosted on two Amazon EC2 instances. The company has its own SSL certificate, which is on each instance to perform SSL termination. There has been an increase in traffic recently, and the operations team determined that SSL encryption and decryption is causing the compute capacity of the web servers to reach their maximum limit.

What should a solutions architect do to increase the application's performance?

- [ ] A. Create a new SSL certificate using AWS Certificate Manager (ACM). Install the ACM certificate on each instance.
- [ ] B. Create an Amazon S3 bucket Migrate the SSL certificate to the S3 bucket. Configure the EC2 instances to reference the bucket for SSL termination.
- [ ] C. Create another EC2 instance as a proxy server. Migrate the SSL certificate to the new instance and configure it to direct connections to the existing EC2 instances.
- [ ] D. Import the SSL certificate into AWS Certificate Manager (ACM). Create an Application Load Balancer with an HTTPS listener that uses the SSL certificate from ACM.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Import the SSL certificate into AWS Certificate Manager (ACM). Create an Application Load Balancer with an HTTPS listener that uses the SSL certificate from ACM.

Why this is the correct answer:

- [ ] Offloading SSL Termination to Application Load Balancer (ALB): The core issue is that SSL encryption/decryption is consuming excessive CPU resources on the EC2 web servers, impacting performance. An - [ ] Application Load Balancer can terminate SSL/TLS connections. This means the ALB handles the computationally intensive SSL handshake and encryption/decryption processes, freeing up the EC2 instances' CPU cycles to focus on application logic. This directly addresses the performance bottleneck.
- [ ] Using Existing SSL Certificate with ACM: The company has its "own SSL certificate." AWS Certificate Manager (ACM) allows you to import third-party SSL/TLS certificates. Once imported into ACM, this certificate can be easily deployed on the ALB.
- [ ] Improved Performance and Scalability: By offloading SSL termination to the ALB, the EC2 instances have more capacity to serve application requests, leading to increased performance. The ALB also provides load balancing across the EC2 instances, which helps with scalability and availability.

Why are the other answers wrong?

- [ ] Option A is wrong because: Simply creating a new SSL certificate with ACM and installing it on the EC2 instances does not solve the problem. SSL termination would still occur on the EC2 instances, and they would continue to experience high CPU load due to SSL processing. The key is to offload this processing from the instances.
- [ ] Option B is wrong because: Storing an SSL certificate in an Amazon S3 bucket and configuring EC2 instances to reference it for SSL termination is not a standard, secure, or efficient way to handle SSL. This approach doesn't address the SSL processing load on the EC2 instances and introduces complexities in managing certificate access.
- [ ] Option C is wrong because: While creating a dedicated EC2 instance to act as a proxy server (e.g., running Nginx or HAProxy) for SSL termination could offload the SSL processing from the web servers, this solution involves managing an additional EC2 instance (patching, scaling, high availability). Using a managed service like an Application Load Balancer (as in Option D) achieves the same SSL offloading benefits with significantly less operational overhead.

</details>

<details>
  <summary>Question 124</summary>
 
A company has a highly dynamic batch processing job that uses many Amazon EC2 instances to complete it. The job is stateless in nature, can be started and stopped at any given time with no negative impact, and typically takes upwards of 60 minutes total to complete. The company has asked a solutions architect to design a scalable and cost-effective solution that meets the requirements of the job.

What should the solutions architect recommend?

- [ ] A. Implement EC2 Spot Instances.
- [ ] B. Purchase EC2 Reserved Instances.
- [ ] C. Implement EC2 On-Demand Instances.
- [ ] D. Implement the processing on AWS Lambda.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Implement EC2 Spot Instances.

Why this is the correct answer:

- [ ] EC2 Spot Instances for Cost-Effectiveness: Amazon EC2 Spot Instances allow you to bid on unused EC2 capacity and can provide savings of up to 90% compared to On-Demand prices. For batch processing jobs that are fault-tolerant and flexible in their execution time, Spot Instances are an extremely cost-effective option.
- [ ] Suitable for Stateless and Interruptible Workloads: The problem states the job is "stateless in nature, can be started and stopped at any given time with no negative impact." This makes it an ideal workload for Spot Instances because if a Spot Instance is interrupted (reclaimed by AWS with a two-minute warning), the stateless nature of the job means it can be resumed or restarted on another instance without significant data loss or corruption.
- [ ] Scalable: Spot Instances can be requested in large numbers to provide the necessary compute capacity for the "many Amazon EC2 instances" needed to complete the job.
- [ ] Handles Job Duration: The job duration of "upwards of 60 minutes" is generally well within the typical runtimes achievable with Spot Instances, especially if the application is designed to handle potential interruptions gracefully (e.g., by checkpointing or processing data in smaller, independent chunks).

Why are the other answers wrong?

- [ ] Option B is wrong because: EC2 Reserved Instances (RIs) provide a discount in exchange for a 1-year or 3-year commitment to use EC2. RIs are best suited for continuous, predictable workloads. For a "highly dynamic batch processing job" that might not run 24/7 or whose instance requirements might change, RIs could lead to underutilized committed capacity and may not be as cost-effective as Spot Instances, especially given the interruptible nature of the job.
- [ ] Option C is wrong because: EC2 On-Demand Instances provide compute capacity with no long-term commitment, billed by the second (for Linux). While they offer flexibility, On-Demand pricing is significantly higher than Spot Instance pricing. Since the workload is stateless and interruptible, the cost savings offered by Spot Instances make them a more compelling choice for cost-effectiveness.
- [ ] Option D is wrong because: AWS Lambda functions currently have a maximum execution duration of 15 minutes (900 seconds). The batch processing job "typically takes upwards of 60 minutes total to complete." While a large job could be broken down into many smaller tasks that run on Lambda and are orchestrated (e.g., by AWS Step Functions), if individual components of the batch job themselves are long-running, Lambda might not be the most direct or suitable compute option. For a job already designed to use "many Amazon EC2 instances," leveraging EC2 Spot Instances is a more direct fit for the described processing duration.

</details>

<details>
  <summary>Question 125</summary>

A company runs its two-tier ecommerce website on AWS. The web tier consists of a load balancer that sends traffic to Amazon EC2 instances. The database tier uses an Amazon RDS DB instance. The EC2 instances and the RDS DB instance should not be exposed to the public internet. The EC2 instances require internet access to complete payment processing of orders through a third-party web service. The application must be highly available.

Which combination of configuration options will meet these requirements? (Choose two.)

- [ ] A. Use an Auto Scaling group to launch the EC2 instances in private subnets. Deploy an RDS Multi-AZ DB instance in private subnets.
- [ ] B. Configure a VPC with two private subnets and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the private subnets.
- [ ] C. Use an Auto Scaling group to launch the EC2 instances in public subnets across two Availability Zones. Deploy an RDS Multi-AZ DB instance in private subnets.
- [ ] D. Configure a VPC with one public subnet, one private subnet, and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the public subnet.
- [ ] E. Configure a VPC with two public subnets, two private subnets, and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the public subnets.

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Use an Auto Scaling group to launch the EC2 instances in private subnets. Deploy an RDS Multi-AZ DB instance in private subnets.
- [ ] E. Configure a VPC with two public subnets, two private subnets, and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the public subnets.

Why these are the correct answers:

This question asks for a combination of configurations to build a highly available and secure two-tier web application.

A. Use an Auto Scaling group to launch the EC2 instances in private subnets. Deploy an RDS Multi-AZ DB instance in private subnets.
- [ ] Security for EC2 and RDS: Placing both the EC2 instances (application/web tier) and the RDS DB instance in private subnets ensures they are "not exposed to the public internet," meeting a key security requirement.
- [ ] High Availability for EC2: Using an Auto Scaling group to launch EC2 instances allows the application tier to scale based on demand and maintain availability by replacing unhealthy instances. For high availability, this Auto Scaling group should be configured to span multiple Availability Zones.
- [ ] High Availability for RDS: Deploying the RDS DB instance as a Multi-AZ deployment ensures database high availability through a synchronous standby replica in a different AZ with automatic failover.

E. Configure a VPC with two public subnets, two private subnets, and two NAT gateways across two Availability Zones. Deploy an Application Load Balancer in the public subnets.
- [ ] VPC Structure for High Availability: A VPC configured with public and private subnets across at least two Availability Zones is fundamental for a highly available architecture. "Two public subnets, two private subnets" typically implies one of each per AZ for a two-AZ setup.
- [ ] Application Load Balancer (ALB) in Public Subnets: The load balancer (which would be an ALB for an HTTP/S website) needs to be accessible from the internet to receive user traffic. Placing it in the public subnets achieves this. An ALB itself is highly available as it distributes traffic across multiple AZs.
- [ ] NAT Gateways for Outbound Internet Access: The EC2 instances in private subnets "require internet access to complete payment processing." NAT gateways, deployed in public subnets (one in each AZ for high availability), allow instances in private subnets to initiate outbound connections to the internet while preventing inbound connections from the internet. Route tables for the private subnets would be configured to route internet-bound traffic through these NAT gateways.

Combining these two options (A and E) creates a complete, secure, and highly available architecture:

- [ ] A well-structured VPC with public and private subnets across multiple AZs.
- [ ] A public-facing ALB in the public subnets.
- [ ] EC2 instances for the application tier in private subnets, managed by an Auto Scaling group spanning multiple AZs.
- [ ] An RDS Multi-AZ database in private subnets.
- [ ] Highly available NAT gateways in public subnets for outbound internet access from the EC2 instances.

Why are the other answers wrong?

- [ ] Option B is wrong because: Deploying an Application Load Balancer in private subnets would make it an internal load balancer, which is not suitable for a public-facing ecommerce website that needs to receive traffic from the internet.
- [ ] Option C is wrong because: Placing the EC2 instances (application/web tier) in public subnets would expose them directly to the internet, which contradicts the requirement that "The EC2 instances... should not be exposed to the public internet."
- [ ] Option D (the first "D" in the PDF) is wrong because: While it correctly places the ALB in a public subnet and mentions two NAT gateways across two AZs (good for HA), stating "one public subnet, one private subnet" in total for a VPC spanning two AZs is an insufficient network design for high availability. For a robust two-AZ setup, you'd typically have at least one public and one private subnet in each of the two Availability Zones.

</details>

<details>
  <summary>Question 126</summary>

A solutions architect needs to implement a solution to reduce a company's storage costs. All the company's data is in the Amazon S3 Standard storage class. The company must keep all data for at least 25 years. Data from the most recent 2 years must be highly available and immediately retrievable.

Which solution will meet these requirements?

- [ ] A. Set up an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive immediately.
- [ ] B. Set up an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive after 2 years.
- [ ] C. Use S3 Intelligent-Tiering. Activate the archiving option to ensure that data is archived in S3 Glacier Deep Archive.
- [ ] D. Set up an S3 Lifecycle policy to transition objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) immediately and to S3 Glacier Deep Archive after 2 years.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Set up an S3 Lifecycle policy to transition objects to S3 Glacier Deep Archive after 2 years.

Why this is the correct answer:

This solution balances the need for immediate access to recent data with cost-effective long-term archival:

- [ ] Initial Storage (Implicitly S3 Standard): The data is currently in S3 Standard. For the "most recent 2 years," this data needs to be "highly available and immediately retrievable." S3 Standard meets these criteria.
- [ ] Transition to S3 Glacier Deep Archive after 2 Years: After 2 years, the data can be moved to a very low-cost archival storage class. Amazon S3 Glacier Deep Archive is AWS's lowest-cost storage class and is designed for long-term retention (like 25 years) of data that is rarely, if ever, accessed and for which retrieval times of several hours are acceptable. An S3 Lifecycle policy can be configured to automatically transition objects from their current storage class (e.g., S3 Standard or S3 Standard-IA if it was tiered there first) to S3 Glacier Deep Archive after they reach 2 years of age.
- [ ] Cost Reduction: This approach significantly reduces long-term storage costs by moving the bulk of the data (older than 2 years) to the most economical archive tier.
- [ ] Meets Retention and Accessibility Requirements: It ensures data is kept for 25 years, recent data (within 2 years) remains immediately accessible, and older data is archived cost-effectively.

Why are the other answers wrong?

- [ ] Option A is wrong because: Transitioning objects to S3 Glacier Deep Archive "immediately" would make the data from the "most recent 2 years" not immediately retrievable (retrieval takes hours) and not highly available in the sense of instant access. This violates a key requirement.
- [ ] Option C is wrong because: S3 Intelligent-Tiering is designed for data with unknown or changing access patterns. While it can be configured to automatically archive data to S3 Glacier Deep Archive, the access pattern here is somewhat defined (immediately accessible for 2 years, then archive). A direct S3 Lifecycle policy (as in option B) to transition to S3 Glacier Deep Archive after a fixed 2-year period is a more straightforward and potentially more cost-effective approach for this predictable archival need. S3 Intelligent-Tiering incurs small per-object monitoring fees for its automatic tiering capabilities.
- [ ] Option D is wrong because: Transitioning objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) "immediately" means that data, including the most recent 2 years' data, would be stored in a single Availability Zone. This makes it less durable and not "highly available" in the event of an AZ failure, which is risky for data that must be kept for 25 years. For the initial 2 years when data needs to be highly available, a multi-AZ storage class like S3 Standard or S3 Standard-IA would be more appropriate.

</details>

<details>
  <summary>Question 127</summary>

A media company is evaluating the possibility of moving its systems to the AWS Cloud. The company needs at least 10 TB of storage with the maximum possible I/O performance for video processing, 300 TB of very durable storage for storing media content, and 900 TB of storage to meet requirements for archival media that is not in use anymore.

Which set of services should a solutions architect recommend to meet these requirements?

- [ ] A. Amazon EBS for maximum performance, Amazon S3 for durable data storage, and Amazon S3 Glacier for archival storage
- [ ] B. Amazon EBS for maximum performance, Amazon EFS for durable data storage, and Amazon S3 Glacier for archival storage
- [ ] C. Amazon EC2 instance store for maximum performance, Amazon EFS for durable data storage, and Amazon S3 for archival storage
- [ ] D. Amazon EC2 instance store for maximum performance, Amazon S3 for durable data storage, and Amazon S3 Glacier for archival storage

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Amazon EC2 instance store for maximum performance, Amazon S3 for durable data storage, and Amazon S3 Glacier for archival storage

Why this is the correct answer:

This question asks for a set of storage services to meet three distinct requirements: maximum I/O performance for processing, durable storage for primary content, and archival storage.

- [ ] Amazon EC2 Instance Store for Maximum Performance (10 TB for video processing): EC2 instance stores provide temporary block-level storage that is physically attached to the host EC2 instance. NVMe-based instance stores offer very high IOPS and extremely low latency, making them ideal for workloads requiring the "maximum possible I/O performance," such as scratch space for video processing. While instance store is ephemeral (data is lost when the instance stops or terminates), it's suitable for temporary high-performance processing, with the source and destination data residing in durable storage.
- [ ] Amazon S3 for Durable Data Storage (300 TB for media content): Amazon S3 is designed for high durability (99.999999999%), availability, and scalability. It is an excellent choice for storing 300 TB of primary media content that needs to be "very durable." S3 is also cost-effective for storing large amounts of data.
- [ ] Amazon S3 Glacier for Archival Storage (900 TB for archival media): Amazon S3 Glacier (and its variants like S3 Glacier Deep Archive) provides secure, durable, and extremely low-cost storage for data archiving and long-term backup. It is perfect for "archival media that is not in use anymore," where retrieval times of minutes to hours are acceptable.
- [ ] This combination correctly matches each storage requirement with the most appropriate AWS service.

Why are the other answers wrong?

- [ ] Option A is wrong because: While high-performance Amazon EBS volumes (like io2 Block Express) can provide excellent I/O, EC2 instance stores (especially NVMe) generally offer the absolute "maximum possible I/O performance" due to their direct attachment and lack of network overhead, which might be preferred for intensive video processing scratch space. The rest of the option (S3 and S3 Glacier) is appropriate. The choice between EBS and instance store for "maximum performance" often hinges on whether persistence of that high-performance tier itself is required (EBS) or if it's purely for temporary, high-speed scratch space (instance store).
- [ ] Option B is wrong because: Using Amazon EFS for 300 TB of "durable data storage" for media content is generally more expensive per GB than Amazon S3. While EFS is durable and provides file system access, S3 is typically more cost-effective and better suited for storing large object-based media content unless a shared file system interface is strictly required for that primary content.
- [ ] Option C is wrong because:
Using Amazon EFS for 300 TB of durable media content storage has the same cost-effectiveness concern as in option B when compared to S3.
Using "Amazon S3 for archival storage" is too generic. While S3 has archive tiers (Glacier, Glacier Deep Archive), explicitly mentioning S3 Glacier (as in options A, B, and D) is more precise for the "archival media" requirement.

</details>

<details>
  <summary>Question 128</summary>

A company wants to run applications in containers in the AWS Cloud. These applications are stateless and can tolerate disruptions within the underlying infrastructure. The company needs a solution that minimizes cost and operational overhead.

What should a solutions architect do to meet these requirements?

- [ ] A. Use Spot Instances in an Amazon EC2 Auto Scaling group to run the application containers.
- [ ] B. Use Spot Instances in an Amazon Elastic Kubernetes Service (Amazon EKS) managed node group.
- [ ] C. Use On-Demand Instances in an Amazon EC2 Auto Scaling group to run the application containers.
- [ ] D. Use On-Demand Instances in an Amazon Elastic Kubernetes Service (Amazon EKS) managed node group.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use Spot Instances in an Amazon Elastic Kubernetes Service (Amazon EKS) managed node group.

Why this is the correct answer:

- [ ] Container Orchestration with Amazon EKS: Amazon EKS is a managed Kubernetes service that makes it easier to run Kubernetes on AWS without needing to install, operate, and maintain your own Kubernetes control plane. This helps manage containerized applications.   
- [ ] Cost Minimization with Spot Instances: The applications are "stateless and can tolerate disruptions." This makes them ideal candidates for Amazon EC2 Spot Instances, which offer significant cost savings (up to 90% off On-Demand prices) by allowing you to use spare EC2 capacity. Since the applications can handle interruptions, using Spot Instances directly addresses the "minimizes cost" requirement.
- [ ] EKS Managed Node Groups with Spot: EKS managed node groups automate the provisioning and lifecycle management of EC2 instances (nodes) for your EKS cluster. You can configure these managed node groups to use Spot Instances. This combines the orchestration benefits of Kubernetes with the cost savings of Spot Instances.
- [ ] Reduced Operational Overhead: Using EKS with managed node groups (especially compared to self-managing Kubernetes on EC2) reduces the operational overhead associated with the Kubernetes control plane and worker node management. While there's still some overhead compared to a fully serverless solution like Fargate (not an option here for EKS Spot directly), it's less than managing everything manually.
- [ ] This solution effectively balances running containerized applications with cost minimization and a reduction in operational overhead for the underlying compute infrastructure.

Why are the other answers wrong?

- [ ] Option A is wrong because: While using Spot Instances in an EC2 Auto Scaling group is cost-effective for disruption-tolerant workloads, it doesn't provide a container orchestration platform like Kubernetes (EKS) or ECS. If the company specifically wants to run applications "in containers" with the benefits of an orchestrator (like service discovery, deployment strategies, etc.), simply using EC2 Auto Scaling groups with Spot might require more manual effort to manage the container lifecycle.
- [ ] Option C is wrong because: Using On-Demand Instances is significantly more expensive than Spot Instances. Since the applications are stateless and can tolerate disruptions, there's a missed opportunity for cost savings if On-Demand Instances are used instead of Spot Instances. This does not meet the "minimizes cost" requirement as effectively.
- [ ] Option D is wrong because: Similar to option C, using On-Demand Instances in an EKS managed node group is more expensive than using Spot Instances. For workloads that are disruption-tolerant, Spot Instances provide a much better cost profile.

</details>

<details>
  <summary>Question 129</summary>

A company is running a multi-tier web application on premises. The web application is containerized and runs on a number of Linux hosts connected to a PostgreSQL database that contains user records. The operational overhead of maintaining the infrastructure and capacity planning is limiting the company's growth. A solutions architect must improve the application's infrastructure.

Which combination of actions should the solutions architect take to accomplish this? (Choose two.)

- [ ] A. Migrate the PostgreSQL database to Amazon Aurora.
- [ ] B. Migrate the web application to be hosted on Amazon EC2 instances.
- [ ] C. Set up an Amazon CloudFront distribution for the web application content.
- [ ] D. Set up Amazon ElastiCache between the web application and the PostgreSQL database.
- [ ] E. Migrate the web application to be hosted on AWS Fargate with Amazon Elastic Container Service (Amazon ECS).

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Migrate the PostgreSQL database to Amazon Aurora.
- [ ] E. Migrate the web application to be hosted on AWS Fargate with Amazon Elastic Container Service (Amazon ECS).

Why these are the correct answers:

This question focuses on reducing operational overhead and improving scalability for an existing containerized application and its database.

A. Migrate the PostgreSQL database to Amazon Aurora.
- [ ] Reduced Database Operational Overhead: Amazon Aurora is a fully managed relational database service compatible with MySQL and PostgreSQL.
- [ ] Migrating the on-premises PostgreSQL database to Amazon Aurora (PostgreSQL-compatible edition) significantly reduces the operational burden associated with database administration tasks like patching, backups, scaling, and ensuring high availability, as AWS manages these aspects.
- [ ] Improved Scalability and Availability: Aurora is designed for high performance, scalability, and availability, which helps in supporting the company's growth.

E. Migrate the web application to be hosted on AWS Fargate with Amazon Elastic Container Service (Amazon ECS).
- [ ] Reduced Web Tier Operational Overhead: The web application is already containerized. AWS Fargate is a serverless compute engine for containers that works with Amazon ECS (a container orchestration service).
- [ ] By migrating the containerized application to ECS using the Fargate launch type, the company eliminates the need to provision, manage, patch, or scale the underlying EC2 instances (the current Linux hosts). This directly addresses the problem of "operational overhead of maintaining the infrastructure and capacity planning" for the application tier.
- [ ] Scalability: ECS with Fargate allows the application to scale seamlessly based on demand without manual intervention for the underlying compute resources.
Combining these two solutions modernizes both the database and application tiers to managed/serverless AWS services, thereby reducing operational overhead and improving scalability.

Why are the other answers wrong?

- [ ] Option B is wrong because: Migrating the web application to self-managed Amazon EC2 instances, even if using Auto Scaling, still requires the company to manage the operating systems, patching, and some aspects of capacity planning for those instances. This does not minimize operational overhead as effectively as using a serverless container platform like AWS Fargate (Option E).
- [ ] Option C is wrong because: Amazon CloudFront is a content delivery network (CDN) that can improve website performance and reduce load by caching content. While beneficial, it doesn't directly address the core issue of operational overhead in maintaining the on-premises application servers or the database infrastructure. It's a complementary optimization, not a foundational solution for reducing the stated overhead.
- [ ] Option D is wrong because: Amazon ElastiCache is an in-memory caching service that can reduce latency and database load by caching frequently accessed data. While this can improve performance, it does not reduce the operational overhead of managing the PostgreSQL database itself or the application hosting infrastructure. Migrating to managed services like Aurora and Fargate addresses the operational overhead more directly.


</details>

<details>
  <summary>Question 130</summary>

An application runs on Amazon EC2 instances across multiple Availability Zones. The instances run in an Amazon EC2 Auto Scaling group behind an Application Load Balancer. The application performs best when the CPU utilization of the EC2 instances is at or near 40%.

What should a solutions architect do to maintain the desired performance across all instances in the group?

- [ ] A. Use a simple scaling policy to dynamically scale the Auto Scaling group.
- [ ] B. Use a target tracking policy to dynamically scale the Auto Scaling group.
- [ ] C. Use an AWS Lambda function to update the desired Auto Scaling group capacity.
- [ ] D. Use scheduled scaling actions to scale up and scale down the Auto Scaling group.

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Use a target tracking policy to dynamically scale the Auto Scaling group.

Why this is the correct answer:

- [ ] Target Tracking Scaling Policy: This is an Amazon EC2 Auto Scaling feature that allows you to define a target value for a specific CloudWatch metric (such as average CPU utilization for the Auto Scaling group). The Auto Scaling group then automatically adjusts the number of instances (scales out or scales in) as needed to keep the specified metric at, or close to, the target value you've set.
- [ ] Maintaining Specific CPU Utilization: The requirement is to keep the CPU utilization of the EC2 instances "at or near 40%." A target tracking scaling policy with the AverageCPUUtilization metric set to a target of 40 is precisely designed for this scenario. It provides a dynamic and responsive way to maintain the desired performance level.
- [ ] Simplicity and Efficiency: Target tracking policies are generally simpler to configure and manage than simple or step scaling policies because you only need to define the target metric and value. AWS Auto Scaling handles the underlying calculations for how many instances to add or remove.

Why are the other answers wrong?

- [ ] Option A is wrong because: Simple scaling policies adjust the current capacity based on a single scaling adjustment after a CloudWatch alarm threshold is breached. They require a cooldown period after each scaling activity before they can respond to further alarms. This can lead to slower or less precise adjustments compared to target tracking, which continuously works to keep the metric near the target.
- [ ] Option C is wrong because: While it's possible to use an AWS Lambda function to monitor CloudWatch metrics and programmatically adjust the Auto Scaling group's desired capacity, this is a custom solution that involves writing, deploying, and maintaining code. This introduces more operational overhead compared to using the built-in target tracking scaling policies provided by Auto Scaling.
- [ ] Option D is wrong because: Scheduled scaling actions are used to scale capacity based on predictable, time-based patterns (e.g., increasing capacity during business hours and decreasing it at night). The scenario does not indicate a predictable load schedule but rather a desired CPU utilization level that needs to be maintained dynamically regardless of the time. Target tracking scaling is more appropriate for responding to actual load and maintaining a performance metric.

</details>

</details>

<details>
  <summary>==Questions 131-140==</summary>

<details>
  <summary>Question 131</summary>

- [ ] A.  Turn  
A company is developing a file-sharing application that will use an Amazon S3 bucket for storage. The company wants to serve all the files through an Amazon CloudFront distribution. The company does not want the files to be accessible through direct navigation to the S3 URL.

What should a solutions architect do to meet these requirements?

- [ ] A. Write individual policies for each S3 bucket to grant read permission for only CloudFront access.
- [ ] B. Create an IAM user. Grant the user read permission to objects in the S3 bucket. Assign the user to CloudFront.
- [ ] C. Write an S3 bucket policy that assigns the CloudFront distribution ID as the Principal and assigns the target S3 bucket as the Amazon Resource Name (ARN).
- [ ] D. Create an origin access identity (OAI). Assign the OAI to the CloudFront distribution. Configure the S3 bucket permissions so that only the OAI has read permission.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Create an origin access identity (OAI). Assign the OAI to the CloudFront distribution. Configure the S3 bucket permissions so that only the OAI has read permission.

Why this is the correct answer:

This solution uses the standard and recommended method for restricting S3 bucket access to only a CloudFront distribution:

- [ ] Origin Access Identity (OAI): An OAI is a special CloudFront user identity that you create and associate with your CloudFront distribution. This OAI acts as a trusted principal that CloudFront uses when accessing your S3 bucket.
- [ ] Restricting S3 Bucket Permissions to OAI: You then modify the S3 bucket policy (or ACLs, though bucket policies are generally preferred for more granular control) to grant read permissions (e.g., s3:GetObject) only to this specific OAI. All other public access or access by other IAM principals to the S3 bucket is denied or not granted.
- [ ] Result: When users request files through the CloudFront distribution, CloudFront uses its associated OAI to fetch the files from the S3 bucket. Since the bucket policy only allows the OAI to read the objects, direct S3 URLs will not work for users, thus meeting the requirement that files are not accessible through direct S3 navigation.
- [ ] Note: AWS has also introduced Origin Access Control (OAC) as an enhanced alternative to OAI, offering better security and features, but OAI is the classic method and still valid, especially in the context of these exam options.

Why are the other answers wrong?

- [ ] Option A is wrong because: While bucket policies are used, this option is too vague. "Grant read permission for only CloudFront access" needs a specific mechanism. Simply stating this doesn't explain how CloudFront's access is identified and restricted. Option D provides that specific mechanism (OAI).
- [ ] Option B is wrong because: You do not assign an IAM user to CloudFront for origin access. CloudFront does not use IAM user credentials in this manner to access S3 buckets. The OAI (or OAC) mechanism is used for this purpose.
- [ ] Option C is wrong because: You cannot directly assign a CloudFront distribution ID as a Principal in an S3 bucket policy in the way described. The Principal in the bucket policy, when using OAI, refers to the canonical user ID of the OAI. If using OAC, the principal is cloudfront.amazonaws.com with a condition based on the distribution.

</details>  

<details>
  <summary>Question 132</summary>

A company's website provides users with downloadable historical performance reports. The website needs a solution that will scale to meet the company's website demands globally. The solution should be cost-effective, limit the provisioning of infrastructure resources, and provide the fastest possible response time.

Which combination should a solutions architect recommend to meet these requirements?

- [ ] A. Amazon CloudFront and Amazon S3
- [ ] B. AWS Lambda and Amazon DynamoDB
- [ ] C. Application Load Balancer with Amazon EC2 Auto Scaling
- [ ] D. Amazon Route 53 with internal Application Load Balancers

</details>

<details>
  <summary>Answer</summary>

- [ ] A. Amazon CloudFront and Amazon S3

Why this is the correct answer:

This solution leverages AWS services designed for scalable, cost-effective, and low-latency global content delivery:

- [ ] Amazon S3 for Storing Reports: Amazon S3 (Simple Storage Service) is an ideal place to store the "downloadable historical performance reports." S3 offers high durability, scalability, and is very cost-effective for storing static files.
- [ ] Amazon CloudFront for Global Delivery and Performance: Amazon CloudFront is a global content delivery network (CDN). By configuring CloudFront with the S3 bucket as its origin, the performance reports will be cached at edge locations geographically closer to users around the world. When a user requests a report, it is served from the nearest edge location, which significantly reduces latency and provides the "fastest possible response time."
- [ ] Scalability: Both S3 and CloudFront are highly scalable services that can automatically handle large and fluctuating demand globally.
- [ ] Cost-Effectiveness and Limited Infrastructure Provisioning: This is a serverless approach for content storage and delivery. There are no servers to manage, which "limits the provisioning of infrastructure resources" and reduces operational overhead. This combination is generally very "cost-effective" for distributing static content globally.

Why are the other answers wrong?

- [ ] Option B is wrong because: AWS Lambda and Amazon DynamoDB are powerful serverless components for building application backends (e.g., processing requests, storing structured data). However, for serving static downloadable files like performance reports globally with the fastest response time, S3 and CloudFront are more direct and optimized. Lambda and DynamoDB might be used to generate the reports or manage metadata about them, but not for the primary delivery of the files themselves.
- [ ] Option C is wrong because: Using an Application Load Balancer with Amazon EC2 Auto Scaling is a solution for hosting dynamic web applications that require compute instances. For serving downloadable static reports, this approach involves managing EC2 instances, which increases operational overhead and cost compared to a serverless S3/CloudFront solution. It does not align well with "limit the provisioning of infrastructure resources."
- [ ] Option D is wrong because: Amazon Route 53 is a DNS service used for routing users to endpoints. Internal Application Load Balancers are designed for distributing traffic to applications within your VPC, not for serving content globally to external users with the fastest possible response time. CloudFront is the key AWS service for global, low-latency content delivery to end-users.

</details>

<details>
  <summary>Question 133</summary>

A company runs an Oracle database on premises. As part of the company's migration to AWS, the company wants to upgrade the database to the most recent available version. The company also wants to set up disaster recovery (DR) for the database. The company needs to minimize the operational overhead for normal operations and DR setup. The company also needs to maintain access to the database's underlying operating system.

Which solution will meet these requirements?

- [ ] A. Migrate the Oracle database to an Amazon EC2 instance. Set up database replication to a different AWS Region.
- [ ] B. Migrate the Oracle database to Amazon RDS for Oracle. Activate Cross-Region automated backups to replicate the snapshots to another AWS Region.
- [ ] C. Migrate the Oracle database to Amazon RDS Custom for Oracle. Create a read replica for the database in another AWS Region.
- [ ] D. Migrate the Oracle database to Amazon RDS for Oracle. Create a standby database in another Availability Zone.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Migrate the Oracle database to Amazon RDS Custom for Oracle. Create a read replica for the database in another AWS Region.

Why this is the correct answer:

This question has several key requirements: upgrade flexibility, disaster recovery (DR), minimized operational overhead, and access to the underlying operating system.

- [ ] Maintain Access to Underlying OS (Amazon RDS Custom for Oracle): A critical requirement is to "maintain access to the database's underlying operating system." Standard Amazon RDS is fully managed and does not provide OS access. Amazon RDS Custom for Oracle, however, gives you administrative access to the underlying EC2 instance and the database environment. This allows for customizations, specific configurations, and the ability to apply patches or upgrades that might not yet be supported by standard RDS.
- [ ] Upgrade to Most Recent Version: With OS and database environment access provided by RDS Custom, the company has more control and flexibility to "upgrade the database to the most recent available version," even if that version isn't immediately available or fully supported in standard RDS.
- [ ] Disaster Recovery (Cross-Region Read Replica): For Amazon RDS Custom for Oracle, you can set up a cross-region read replica. In a DR scenario, this read replica in a different AWS Region can be promoted to become a standalone, writable instance. This provides a DR solution.
- [ ] Minimize Operational Overhead (Relative to EC2): While RDS Custom requires more customer management responsibility than standard RDS (due to OS access), it still automates some database administration tasks (like provisioning and some backup aspects) compared to running an Oracle database entirely on self-managed EC2 instances. For setting up DR via a managed read replica, it is also less overhead than manually configuring and managing replication for Oracle on EC2.
- [ ] This option best balances the need for OS access with managed features for DR and somewhat reduced operational overhead compared to a full EC2 deployment.

Why are the other answers wrong?

- [ ] Option A is wrong because: Migrating to an Oracle database on a self-managed Amazon EC2 instance provides full OS access. However, setting up and managing database replication to a different AWS Region for DR would involve significant operational overhead for configuration, monitoring, patching the OS and database, and managing the replication process itself. This contradicts the requirement to "minimize the operational overhead for... DR setup."
- [ ] Option B is wrong because: Standard Amazon RDS for Oracle is a fully managed service and does not provide access to the underlying operating system, which is a key requirement. While Cross-Region automated backups are a valid DR mechanism for standard RDS, the lack of OS access makes this option unsuitable.
- [ ] Option D is wrong because:
Standard Amazon RDS for Oracle does not provide OS access.
Creating a standby database in another Availability Zone (a Multi-AZ deployment) is for high availability within a single region, protecting against an AZ failure. It is not a cross-region disaster recovery solution.

</details>

<details>
  <summary>Question 134</summary>

- [ ] A.  Turn  
A company wants to move its application to a serverless solution. The serverless solution needs to analyze existing and new data by using SQL. The company stores the data in an Amazon S3 bucket. The data requires encryption and must be replicated to a different AWS Region.

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A. Create a new S3 bucket. Load the data into the new S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with AWS KMS multi-Region keys (SSE-KMS). Use Amazon Athena to query the data.
- [ ] B. Create a new S3 bucket. Load the data into the new S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with AWS KMS multi-Region keys (SSE-KMS). Use Amazon RDS to query the data.
- [ ] C. Load the data into the existing S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use Amazon Athena to query the data.
- [ ] D. Load the data into the existing S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use Amazon RDS to query the data.

</details>

<details>
  <summary>Answer</summary>

- [ ] C. Load the data into the existing S3 bucket. Use S3 Cross-Region Replication (CRR) to replicate encrypted objects to an S3 bucket in another Region. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use Amazon Athena to query the data.

Why this is the correct answer:

This solution focuses on serverless components and minimizing operational overhead.

- [ ] Using Existing S3 Bucket: Loading data into an existing S3 bucket is straightforward and avoids the setup of a new one if not strictly necessary.
S3 Cross-Region Replication (CRR): CRR automatically and asynchronously copies objects across buckets in different AWS Regions. This meets the requirement that "data must be replicated to a different AWS Region."
- [ ] Server-Side Encryption with S3-Managed Keys (SSE-S3): SSE-S3 provides encryption at rest where Amazon S3 manages both the encryption process and the encryption keys. This is the simplest way to achieve encryption in S3 with the absolute "LEAST operational overhead" because there are no keys for the customer to manage. When using CRR with SSE-S3 encrypted objects, S3 handles the re-encryption in the destination region transparently.
- [ ] Amazon Athena for SQL Analysis: Amazon Athena is a serverless, interactive query service that makes it easy to analyze data directly in Amazon S3 using standard SQL. This perfectly fits the requirement to "analyze existing and new data by using SQL" in a serverless manner with minimal setup or management.   
- [ ] Overall Least Operational Overhead: This combination leverages fully managed and serverless AWS services for storage, replication, encryption, and querying, leading to the lowest possible operational burden.

Why are the other answers wrong?

- [ ] Option A is wrong because: While using server-side encryption with AWS KMS multi-Region keys (SSE-KMS) provides more customer control over the encryption keys and allows the same key to be used across regions, it generally involves slightly more operational overhead than SSE-S3. With SSE-KMS, you need to create and manage the KMS key, its policies, and potentially its rotation (though automatic rotation is available for KMS-managed CMKs). SSE-S3 is simpler from a key management perspective if the only requirement is that data "requires encryption."
- [ ] Option B is wrong because:
Similar to option A, SSE-KMS has slightly more operational overhead for key management than SSE-S3.
More significantly, using Amazon RDS (a relational database service) to query data stored directly in an Amazon S3 bucket is not its primary design. While some RDS engines might offer ways to load data from S3 or link to it (like Aurora querying S3), Amazon Athena is the purpose-built, serverless SQL query engine for data in S3. Using RDS would introduce the operational overhead of managing a database instance.
- [ ] Option D is wrong because: Similar to option B, using Amazon RDS to query data directly from S3 is not the most operationally efficient or standard serverless approach. Athena is the correct tool for serverless SQL queries on S3.

</details>

<details>
  <summary>Question 135</summary>

A company runs workloads on AWS. The company needs to connect to a service from an external provider. The service is hosted in the provider's VPC. According to the company's security team, the connectivity must be private and must be restricted to the target service. The connection must be initiated only from the company's VPC.

Which solution will meet these requirements?

- [ ] A. Create a VPC peering connection between the company's VPC and the provider's VPC. Update the route table to connect to the target service.
- [ ] B. Ask the provider to create a virtual private gateway in its VPC. Use AWS PrivateLink to connect to the target service.
- [ ] C. Create a NAT gateway in a public subnet of the company's VPC. Update the route table to connect to the target service.
- [ ] D. Ask the provider to create a VPC endpoint for the target service. Use AWS PrivateLink to connect to the target service.

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Ask the provider to create a VPC endpoint for the target service. Use AWS PrivateLink to connect to the target service.

Why this is the correct answer:

- [ ] (Note: The PDF phrasing "provider to create a VPC endpoint" is slightly imprecise. The provider creates an endpoint service, and the company creates an interface VPC endpoint to connect to that service.)
- [ ] AWS PrivateLink for Private and Secure Connectivity: AWS PrivateLink is designed to provide secure, private connectivity between your VPCs, AWS services, and your on-premises applications, without exposing your traffic to the public internet. This directly addresses the "connectivity must be private" requirement.
- [ ] Provider Creates Endpoint Service: The external provider would host their service behind a Network Load Balancer (NLB) and then create a VPC endpoint service configuration for it. This makes their service available for private connections via PrivateLink.
- [ ] Company Creates Interface Endpoint: The company (consumer) then creates an interface VPC endpoint in their own VPC. This endpoint gets a private IP address from the company's subnet and acts as an entry point to the provider's service.
- [ ] Restricted to Target Service: Connectivity via PrivateLink is specific to the service endpoint defined by the provider. This ensures that access is "restricted to the target service," not the provider's entire VPC.
- [ ] Connection Initiated from Company's VPC: The connection is initiated from the company's VPC through the interface endpoint to the provider's service. Traffic flows unidirectionally in terms of initiation, from consumer to provider service.
- [ ] No Internet Exposure: Traffic stays within the AWS private network.

Why are the other answers wrong?

- [ ] Option A is wrong because: VPC peering establishes a network connection between two VPCs, enabling them to communicate as if they are on the same private network. However, VPC peering connects the VPCs at a network level, potentially exposing more than just the target service unless very strict security group and network ACL rules are applied. It also doesn't work if the VPCs have overlapping CIDR blocks. PrivateLink offers more granular, service-specific private connectivity.
- [ ] Option B is wrong because: A virtual private gateway is used on the VPC side to establish a VPN connection or an AWS Direct Connect connection, typically to an on-premises network. It's not the component a service provider creates in their VPC for consumers to connect via PrivateLink. The provider would create an endpoint service.
- [ ] Option C is wrong because: A NAT gateway is used to enable instances in a private subnet to initiate outbound connections to the internet. This solution would route traffic over the public internet to access the provider's service (if it had a public endpoint), which violates the "connectivity must be private" requirement.

</details>

<details>
  <summary>Question X</summary>

- [ ] A.  Turn  


</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn


</details>


<details>
  <summary>Question X</summary>

- [ ] A.  Turn  


</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn


</details>












</details>















<details>
  <summary>Question X</summary>

- [ ] A.  Turn  


</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn


</details>




<details>
  <summary>==Questions X-X==</summary>

  


</details>






