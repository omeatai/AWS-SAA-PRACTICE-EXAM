# AWS-SAA-PRACTICE-EXAM
AWS-SAA-PRACTICE-EXAM

<details>
  <summary>==Questions 1-10==</summary>
  
<details>
  <summary>Question 1</summary>
 
A company collects data for temperature, humidity, and atmospheric pressure in cities across multiple continents.  The average volume of data that the company collects from each site daily is 500 GB.  Each site has a high-speed internet connection.  The company wants to aggregate the data from all these global sites as quickly as possible in a single Amazon S3 bucket.  The solution must minimize operational complexity.  

Which solution meets these requirements?

- [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    
- [ ] B.  Upload the data from each site to an S3 bucket in the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.  Then remove the data from the origin S3 bucket.    
- [ ] C.  Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.    
- [ ] D.  Upload the data from each site to an Amazon EC2 instance in the closest Region.  Store the data in an Amazon Elastic Block Store (Amazon EBS) volume.  At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket.  Restore the EBS volume in that Region. 

</details>

<details>
  <summary>Answer</summary>

  - [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    

  The correct answer is A. Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.   

  Why is this the correct answer?
  
  - [ ] S3 Transfer Acceleration: This feature is designed to speed up transfers of data over long distances between your client and your S3 bucket. It leverages Amazon's CloudFront edge locations to optimize the network path, reducing latency and increasing throughput. Given that the company is transferring data from sites across multiple continents, S3 Transfer Acceleration is highly relevant.
  - [ ] Multipart Uploads: This feature allows you to upload a single object as a set of parts. Multipart uploads can improve throughput, provide faster recovery from network issues, and support uploading large objects (up to 5 TB). Since the company is dealing with 500 GB of data daily from each site, multipart uploads are a good practice to ensure efficient and reliable uploads.
  - [ ] Minimal Operational Complexity: This solution is relatively straightforward to implement. You enable S3 Transfer Acceleration on the bucket and configure your uploads to use the multipart upload API. This avoids the need for complex infrastructure setups or data transfer orchestration.
  
  Why are the other answers wrong?
  
  B. Upload the data from each site to an S3 bucket in the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket. Then remove the data from the origin S3 bucket.   
  
  Why it's wrong: 
  
  While this approach could work, it adds operational complexity. It involves managing S3 buckets in multiple regions, setting up and monitoring Cross-Region Replication, and ensuring data is deleted from the origin buckets. This adds more steps and potential points of failure compared to directly uploading with S3 Transfer Acceleration and multipart uploads. Also, Cross-Region Replication is optimized for asynchronous copying, which might not meet the requirement of getting the data into the central bucket "as quickly as possible."
  
  C. Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.   
  
  Why it's wrong: 
  
  AWS Snowball Edge is designed for large-scale data transfers where network bandwidth is limited or cost-prohibitive. In this scenario, each site has a high-speed internet connection, making Snowball Edge unnecessary and adding significant operational overhead. Managing Snowball Edge devices, shipping them, and coordinating the data transfer process is far more complex than using direct uploads.
  
  D. Upload the data from each site to an Amazon EC2 instance in the closest Region. Store the data in an Amazon Elastic Block Store (Amazon EBS) volume. At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket. Restore the EBS volume in that Region.   
  
  Why it's wrong: 
  
  This solution is overly complex and inefficient. It involves managing EC2 instances and EBS volumes in multiple regions, dealing with EBS snapshots, and restoring volumes. This adds significant operational overhead and introduces potential performance bottlenecks. It's also more costly than directly uploading to S3.
  
  Summary
  
  The best solution is A. Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket. This approach provides the fastest data transfer over long distances with the least operational complexity.

</details>

<details>
  <summary>Question 2</summary>

  A company runs a web application that stores session state in memory on Amazon EC2 instances. The company wants to implement a solution to store session state data durably and elastically. The solution must minimize operational overhead.
  
  Which solution meets these requirements?
  
  - [ ] A. Use Amazon DynamoDB to store the session state data. Modify the application to read and write session state data to DynamoDB.
  - [ ] B. Use Amazon ElastiCache for Redis to store the session state data. Modify the application to read and write session state data to ElastiCache.
  - [ ] C. Use Amazon EC2 instance store volumes to store the session state data. Configure the application to replicate session state data across multiple EC2 instances.
  - [ ] D. Use Amazon Elastic File System (Amazon EFS) to store the session state data. Modify the application to read and write session state data to Amazon EFS.

</details>

<details>
  <summary>Answer</summary>

  - [ ] B. Use Amazon ElastiCache for Redis to store the session state data. Modify the application to read and write session state data to ElastiCache.

  The correct answer is B. Use Amazon ElastiCache for Redis to store the session state data. Modify the application to read and write session state data to ElastiCache.

  Why is this the correct answer?
  
  - [ ] Durable and Elastic: ElastiCache (both Redis and Memcached) provides an in-memory data store that is highly available and scalable.
  - [ ] Redis, in particular, offers durability options (RDB and AOF persistence) to ensure that session data is not lost in case of failures.
  - [ ] ElastiCache can elastically scale to handle increasing session loads.
  - [ ] Minimizes Operational Overhead: ElastiCache is a managed service.
  - [ ] AWS handles the setup, patching, scaling, and maintenance of the cache infrastructure, reducing the operational burden on the company.
  
  Why are the other answers wrong?
  
  A. Use Amazon DynamoDB to store the session state data. Modify the application to read and write session state data to DynamoDB.
  
  Why it's wrong: While DynamoDB is durable and scalable, it's a NoSQL database, not an in-memory cache. Accessing DynamoDB is generally slower than accessing an in-memory cache like ElastiCache. For session state, low latency is crucial for a good user experience. DynamoDB might introduce more latency than ElastiCache.
  
  C. Use Amazon EC2 instance store volumes to store the session state data. Configure the application to replicate session state data across multiple EC2 instances.
  
  Why it's wrong: EC2 instance store volumes are ephemeral, meaning the data is lost if the instance stops, terminates, or fails. Relying on instance store for session state would require complex application-level replication, which introduces significant operational overhead and potential data loss. This solution is neither durable nor operationally simple.
  
  D. Use Amazon Elastic File System (Amazon EFS) to store the session state data. Modify the application to read and write session state data to Amazon EFS.
  
  Why it's wrong: EFS is a file storage service. File system operations are generally slower than in-memory operations. Using EFS for session state would introduce latency and negatively impact application performance. It's not designed for the high-speed, low-latency access required for session data.
  
  Summary
  
  ElastiCache for Redis is the best solution because it provides a durable, elastic, and managed in-memory store that minimizes operational overhead and meets the performance requirements for session state management.
    
</details>


<details>
  <summary>Question 3</summary>

  A company uses AWS Organizations to manage multiple AWS accounts for different departments.  The management account has an Amazon S3 bucket that contains project reports.  The company wants to limit access to this S3 bucket to only users of accounts within the organization in AWS Organizations.  Which solution meets these requirements with the LEAST amount of operational overhead?    
  
  - [ ] A.  Add the aws:PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy.    
  - [ ] B.  Create an organizational unit (OU) for each department.  Add the aws:PrincipalOrgPaths global condition key to the S3 bucket policy.    
  - [ ] C.  Use AWS CloudTrail to monitor the CreateAccount, InviteAccountToOrganization, LeaveOrganization, and RemoveAccountFrom Organization events.  Update the S3 bucket policy accordingly.    
  - [ ] D.  Tag each user that needs access to the S3 bucket.  Add the aws: PrincipalTag global condition key to the S3 bucket policy.    

</details>

<details>
  <summary>Answer</summary>

  - [ ] A. Add the aws:PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy.

  The correct answer is A. Add the aws:PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy.

Why is this the correct answer?

aws:PrincipalOrgID for Organization-Wide Access: The aws:PrincipalOrgID condition key in an S3 bucket policy allows you to restrict access to AWS principals (users, roles) that belong to a specific AWS organization. This is the most efficient and direct way to enforce the requirement that only accounts within your AWS Organizations can access the bucket.
Least Operational Overhead: This method involves a simple modification to the S3 bucket policy. You add a condition that checks the organization ID, which is a static identifier. There's no need to manage individual accounts, OUs, or tags, resulting in minimal administrative effort.
Why are the other answers wrong?

B. Create an organizational unit (OU) for each department. Add the aws:PrincipalOrgPaths global condition key to the S3 bucket policy.   

Why it's wrong: While this could work, it's more complex than necessary. It requires you to create and maintain OUs, which adds organizational structure overhead. The aws:PrincipalOrgPaths condition key restricts access based on the OU hierarchy, which might be useful in some cases but is not needed for the simple requirement of granting access to the entire organization. If the organization structure changes, you might need to update the bucket policy.
C. Use AWS CloudTrail to monitor the CreateAccount, InviteAccountToOrganization, LeaveOrganization, and RemoveAccountFrom Organization events. Update the S3 bucket policy accordingly.

Why it's wrong: This is an extremely complex and inefficient solution. It involves:
Setting up CloudTrail to log organization events.
Creating an automated process (e.g., Lambda function) to analyze CloudTrail logs.
Parsing the logs to identify changes in the organization membership.
Dynamically updating the S3 bucket policy based on those changes. This approach introduces significant operational overhead, potential latency in policy updates, and increased risk of errors.
D. Tag each user that needs access to the S3 bucket. Add the aws: PrincipalTag global condition key to the S3 bucket policy.   

Why it's wrong: This is also complex and difficult to maintain. It requires:
Implementing a system to tag IAM users.
Ensuring that all relevant users are correctly tagged.
Updating tags whenever users are added or removed.
The aws:PrincipalTag condition key is useful for attribute-based access control (ABAC) but is not the most efficient way to control access based on organization membership.
Summary

The best solution is A. Add the aws:PrincipalOrgID global condition key because it's the simplest, most direct, and least operationally intensive way to restrict S3 bucket access to accounts within an AWS organization.

</details>

<details>
  <summary>Question 4</summary>

An application runs on an Amazon EC2 instance in a VPC.  The application processes logs that are stored in an Amazon S3 bucket.  The EC2 instance needs to access the S3 bucket without connectivity to the internet.  Which solution will provide private network connectivity to Amazon S3?   

- [ ] A.  Create a gateway VPC endpoint to the S3 bucket.    
- [ ] B.  Stream the logs to Amazon CloudWatch Logs.  Export the logs to the S3 bucket.    
- [ ] C.  Create an instance profile on Amazon EC2 to allow S3 access.    
- [ ] D.  Create an Amazon API Gateway API with a private link to access the S3 endpoint.  

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Create a gateway VPC endpoint to the S3 bucket.  

The correct answer is A. Create a gateway VPC endpoint to the S3 bucket.

Why is this the correct answer?

- [ ] Gateway VPC Endpoint for Private Connectivity: A gateway VPC endpoint for S3 enables instances within your VPC to access S3 over the AWS private network, without traversing the internet.
- [ ] This provides secure and private connectivity, satisfying the requirement to access the S3 bucket without internet access.

Why are the other answers wrong?

B. Stream the logs to Amazon CloudWatch Logs. Export the logs to the S3 bucket.

Why it's wrong: While CloudWatch Logs is useful for log management, this solution doesn't address the requirement for private connectivity. Exporting logs from CloudWatch Logs to S3 still involves network traffic, and it doesn't guarantee that the initial application access to S3 is private. It also adds an unnecessary intermediary step.

C. Create an instance profile on Amazon EC2 to allow S3 access.

Why it's wrong: An instance profile (which uses IAM roles) grants permissions to the EC2 instance to access AWS services like S3. However, it does not, by itself, provide private connectivity. The instance would still use public endpoints to access S3 unless a VPC endpoint is configured.

D. Create an Amazon API Gateway API with a private link to access the S3 endpoint.

Why it's wrong: While PrivateLink does provide private connectivity, using API Gateway in this scenario is overly complex and not the intended use case. API Gateway is designed for creating and managing APIs, not for direct, high-volume data access to S3 from EC2 instances. Gateway VPC endpoints are simpler and more efficient for this purpose.

Summary

The best solution is A. Create a gateway VPC endpoint to the S3 bucket because it directly and efficiently provides the required private connectivity between the EC2 instance and the S3 bucket.

</details>

<details>
  <summary>Question 5</summary>

A company is hosting a web application on AWS using a single Amazon EC2 instance that stores user-uploaded documents in an Amazon EBS volume.  For better scalability and availability, the company duplicated the architecture and created a second EC2 instance and EBS volume in another Availability Zone, placing both behind an Application Load Balancer.  After completing this change, users reported that, each time they refreshed the website, they could see one subset of their documents or the other, but never all of the documents at the same time.  What should a solutions architect propose to ensure users see all of their documents at once?   

- [ ] A.  Copy the data so both EBS volumes contain all the documents
- [ ] B.  Configure the Application Load Balancer to direct a user to the server with the documents
- [ ] C.  Copy the data from both EBS volumes to Amazon EFS.  Modify the application to save new documents to Amazon EFS   
- [ ] D.  Configure the Application Load Balancer to send the request to both servers.  Return each document from the correct server   

</details>

<details>
  <summary>Answer</summary>

- [ ] C.  Copy the data from both EBS volumes to Amazon EFS.  Modify the application to save new documents to Amazon EFS.

The correct answer is C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS.

Why is this the correct answer?

- [ ] Shared File System: Amazon EFS provides a shared file system that can be mounted by multiple EC2 instances concurrently. By migrating the documents to EFS, both EC2 instances can access the same set of files, resolving the issue where users only see a subset of their documents depending on which instance they hit.
- [ ] Persistence and Availability: EFS is designed for high availability and durability. The data is stored redundantly across multiple Availability Zones, ensuring that files are not lost if an instance fails.
- [ ] Scalability: EFS automatically scales its storage capacity as you add or remove files, making it suitable for handling a growing number of user-uploaded documents.
Why are the other answers wrong?

A. Copy the data so both EBS volumes contain all the documents.

Why it's wrong: While this might seem like a straightforward solution, it introduces significant challenges. You would need to implement a synchronization mechanism to keep the EBS volumes in sync, which can be complex, error-prone, and introduce latency. It also doesn't address the underlying architecture issue of each instance having its own storage.

B. Configure the Application Load Balancer to direct a user to the server with the documents.

Why it's wrong: This approach is not scalable or highly available. It creates "sticky sessions," meaning a user is always routed to the same EC2 instance. If that instance fails, the user loses access to their documents. It also doesn't solve the problem of data consistency.

D. Configure the Application Load Balancer to send the request to both servers. Return each document from the correct server.

Why it's wrong: This is also complex and inefficient. It would require significant application logic to track which documents are stored on which server and then merge the results. It also increases the load on both servers, as they would both need to process each request.

Summary

The best solution is C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS because it provides a shared, scalable, and durable storage solution that eliminates data inconsistency and improves the application's availability.

</details>

<details>
  <summary>Question 6</summary>

A company uses NFS to store large video files in on-premises network attached storage.  Each video file ranges in size from 1 MB to 500 GB.  The total storage is 70 TB and is no longer growing.  The company decides to migrate the video files to Amazon S3.  The company must migrate the video files as soon as possible while using the least possible network bandwidth.    

Which solution will meet these requirements?

- [ ] A.  Create an S3 bucket.  Create an IAM role that has permissions to write to the S3 bucket.  Use the AWS CLI to copy all files locally to the S3 bucket.    
- [ ] B.  Create an AWS Snowball Edge job.  Receive a Snowball Edge device on premises.  Use the Snowball Edge client to transfer data to the device.  Return the device so that AWS can import the data into Amazon S3.    
- [ ] C.  Deploy an S3 File Gateway on premises.  Create a public service endpoint to connect to the S3 File Gateway.  Create an S3 bucket.  Create a new NFS file share on the S3 File Gateway.  Point the new file share to the S3 bucket.  Transfer the data from the existing NFS file share to the S3 File Gateway.    
- [ ] D.  Set up an AWS Direct Connect connection between the on-premises network and AWS.  Deploy an S3 File Gateway on premises.  Create a public virtual interface (VIF) to connect to the S3 File Gateway.  Create an S3 bucket.   Create a new NFS file share on the S3 File Gateway.  Point the new file share to the S3 bucket.  Transfer the data from the existing NFS file share to the S3 File Gateway.    

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3.    

The correct answer is B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3.    

Why is this the correct answer?

- [ ] Least Network Bandwidth: AWS Snowball Edge is a physical device that you can use to transfer large amounts of data into and out of AWS.  It's designed for situations where transferring data over the internet would be slow or expensive due to limited network bandwidth.  In this case, the requirement to use the "least possible network bandwidth" strongly points to Snowball Edge.   
- [ ] Speed for Large Data: For 70 TB of data, even with a good internet connection, the time to transfer all that data can be significant. Snowball Edge allows for much faster transfer of large volumes of data compared to online methods.    
- [ ] One-time Migration: The scenario describes a one-time migration of existing data, not an ongoing synchronization. Snowball Edge is well-suited for this type of bulk transfer.    

Why are the other answers wrong?

A. Create an S3 bucket. Create an IAM role that has permissions to write to the S3 bucket. Use the AWS CLI to copy all files locally to the S3 bucket.

Why it's wrong: This solution relies on transferring 70 TB of data over the network using the AWS CLI.  This would consume a large amount of network bandwidth and take a significant amount of time, directly contradicting the requirements.   

C. Deploy an S3 File Gateway on premises. Create a public service endpoint to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.

Why it's wrong: AWS S3 File Gateway is designed for hybrid cloud storage, providing on-premises applications access to S3.  While it could be used for migration, it's more complex than Snowball Edge for a one-time transfer. It still involves network transfer, although it might be optimized, it doesn't minimize bandwidth usage to the same extent as Snowball Edge.    

D. Set up an AWS Direct Connect connection between the on-premises network and AWS. Deploy an S3 File Gateway on premises. Create a public virtual interface (VIF) to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway.

Why it's wrong: AWS Direct Connect provides a dedicated network connection to AWS, which can increase transfer speed.  However, setting up Direct Connect is a complex and time-consuming process. It's an overkill for a one-time migration and doesn't meet the "least possible network bandwidth" requirement as efficiently as Snowball Edge.  Additionally, it includes the complexity of S3 File Gateway.    

Summary

The best solution is B. Create an AWS Snowball Edge job because it minimizes network bandwidth usage and provides a fast way to migrate a large amount of data to Amazon S3.

</details>

<details>
  <summary>Question 7</summary>

A company has an application that ingests incoming messages.  Dozens of other applications and microservices then quickly consume these messages.  The number of messages varies drastically and sometimes increases suddenly to 100,000 each second.  The company wants to decouple the solution and increase scalability.   

Which solution meets these requirements?

- [ ] A.  Persist the messages to Amazon Kinesis Data Analytics.  Configure the consumer applications to read and process the messages.    
- [ ] B.  Deploy the ingestion application on Amazon EC2 instances in an Auto Scaling group to scale the number of EC2 instances based on CPU metrics.    
- [ ] C.  Write the messages to Amazon Kinesis Data Streams with a single shard.  Use an AWS Lambda function to preprocess messages and store them in Amazon DynamoDB.  Configure the consumer applications to read from DynamoDB to process the messages.    
- [ ] D.  Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SOS) subscriptions.  Configure the consumer applications to process the messages from the queues. 

</details>

<details>
  <summary>Answer</summary>

- [ ] D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SQS) subscriptions. Configure the consumer applications to process the messages from the queues.

The correct answer is D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic with multiple Amazon Simple Queue Service (Amazon SQS) subscriptions. Configure the consumer applications to process the messages from the queues.
   
Why is this the correct answer?

- [ ] Decoupling: Amazon SNS and SQS together provide excellent decoupling.  The ingestion application publishes messages to an SNS topic, and the consumer applications subscribe to the topic via SQS queues.  This means the ingestion application doesn't need to know anything about the consumers, and consumers don't need to know about each other.   
- [ ] Scalability: SNS can handle a high volume of messages, and SQS queues can buffer messages for consumers, allowing each part of the system to scale independently.  If message volume spikes to 100,000 per second, SNS can handle it, and SQS queues will ensure that consumers can process them at their own pace.   
- [ ] Fan-out: SNS allows for "fan-out" delivery, meaning a single message can be delivered to multiple SQS queues.  This perfectly suits the requirement that "dozens" of applications and microservices consume the messages. Each consumer gets its own copy of the message via its SQS queue.   

Why are the other answers wrong?

A. Persist the messages to Amazon Kinesis Data Analytics. Configure the consumer applications to read and process the messages.

Why it's wrong: Kinesis Data Analytics is designed for real-time analytics on streaming data.  While it can handle high volumes, it's not the best fit for simple message distribution to many consumers.  It's more complex and expensive than SNS/SQS for this use case.   

B. Deploy the ingestion application on Amazon EC2 instances in an Auto Scaling group to scale the number of EC2 instances based on CPU metrics.

Why it's wrong: This only scales the ingestion of messages, not the consumption.  It doesn't decouple the system or address the scalability of the many consumer applications.   

C. Write the messages to Amazon Kinesis Data Streams with a single shard. Use an AWS Lambda function to preprocess messages and store them in Amazon DynamoDB. Configure the consumer applications to read from DynamoDB to process the messages.

Why it's wrong: Kinesis Data Streams is good for real-time data streaming, but using a single shard will limit throughput.  Lambda preprocessing adds complexity and potential bottlenecks.  Storing in DynamoDB adds latency and cost compared to SQS queues, which are designed for message queuing.  Also, reading from DynamoDB by many consumers can create hot spots and scaling issues.   

Summary

The best solution is D.
 
Amazon SNS and SQS provide the best combination of decoupling, scalability, and fan-out capability to meet the requirements of the scenario.

</details>

<details>
  <summary>Question 8</summary>

A company is migrating a distributed application to AWS. The application serves variable workloads. The legacy platform consists of a primary server that coordinates jobs across multiple compute nodes. The company wants to modernize the application with a solution that maximizes resiliency and scalability. How should a solutions architect design the architecture to meet these requirements?   

- [ ] A. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling to use scheduled scaling.   
- [ ] B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.
- [ ] C. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure AWS CloudTrail as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the primary server.   
- [ ] D. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure Amazon EventBridge (Amazon CloudWatch Events) as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the compute nodes. 

</details>

<details>
  <summary>Answer</summary>

- [ ] B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.

The correct answer is B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.

Why is this the correct answer?

- [ ] SQS for Decoupling and Resiliency: Using an SQS queue effectively decouples the primary server from the compute nodes. The primary server puts jobs in the queue, and the compute nodes take jobs from the queue. This makes the system more resilient because if a compute node fails, the job remains in the queue to be processed by another node.   
- [ ] Auto Scaling for Scalability: Managing compute nodes with an Auto Scaling group allows the number of nodes to adjust automatically based on demand, providing scalability.   
- [ ] Queue-Based Scaling: Configuring Auto Scaling based on the size of the SQS queue is crucial for handling variable workloads. When the queue is large, Auto Scaling adds more compute nodes to process the backlog. When the queue is small, Auto Scaling removes nodes to save costs. This ensures the application scales efficiently with the workload.   

Why are the other answers wrong?

A. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling to use scheduled scaling.

Why it's wrong: Scheduled scaling doesn't provide the dynamic responsiveness needed for variable workloads. Scheduled scaling works well for predictable load patterns, but this scenario involves unpredictable variations. Scaling based on queue size is more adaptive.   

C. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure AWS CloudTrail as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the primary server.

Why it's wrong: CloudTrail is for auditing API calls, not for queuing jobs. Scaling based on the primary server's load might not accurately reflect the work needed by the compute nodes. The queue size is a more direct indicator of the workload for those nodes. Also, it does not decouple the primary server from the compute nodes.   

D. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure Amazon EventBridge (Amazon CloudWatch Events) as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the compute nodes.

Why it's wrong: EventBridge is for event-driven architectures, not for queuing jobs in this manner. While scaling compute nodes based on their load is good, it doesn't address how jobs are passed between the primary server and the compute nodes. SQS provides the necessary decoupling and reliable queuing. Also, it does not decouple the primary server from the compute nodes.   

Summary

The best solution is B.  Using SQS for job queuing and Auto Scaling based on queue size provides the most resilient and scalable architecture for variable workloads.

</details>

<details>
  <summary>Question 9</summary>

A company is running an SMB file server in its data center.  The file server stores large files that are accessed frequently for the first few days after the files are created.  After 7 days the files are rarely accessed.   

The total data size is increasing and is close to the company's total storage capacity.  A solutions architect must increase the company's available storage space without losing low-latency access to the most recently accessed files.  The solutions architect must also provide file lifecycle management to avoid future storage issues.   

Which solution will meet these requirements?

- [ ] A.  Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS. 
- [ ] B.  Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days. 
- [ ] C.  Create an Amazon FSx for Windows File Server file system to extend the company's storage space. 
- [ ] D.  Install a utility on each user's computer to access Amazon S3. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days. 

</details>

<details>
  <summary>Answer</summary>

- [ ] B.  Create an Amazon S3 File Gateway to extend the company's storage space.  Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.  

The correct answer is B. Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.   

Why is this the correct answer?

- [ ] Amazon S3 File Gateway: This service allows you to seamlessly connect your on-premises file server to Amazon S3. It caches frequently accessed files on-premises, providing low-latency access, while transparently storing all your data in S3. This effectively extends the company's storage capacity in the cloud without requiring significant changes to how users access files.
- [ ] S3 Lifecycle Policy: This feature automates the movement of objects between different S3 storage classes. By creating a lifecycle policy to transition files to S3 Glacier Deep Archive after 7 days, you can automatically move infrequently accessed data to a lower-cost storage option. This addresses the requirement for file lifecycle management and avoids future storage issues by optimizing storage costs.

Why are the other answers wrong?

A. Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS.

Why it's wrong: AWS DataSync is designed for data migration and replication, not for providing a seamless extension of on-premises storage with local caching. While it can move data to AWS, it doesn't provide the low-latency access to recently accessed files that the File Gateway offers. It also doesn't integrate as directly with file access as a file gateway.

C. Create an Amazon FSx for Windows File Server file system to extend the company's storage space.

Why it's wrong: Amazon FSx for Windows File Server provides a fully managed native Microsoft Windows file system. While it's suitable for Windows-based workloads, it doesn't inherently provide the lifecycle management to archive older files to a lower-cost storage like S3 Glacier Deep Archive. It's also a separate file system, not an extension of the existing SMB file server with caching.

D. Install a utility on each user's computer to access Amazon S3. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days.   

Why it's wrong: This approach is disruptive and complex. It requires users to change their workflow to access files directly from S3, which is not a seamless extension of their existing SMB file server. It also introduces management overhead for installing and maintaining the utility on each user's computer. While S3 Lifecycle can move data to Glacier, it doesn't address the need for low-latency access to recently accessed files in the same way that File Gateway's caching does.

Summary

The best solution is B because it provides a seamless way to extend on-premises storage with caching for frequently accessed files (via S3 File Gateway) and automates the archival of older files to a cost-effective storage class (via S3 Lifecycle policy), meeting all the requirements of the scenario.

</details>

<details>
  <summary>Question 10</summary>

A company is building an ecommerce web application on AWS.  The application sends information about new orders to an Amazon API Gateway REST API to process.  The company wants to ensure that orders are processed in the order that they are received.    

Which solution will meet these requirements?

- [ ] A.  Use an API Gateway integration to publish a message to an Amazon Simple Notification Service (Amazon SNS) topic when the application receives an order.  Subscribe an AWS Lambda function to the topic to perform processing. 
- [ ] B.  Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order.  Configure the SQS FIFO queue to invoke an AWS Lambda function for processing. 
- [ ] C.  Use an API Gateway authorizer to block any requests while the application processes an order. 
- [ ] D.  Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) standard queue when the application receives an order.  Configure the SQS standard queue to invoke an AWS Lambda function for processing.  

</details>

<details>
  <summary>Answer</summary>

- [ ] B.  Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order.  Configure the SQS FIFO queue to invoke an AWS Lambda function for processing. 

The correct answer is B. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order. Configure the SQS FIFO queue to invoke an AWS Lambda function for processing.   

Why is this the correct answer?

- [ ] Amazon SQS FIFO (First-In-First-Out) queue: FIFO queues are designed to preserve the order in which messages are sent and received. This is crucial for the requirement of processing orders in the order they are received. When API Gateway sends order information to a FIFO queue, SQS guarantees that the Lambda function will process these orders in the correct sequence.
- [ ] API Gateway Integration with SQS: API Gateway can directly integrate with SQS to send messages. This allows the application to submit order data to the queue as soon as it's received.
- [ ] SQS Invokes Lambda: SQS can be configured to trigger a Lambda function automatically whenever new messages arrive in the queue. This creates an event-driven architecture, where the Lambda function processes orders asynchronously, ensuring that the API Gateway remains responsive.

Why are the other answers wrong?

A. Use an API Gateway integration to publish a message to an Amazon Simple Notification Service (Amazon SNS) topic when the application receives an order. Subscribe an AWS Lambda function to the topic to perform processing.   

Why it's wrong: Amazon SNS is a publish/subscribe messaging service. While it can trigger Lambda functions, it does not guarantee message ordering. In a publish/subscribe pattern, messages are delivered to all subscribers, and there is no inherent mechanism to ensure that messages are processed in the order they were published. This violates the requirement for ordered processing.

C. Use an API Gateway authorizer to block any requests while the application processes an order.

Why it's wrong: API Gateway authorizers are used for authentication and authorization, not for managing message processing order. Blocking requests while processing would severely impact the application's performance and availability. It would create a bottleneck and prevent the application from handling concurrent orders efficiently. This is not a scalable or practical solution.

D. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) standard queue when the application receives an order. Configure the SQS standard queue to invoke an AWS Lambda function for processing.   

Why it's wrong: Amazon SQS standard queues provide at-least-once delivery but do not guarantee message ordering. Messages might be delivered out of order, which would violate the requirement to process orders in the sequence they are received. While standard queues are suitable for many use cases, they are not appropriate when message ordering is critical.

Summary

The best solution is B because it uses Amazon SQS FIFO queues to ensure orders are processed in the order they are received, which is a key requirement. The other options do not provide guaranteed message ordering or introduce other problems like performance bottlenecks.

</details>

</details>

<details>
  <summary>==Questions 11-20==</summary>

<details>
  <summary>Question 11</summary>

A company has an application that runs on Amazon EC2 instances and uses an Amazon Aurora database.  The EC2 instances connect to the database by using user names and passwords that are stored locally in a file.  The company wants to minimize the operational overhead of credential management.    

What should a solutions architect do to accomplish this goal?

- [ ] A.  Use AWS Secrets Manager.  Turn on automatic rotation.
- [ ] B.  Use AWS Systems Manager Parameter Store.  Turn on automatic rotation.
- [ ] C.  Create an Amazon S3 bucket to store objects that are encrypted with an AWS Key Management Service (AWS KMS) encryption key.  Migrate the credential file to the S3 bucket.  Point the application to the S3 bucket. 
- [ ] D.  Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume for each EC2 instance.  Attach the new EBS volume to each EC2 instance.  Migrate the credential file to the new EBS volume.  Point the application to the new EBS volume.  

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Use AWS Secrets Manager.  Turn on automatic rotation.

The correct answer is A. Use AWS Secrets Manager. Turn on automatic rotation.

Why is this the correct answer?

- [ ] AWS Secrets Manager: This service is specifically designed to help you securely store, retrieve, and manage database credentials, API keys, and other secrets.  It offers features like automatic secret rotation, which greatly reduces the operational overhead of manually changing credentials.    
- [ ] Automatic Rotation: By turning on automatic rotation, Secrets Manager can automatically rotate the database credentials on a defined schedule.  This eliminates the need for manual intervention, minimizing operational overhead and improving security by reducing the risk of using long-lived credentials.    

Why are the other answers wrong?

B. Use AWS Systems Manager Parameter Store. Turn on automatic rotation.

Why it's wrong: While AWS Systems Manager Parameter Store can securely store secrets, it is not primarily designed for automatic secret rotation of database credentials in the same way that Secrets Manager is.  Implementing automatic rotation with Parameter Store would require additional custom configuration and management, increasing operational overhead.    
C. Create an Amazon S3 bucket to store objects that are encrypted with an AWS Key Management Service (AWS KMS) encryption key. Migrate the credential file to the S3 bucket. Point the application to the S3 bucket.

Why it's wrong: Storing credentials in an S3 bucket, even if encrypted, is not a secure or operationally efficient way to manage them.  It introduces complexities in access control, requires managing S3 permissions, and does not provide built-in secret rotation.  Additionally, the application would need to be modified to retrieve credentials from S3, adding overhead.    
D. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume for each EC2 instance. Attach the new EBS volume to each EC2 instance. Migrate the credential file to the new EBS volume. Point the application to the new EBS volume.

Why it's wrong: Storing credentials on EBS volumes is not a recommended security practice.  It increases management overhead as each volume needs to be managed and secured. It also doesn't provide any built-in mechanism for secret rotation.  If an EC2 instance is compromised, the credentials on the EBS volume could be exposed.    
Summary

The best solution is A because AWS Secrets Manager is the most suitable service for managing database credentials with the least operational overhead, thanks to its automatic rotation feature. The other options introduce more complexity, security risks, or lack the automated rotation capabilities needed for efficient credential management.

</details>

<details>
  <summary>Question 12</summary>

A global company hosts its web application on Amazon EC2 instances behind an Application Load Balancer (ALB).  The web application has static data and dynamic data.  The company stores its static data in an Amazon S3 bucket.  The company wants to improve performance and reduce latency for the static data and dynamic data.  The company is using its own domain name registered with Amazon Route 53.    

What should a solutions architect do to meet these requirements?

- [ ] A.  Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins.  Configure Route 53 to route traffic to the CloudFront distribution. 
- [ ] B.  Create an Amazon CloudFront distribution that has the ALB as an origin.  Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint Configure Route 53 to route traffic to the CloudFront distribution. 
- [ ] C.  Create an Amazon CloudFront distribution that has the S3 bucket as an origin.  Create an AWS Global Accelerator standard accelerator that has the ALB and the CloudFront distribution as endpoints.  Create a custom domain name that points to the accelerator DNS name.  Use the custom domain name as an endpoint for the web application. 
- [ ] D.  Create an Amazon CloudFront distribution that has the ALB as an origin.  Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint.  Create two domain names.  Point one domain name to the CloudFront DNS name for dynamic content.  Point the other domain name to the accelerator DNS name for static content.  Use the domain names as endpoints for the web application. 

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins.  Configure Route 53 to route traffic to the CloudFront distribution. 

The correct answer is A. Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.    

Why is this the correct answer?

- [ ] Amazon CloudFront is a content delivery network (CDN) service that speeds up the distribution of your web content to users.
- [ ] By using CloudFront with both the S3 bucket (for static content) and the Application Load Balancer (for dynamic content) as origins, you can effectively cache and deliver both types of content efficiently.
- [ ] Route 53 is a highly available and scalable Domain Name System (DNS) web service.
- [ ] Configuring Route 53 to route traffic to the CloudFront distribution ensures that user requests are directed to the nearest CloudFront edge location, further reducing latency and improving performance.    

Why are the other answers wrong?

B. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint Configure Route 53 to route traffic to the CloudFront distribution.

Why it's wrong: While CloudFront is suitable for caching, AWS Global Accelerator is designed to improve the performance of dynamic traffic over the internet. Using Global Accelerator for the S3 bucket (static content) is not the most efficient use of the service. CloudFront can handle static content delivery effectively. Also, this option introduces unnecessary complexity.    

C. Create an Amazon CloudFront distribution that has the S3 bucket as an origin. Create an AWS Global Accelerator standard accelerator that has the ALB and the CloudFront distribution as endpoints. Create a custom domain name that points to the accelerator DNS name. Use the custom domain name as an endpoint for the web application.

Why it's wrong: This option overcomplicates the architecture. Using Global Accelerator with CloudFront as an endpoint is not a typical or necessary pattern for this use case. CloudFront can handle both static and dynamic content efficiently when configured with both S3 and ALB as origins.    

D. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint. Create two domain names. Point one domain name to the CloudFront DNS name for dynamic content. Point the other domain name to the accelerator DNS name for static content. Use the domain names as endpoints for the web application.

Why it's wrong: This option suggests using two separate domain names, which adds complexity to the application and is not necessary. CloudFront can serve both static and dynamic content under a single domain name when configured correctly with multiple origins. Using Global Accelerator for S3 is not the most cost-effective or efficient approach.    

Summary

The best solution is A because it leverages CloudFront to efficiently deliver both static and dynamic content and uses Route 53 to optimize routing. This approach provides the required performance and latency improvements in a straightforward and effective manner.

</details>

<details>
  <summary>Question 13</summary>
 
A company performs monthly maintenance on its AWS infrastructure.  During these maintenance activities, the company needs to rotate the credentials for its Amazon RDS for MySQL databases across multiple AWS Regions.    

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A.  Store the credentials as secrets in AWS Secrets Manager.  Use multi-Region secret replication for the required Regions.  Configure Secrets Manager to rotate the secrets on a schedule.    
- [ ] B.  Store the credentials as secrets in AWS Systems Manager by creating a secure string parameter.  Use multi-Region secret replication for the required Regions.  Configure Systems Manager to rotate the secrets on a schedule.    
- [ ] C.  Store the credentials in an Amazon S3 bucket that has server-side encryption (SSE) enabled.  Use Amazon EventBridge (Amazon CloudWatch Events) to invoke an AWS Lambda function to rotate the credentials.    
- [ ] D.  Encrypt the credentials as secrets by using AWS Key Management Service (AWS KMS) multi-Region customer managed keys.  Store the secrets in an Amazon DynamoDB global table.  Use an AWS Lambda function to retrieve the secrets from DynamoDB.  Use the RDS API to rotate the secrets.

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Store the credentials as secrets in AWS Secrets Manager.  Use multi-Region secret replication for the required Regions.  Configure Secrets Manager to rotate the secrets on a schedule.    

The correct answer is A. Store the credentials as secrets in AWS Secrets Manager. Use multi-Region secret replication for the required Regions. Configure Secrets Manager to rotate the secrets on a schedule.   

Why is this the correct answer?

- [ ] AWS Secrets Manager: This service is specifically designed for managing database credentials, API keys, and other secrets. It simplifies the process of storing, retrieving, and rotating sensitive information.
- [ ] Multi-Region Secret Replication: Secrets Manager allows you to replicate secrets across multiple AWS Regions. This ensures that the credentials are available in all the necessary Regions for the RDS for MySQL databases.
- [ ] Scheduled Rotation: Secrets Manager has built-in automatic rotation capabilities. You can configure it to rotate the RDS database credentials on a schedule (e.g., monthly), which aligns with the company's maintenance activities. This automation minimizes operational overhead.

Why are the other answers wrong?

B. Store the credentials as secrets in AWS Systems Manager by creating a secure string parameter. Use multi-Region secret replication for the required Regions. Configure Systems Manager to rotate the secrets on a schedule.   

Why it's wrong: While AWS Systems Manager Parameter Store can securely store secrets, it's not as specialized for database credential rotation as Secrets Manager. Implementing automatic rotation with Parameter Store requires more manual setup and management with Lambda functions, increasing operational overhead.

C. Store the credentials in an Amazon S3 bucket that has server-side encryption (SSE) enabled. Use Amazon EventBridge (Amazon CloudWatch Events) to invoke an AWS Lambda function to rotate the credentials.   

Why it's wrong: Storing credentials directly in S3, even with encryption, is not a recommended security best practice. It requires managing access control to the bucket and implementing custom rotation logic using EventBridge and Lambda. This approach is more complex and has higher operational overhead compared to using Secrets Manager.

D. Encrypt the credentials as secrets by using AWS Key Management Service (AWS KMS) multi-Region customer managed keys. Store the secrets in an Amazon DynamoDB global table. Use an AWS Lambda function to retrieve the secrets from DynamoDB. Use the RDS API to rotate the secrets.   

Why it's wrong: This solution is overly complex. Using DynamoDB to store credentials introduces unnecessary overhead, and managing the entire rotation process with Lambda and the RDS API requires significant custom coding and maintenance. Secrets Manager provides a much simpler and more managed approach.

Summary

The best solution is A because AWS Secrets Manager provides a centralized and managed way to store, replicate, and automatically rotate database credentials across multiple Regions, minimizing operational overhead.

</details>

<details>
  <summary>Question 14</summary>

A company runs an ecommerce application on Amazon EC2 instances behind an Application Load Balancer.  The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones.  The Auto Scaling group scales based on CPU utilization metrics.  The ecommerce application stores the transaction data in a MySQL 8.0 database that is hosted on a large EC2 instance.  The database's performance degrades quickly as application load increases.  The application handles more read requests than write transactions.  The company wants a solution that will automatically scale the database to meet the demand of unpredictable read workloads while maintaining high availability.  Which solution will meet these requirements?   

- [ ] A.  Use Amazon Redshift with a single node for leader and compute functionality.    
- [ ] B.  Use Amazon RDS with a Single-AZ deployment Configure Amazon RDS to add reader instances in a different Availability Zone.    
- [ ] C.  Use Amazon Aurora with a Multi-AZ deployment.  Configure Aurora Auto Scaling with Aurora Replicas.    
- [ ] D.  Use Amazon ElastiCache for Memcached with EC2 Spot Instances. 

</details>

<details>
  <summary>Answer</summary>

- [ ] C.  Use Amazon Aurora with a Multi-AZ deployment.  Configure Aurora Auto Scaling with Aurora Replicas.
  
The correct answer is C. Use Amazon Aurora with a Multi-AZ deployment. Configure Aurora Auto Scaling with Aurora Replicas.

Why is this the correct answer?

- [ ] Amazon Aurora: Aurora is a MySQL and PostgreSQL-compatible relational database service built for the cloud. It is designed to offer high performance, scalability, and availability.
- [ ] Multi-AZ Deployment: Deploying Aurora in a Multi-AZ configuration provides high availability by synchronously replicating data to a standby instance in a different Availability Zone. This ensures that the database remains available even if one AZ experiences an outage.   
- [ ] Aurora Auto Scaling with Aurora Replicas: Aurora Auto Scaling allows you to automatically add or remove Aurora Replicas based on the database workload. Since the application handles more read requests, adding replicas will distribute the read load and improve performance.
- [ ] Auto Scaling ensures that this scaling happens automatically in response to demand, meeting the requirement for scaling to unpredictable read workloads.

Why are the other answers wrong?

A. Use Amazon Redshift with a single node for leader and compute functionality.

Why it's wrong: Amazon Redshift is a data warehousing service designed for analytics and business intelligence. It is not suitable for online transaction processing (OLTP) workloads like an ecommerce application's transaction data. Also, using a single node does not provide high availability.

B. Use Amazon RDS with a Single-AZ deployment Configure Amazon RDS to add reader instances in a different Availability Zone.   

Why it's wrong: While Amazon RDS can provide read replicas for scaling reads, a Single-AZ deployment does not offer high availability. If the single AZ fails, the database will be unavailable. Aurora's Multi-AZ deployment provides a more robust high availability solution.

D. Use Amazon ElastiCache for Memcached with EC2 Spot Instances.

Why it's wrong: Amazon ElastiCache is an in-memory caching service. It is used to improve application performance by caching frequently accessed data, but it is not a replacement for a relational database like MySQL for storing transaction data. Also, using EC2 Spot Instances can introduce availability risks, as Spot Instances can be interrupted.

Summary

The best solution is C because Amazon Aurora with Multi-AZ deployment provides both the high availability and the ability to automatically scale read capacity with Aurora Replicas, which directly addresses the application's requirements.

</details>

<details>
  <summary>Question 15</summary>

A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the production VPC.  The company had an inspection server in its on-premises data center.  The inspection server performed specific operations such as traffic flow inspection and traffic filtering.  The company wants to have the same functionalities in the AWS Cloud.    

Which solution will meet these requirements?

- [ ] A.  Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.    
- [ ] B.  Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.    
- [ ] C.  Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.    
- [ ] D.  Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC. 

</details>

<details>
  <summary>Answer</summary>

- [ ] C.  Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.  
          
The correct answer is C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.   

Why is this the correct answer?

- [ ] AWS Network Firewall: This service provides stateful network firewalling for your VPCs. It allows you to filter traffic at the network layer (Layer 3 and 4) as well as the application layer (Layer 7).
- [ ] You can create fine-grained rules for traffic inspection and filtering, giving you the control needed to protect your VPC. It's designed to provide the functionalities of a traditional inspection server in the cloud.

Why are the other answers wrong?

A. Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.

Why it's wrong: Amazon GuardDuty is a threat detection service that monitors for malicious activity and delivers security alerts. It analyzes VPC Flow Logs, DNS Logs, and CloudTrail Logs. While it performs "inspection" in the sense of detecting threats, it does not provide the capability to create custom rules to actively filter or block traffic in the way that a traditional firewall does.

B. Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.

Why it's wrong: Traffic Mirroring allows you to copy network traffic from EC2 instances. However, it only copies the traffic; it doesn't provide the firewall functionality to inspect and filter traffic in real-time. You would need to send the mirrored traffic to separate inspection appliances, adding complexity and cost.

D. Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC.   

Why it's wrong: AWS Firewall Manager is a security management service that allows you to centrally manage firewall rules across multiple AWS accounts and VPCs. It works in conjunction with AWS Network Firewall and AWS WAF. You still need AWS Network Firewall to define the actual inspection and filtering rules; Firewall Manager is for centralized management of those rules.

Summary

The best solution is C because AWS Network Firewall is the service that most closely replicates the functionality of an on-premises inspection server by providing stateful firewall capabilities with customizable rules for traffic inspection and filtering directly within the VPC.
</details>

<details>
  <summary>Question 16</summary>

A company hosts a data lake on AWS.  The data lake consists of data in Amazon S3 and Amazon RDS for PostgreSQL.  The company needs a reporting solution that provides data visualization and includes all the data sources within the data lake.  Only the company's management team should have full access to all the visualizations.  The rest of the company should have only limited access.   

Which solution will meet these requirements?

- [ ] A.  Create an analysis in Amazon QuickSight.  Connect all the data sources and create new datasets.  Publish dashboards to visualize the data.  Share the dashboards with the appropriate IAM roles.   
- [ ] B.  Create an analysis in Amazon QuickSight.  Connect all the data sources and create new datasets.  Publish dashboards to visualize the data.  Share the dashboards with the appropriate users and groups.   
- [ ] C.  Create an AWS Glue table and crawler for the data in Amazon S3.  Create an AWS Glue extract, transform, and load (ETL) job to produce reports.  Publish the reports to Amazon S3.  Use S3 bucket policies to limit access to the reports.   
- [ ] D.  Create an AWS Glue table and crawler for the data in Amazon S3.  Use Amazon Athena Federated Query to access data within Amazon RDS for PostgreSQL.  Generate reports by using Amazon Athena.  Publish the reports to Amazon S3.  Use S3 bucket policies to limit access to the reports. 

</details>

<details>
  <summary>Answer</summary>

- [ ] B.  Create an analysis in Amazon QuickSight.  Connect all the data sources and create new datasets.  Publish dashboards to visualize the data.  Share the dashboards with the appropriate users and groups.    

The correct answer is B. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate users and groups.    

Why is this the correct answer?

- [ ] Amazon QuickSight is a cloud-native, serverless business intelligence service that makes it easy to connect to various data sources, create interactive dashboards, and share them with users within an organization.    
- [ ] By connecting QuickSight to both Amazon S3 and Amazon RDS for PostgreSQL, you can visualize data from both parts of the data lake in one place.
- [ ] QuickSight allows you to share dashboards with specific users and groups, providing granular control over access to the visualizations.  This directly addresses the requirement to give the management team full access and the rest of the company limited access.   

Why are the other answers wrong?

A. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate IAM roles.

Why it's wrong: While QuickSight can integrate with AWS Identity and Access Management (IAM), sharing dashboards directly with IAM roles is not the typical or most effective way to manage user access within QuickSight. QuickSight is designed to manage sharing through users and groups within the QuickSight service itself, which provides more fine-grained control for this use case.

C. Create an AWS Glue table and crawler for the data in Amazon S3. Create an AWS Glue extract, transform, and load (ETL) job to produce reports. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.

Why it's wrong: This solution focuses on using AWS Glue for data processing and S3 for storing reports. While Glue is excellent for ETL, it doesn't provide the data visualization capabilities needed for the reporting solution. Relying solely on S3 bucket policies for access control is less flexible and user-friendly than QuickSight's built-in sharing features.

D. Create an AWS Glue table and crawler for the data in Amazon S3. Use Amazon Athena Federated Query to access data within Amazon RDS for PostgreSQL. Generate reports by using Amazon Athena. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.

Why it's wrong: Similar to option C, this solution uses AWS Glue and Amazon Athena, which are powerful for data querying and analysis, but it lacks the visualization component. Athena is a query service, not a reporting tool. Again, using only S3 bucket policies for access control is less ideal than QuickSight's sharing features.
Summary

The best solution is B because Amazon QuickSight provides the necessary data visualization capabilities and granular control over sharing dashboards with users and groups, fulfilling all the requirements of the scenario.

</details>

<details>
  <summary>Question 17</summary>

A company is implementing a new business application.  The application runs on two Amazon EC2 instances and uses an Amazon S3 bucket for document storage.  A solutions architect needs to ensure that the EC2 instances can access the S3 bucket.    

What should the solutions architect do to meet this requirement?

- [ ] A.  Create an IAM role that grants access to the S3 bucket.  Attach the role to the EC2 instances.    
- [ ] B.  Create an IAM policy that grants access to the S3 bucket.  Attach the policy to the EC2 instances.    
- [ ] C.  Create an IAM group that grants access to the S3 bucket.  Attach the group to the EC2 instances.    
- [ ] D.  Create an IAM user that grants access to the S3 bucket.  Attach the user account to the EC2 instances.

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Create an IAM role that grants access to the S3 bucket.  Attach the role to the EC2 instances.    

The correct answer is A. Create an IAM role that grants access to the S3 bucket. Attach the role to the EC2 instances.

Why is this the correct answer?

- [ ] IAM roles are the recommended way to grant permissions to AWS services like EC2 to access other AWS services like S3.    
- [ ] When you attach an IAM role to an EC2 instance, the applications running on that instance can obtain temporary AWS credentials and use them to access the specified resources.    
- [ ] This method is secure and manageable, as you don't need to manage long-term credentials directly on the EC2 instances.    

Why are the other answers wrong?

B. Create an IAM policy that grants access to the S3 bucket. Attach the policy to the EC2 instances.

Why it's wrong: IAM policies define permissions, but they need to be attached to IAM identities (users, groups, or roles). You can't directly attach a policy to an EC2 instance. Roles are the correct way to grant permissions to EC2 instances.

C. Create an IAM group that grants access to the S3 bucket. Attach the group to the EC2 instances.

Why it's wrong: IAM groups are used to manage permissions for IAM users, not AWS resources like EC2 instances. You can't attach a group to an EC2 instance.

D. Create an IAM user that grants access to the S3 bucket. Attach the user account to the EC2 instances.

Why it's wrong: Creating IAM users and embedding their credentials (access keys and secret access keys) directly on EC2 instances is a security risk. It's harder to manage and rotate credentials, and if an instance is compromised, the credentials could be exposed. Roles provide a more secure and manageable way to grant permissions to EC2 instances.

Summary

The best solution is A because IAM roles are designed for granting permissions to AWS services, providing a secure and manageable way for EC2 instances to access S3 buckets without managing credentials directly on the instances.

</details>

<details>
  <summary>Question 18</summary>

An application development team is designing a microservice that will convert large images to smaller, compressed images.  When a user uploads an image through the web interface, the microservice should store the image in an Amazon S3 bucket, process and compress the image with an AWS Lambda function, and store the image in its compressed form in a different S3 bucket.  A solutions architect needs to design a solution that uses durable, stateless components to process the images automatically.    

Which combination of actions will meet these requirements? (Choose two.)

- [ ] A.  Create an Amazon Simple Queue Service (Amazon SQS) queue.  Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.  
- [ ] B.  Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source.  When the SQS message is successfully processed, delete the message in the queue.  
- [ ] C.  Configure the Lambda function to monitor the S3 bucket for new uploads.  When an uploaded image is detected, write the file name to a text file in memory and use the text file to keep track of the images that were processed.    
- [ ] D.  Launch an Amazon EC2 instance to monitor an Amazon Simple Queue Service (Amazon SQS) queue.  When items are added to the queue, log the file name in a text file on the EC2 instance and invoke the Lambda function.    
- [ ] E.  Configure an Amazon EventBridge (Amazon CloudWatch Events) event to monitor the S3 bucket.  When an image is uploaded, send an alert to an Amazon Simple Notification Service (Amazon SNS) topic with the application owner's email address for further processing. 

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Create an Amazon Simple Queue Service (Amazon SQS) queue.  Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.  
- [ ] B.  Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source.  When the SQS message is successfully processed, delete the message in the queue.

The answer is A and B.

Here's why:

A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.    

- [ ] This is a good first step because it creates a durable and decoupled way to handle the image processing requests.
- [ ] S3 event notifications allow the bucket to trigger events when new objects are created (images uploaded), and sending these notifications to an SQS queue ensures that the processing requests are reliably queued for processing, even if the system experiences temporary issues.

B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.    

- [ ] This connects the processing logic (Lambda function) to the queue.
- [ ] Lambda can be triggered by new messages in the SQS queue, and deleting the message after successful processing ensures that each image is processed exactly once. This is important for durability and prevents reprocessing.

Why other options are not the best fit:

C. While Lambda can be triggered by S3 events, relying on Lambda to "monitor" the S3 bucket and maintain a text file in memory for tracking introduces state management within the Lambda function, which is generally bad practice. Lambda functions should be stateless. It also doesn't provide the same level of durability as SQS.

D. Using an EC2 instance to monitor an SQS queue adds unnecessary complexity and cost. Lambda functions are designed for this type of event-driven processing and are more cost-effective and scalable.

E. While EventBridge or SNS could be used to trigger actions based on S3 events, they don't provide the queueing and guaranteed delivery that SQS does. SNS is more for fan-out notifications, and EventBridge is an event bus. SQS is designed for reliable message queuing. Sending an email to the application owner is not part of the automated processing requirement.

In summary

A and B together create a durable, scalable, and stateless solution: S3 triggers SQS, and Lambda processes items from SQS. 

</details>

<details>
  <summary>Question 19</summary>

A company has a three-tier web application that is deployed on AWS.  The web servers are deployed in a public subnet in a VPC.  The application servers and database servers are deployed in private subnets in the same VPC.  The company has deployed a third-party virtual firewall appliance from AWS Marketplace in an inspection VPC.  The appliance is configured with an IP interface that can accept IP packets.  A solutions architect needs to integrate the web application with the appliance to inspect all traffic to the application before the traffic reaches the web server.    
Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A.  Create a Network Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.    
- [ ] B.  Create an Application Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.    
- [ ] C.  Deploy a transit gateway in the inspection VPConfigure route tables to route the incoming packets through the transit gateway.    
- [ ] D.  Deploy a Gateway Load Balancer in the inspection VPC.  Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance. 

</details>

<details>
  <summary>Answer</summary>

- [ ] D.  Deploy a Gateway Load Balancer in the inspection VPC.  Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.
 
The answer is D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.   

Here's a breakdown of why:

Why is this the correct answer?

- [ ] Gateway Load Balancer (GWLB): This load balancer is specifically designed to distribute traffic to virtual appliances like firewalls. It operates at the network layer (Layer 3) and uses the GENEVE protocol to encapsulate and forward traffic to the appliance.
- [ ] GWLB Endpoint: This endpoint allows you to transparently insert the GWLB into the traffic flow. You configure your VPC route tables to send traffic to the GWLB endpoint, and the GWLB then distributes that traffic to your firewall appliance.
- [ ] This solution minimizes operational overhead by providing a scalable and highly available way to direct network traffic to the inspection appliance without requiring you to manage individual connections or instances.

Why are the other answers wrong?

A. Create a Network Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.   

Why it's wrong: While NLB works at the network layer, it's not designed for transparently inserting appliances into the traffic path. It's more suited for load balancing traffic to applications. Using an NLB for this purpose would require more complex configuration and might not handle the traffic inspection use case as efficiently as a GWLB.

B. Create an Application Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.   

Why it's wrong: ALB operates at the application layer (Layer 7) and is designed for HTTP/HTTPS traffic. It's not suitable for inspecting all types of network traffic. Using an ALB for this purpose would introduce unnecessary overhead and limitations.

C. Deploy a transit gateway in the inspection VPConfigure route tables to route the incoming packets through the transit gateway.   

Why it's wrong: While a Transit Gateway can connect VPCs, it doesn't inherently provide the service insertion capability needed for this use case. You would still need a mechanism to direct traffic from the Transit Gateway to the appliance, which would likely involve complex routing and Network Address Translation (NAT) configurations. GWLB simplifies this process.

Summary

The best solution is D because Gateway Load Balancer is the AWS service specifically built to simplify the deployment and scaling of virtual appliances for network traffic inspection, minimizing operational overhead.

</details>

<details>
  <summary>Question 20</summary>

A company wants to improve its ability to clone large amounts of production data into a test environment in the same AWS Region.  The data is stored in Amazon EC2 instances on Amazon Elastic Block Store (Amazon EBS) volumes.  Modifications to the cloned data must not affect the production environment.  The software that accesses this data requires consistently high I/O performance.  A solutions architect needs to minimize the time that is required to clone the production data into the test environment.    

Which solution will meet these requirements?

- [ ] A.  Take EBS snapshots of the production EBS volumes.  Restore the snapshots onto EC2 instance store volumes in the test environment.    
- [ ] B.  Configure the production EBS volumes to use the EBS Multi-Attach feature.  Take EBS snapshots of the production EBS volumes.  Attach the production EBS volumes to the EC2 instances in the test environment.    
- [ ] C.  Take EBS snapshots of the production EBS volumes.  Create and initialize new EBS volumes.  Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.    
- [ ] D.  Take EBS snapshots of the production EBS volumes.  Turn on the EBS fast snapshot restore feature on the EBS snapshots.  Restore the snapshots into new EBS volumes.  Attach the new EBS volumes to EC2 instances in the test environment.

</details>

<details>
  <summary>Answer</summary>

- [ ] D.  Take EBS snapshots of the production EBS volumes.  Turn on the EBS fast snapshot restore feature on the EBS snapshots.  Restore the snapshots into new EBS volumes.  Attach the new EBS volumes to EC2 instances in the test environment.  

The answer is D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.   

Here's a detailed explanation:

Why is this the correct answer?

- [ ] EBS Snapshots: Snapshots are the standard way to back up EBS volumes. They capture a point-in-time copy of your data.
- [ ] EBS Fast Snapshot Restore: This feature is crucial for minimizing the time required to clone data. By enabling Fast Snapshot Restore, you pre-initialize the EBS volumes created from the snapshot, significantly reducing the latency when they are first attached to an EC2 instance. This is essential for applications that require consistently high I/O performance immediately.
- [ ] New EBS Volumes: Creating new volumes from the snapshots ensures that the test environment has its own independent copy of the data, and modifications in the test environment will not affect the production environment.

Why are the other answers wrong?

A. Take EBS snapshots of the production EBS volumes. Restore the snapshots onto EC2 instance store volumes in the test environment.   

Why it's wrong: EC2 instance store volumes are ephemeral, meaning their data is lost when the instance is stopped or terminated. This is not suitable for a test environment where data needs to persist. Additionally, instance store volumes generally do not provide the same level of consistent I/O performance as EBS volumes.

B. Configure the production EBS volumes to use the EBS Multi-Attach feature. Take EBS snapshots of the production EBS volumes. Attach the production EBS volumes to the EC2 instances in the test environment.   

Why it's wrong: EBS Multi-Attach allows attaching a single volume to multiple EC2 instances concurrently, but it's designed for specific use cases (like clustered applications) and is not appropriate for cloning production data for testing. Attaching the production volumes to the test environment directly poses a significant risk of accidentally corrupting or modifying production data.

C. Take EBS snapshots of the production EBS volumes. Create and initialize new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.   

Why it's wrong: While this approach creates independent volumes, it misses the crucial step of using Fast Snapshot Restore. Initializing EBS volumes from snapshots can take a significant amount of time, especially for large volumes, as the data is lazily loaded from S3. This would not meet the requirement to minimize the time needed to clone the data.

Summary

The best solution is D because it combines the use of EBS snapshots for data cloning with the EBS Fast Snapshot Restore feature to minimize the time required to make the cloned data available with high I/O performance. This solution effectively balances data isolation and performance.

</details>

</details>

<details>
  <summary>==Questions 21-30==</summary>

<details>
  <summary>Question 21</summary>

An ecommerce company wants to launch a one-deal-a-day website on AWS. Each day will feature exactly one product on sale for a period of 24 hours. The company wants to be able to handle millions of requests each hour with millisecond latency during peak hours.    

Which solution will meet these requirements with the LEAST operational overhead?

- [ ] A.  Use Amazon S3 to host the full website in different S3 buckets.  Add Amazon CloudFront distributions.  Set the S3 buckets as origins for the distributions.  Store the order data in Amazon S3.    
- [ ] B.  Deploy the full website on Amazon EC2 instances that run in Auto Scaling groups across multiple Availability Zones.  Add an Application Load Balancer (ALB) to distribute the website traffic.  Add another ALB for the backend APIs.  Store the data in Amazon RDS for MySQL.    
- [ ] C.  Migrate the full application to run in containers.  Host the containers on Amazon Elastic Kubernetes Service (Amazon EKS).  Use the Kubernetes Cluster Autoscaler to increase and decrease the number of pods to process bursts in traffic.  Store the data in Amazon RDS for MySQL.    
- [ ] D.  Use an Amazon S3 bucket to host the website's static content.  Deploy an Amazon CloudFront distribution.  Set the S3 bucket as the origin.  Use Amazon API Gateway and AWS Lambda functions for the backend APIs.  Store the data in Amazon DynamoDB.   

</details>

<details>
  <summary>Answer</summary>

- [ ] D.  Use an Amazon S3 bucket to host the website's static content.  Deploy an Amazon CloudFront distribution.  Set the S3 bucket as the origin.  Use Amazon API Gateway and AWS Lambda functions for the backend APIs.  Store the data in Amazon DynamoDB.   

The answer is D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.    

Here's a breakdown of why this is the best solution for a high-traffic, low-latency, one-deal-a-day website:

Why this solution works:

- [ ] Amazon S3 for Static Content: S3 is excellent for storing static assets (images, CSS, JavaScript) due to its scalability, availability, and cost-effectiveness.    
- [ ] Amazon CloudFront: CloudFront is a Content Delivery Network (CDN) that caches static content and distributes it globally, reducing latency and improving load times for users. This is crucial for handling millions of requests with millisecond latency.    
- [ ] Amazon API Gateway and AWS Lambda: API Gateway provides a scalable and serverless way to create and manage APIs. Lambda allows you to run backend code without provisioning or managing servers. This combination is ideal for handling the dynamic parts of the website (e.g., retrieving product information, processing orders) with high scalability.    
- [ ] Amazon DynamoDB: DynamoDB is a NoSQL database service that provides fast and predictable performance with seamless scalability. It's well-suited for storing data that needs to be accessed with low latency, such as product information and order details.    
- [ ] Least Operational Overhead: This architecture leverages managed services (S3, CloudFront, API Gateway, Lambda, DynamoDB), which reduces the operational overhead of managing servers, scaling infrastructure, and ensuring availability.    

Why the other options are not as suitable:

A. While S3 and CloudFront are good for static content, hosting the entire website (including the dynamic parts) in S3 would be complex and inefficient. You would still need a backend to handle the dynamic logic, and S3 is not designed for that.
B. Deploying the website on EC2 instances with Auto Scaling and ALBs can handle traffic, but it involves more operational overhead than the serverless approach in Option D. You are responsible for managing the EC2 instances, scaling them, and ensuring their availability. RDS for MySQL might also become a bottleneck under extreme load.
C. EKS is a powerful container orchestration platform, but it adds significant operational complexity. Managing a Kubernetes cluster requires more effort than using managed services like Lambda and DynamoDB. While it can scale, it's more overhead than necessary for this use case. RDS for MySQL might also become a bottleneck under extreme load.

In summary

Option D provides the best balance of scalability, performance, and minimal operational overhead for a high-traffic, one-deal-a-day website.

</details>

<details>
  <summary>Question 22</summary>

A solutions architect is using Amazon S3 to design the storage architecture of a new digital media application.  The media files must be resilient to the loss of an Availability Zone.  Some files are accessed frequently while other files are rarely accessed in an unpredictable pattern.  The solutions architect must minimize the costs of storing and retrieving the media files.   

Which storage option meets these requirements?

- [ ] A. S3 Standard
- [ ] B. S3 Intelligent-Tiering
- [ ] C. S3 Standard-Infrequent Access (S3 Standard-IA)
- [ ] D. S3 One Zone-Infrequent Access (S3 One Zone-IA)   

</details>

<details>
  <summary>Answer</summary>

- [ ] B. S3 Intelligent-Tiering

The answer is indeed B. S3 Intelligent-Tiering.   

Here's why:

- [ ] S3 Intelligent-Tiering: This storage class is designed to optimize costs by automatically moving data between frequently accessed (hot) and infrequently accessed (cool) tiers based on changing access patterns.   
- [ ] It addresses the requirement of unpredictable access patterns by ensuring that frequently accessed files are readily available at low latency, while automatically moving less frequently accessed files to a lower-cost tier.   
- [ ] It also satisfies the requirement for resilience to the loss of an Availability Zone because it stores data across multiple Availability Zones.

Let's look at why the other options are not the best fit:

A. S3 Standard: While S3 Standard offers high availability and durability, it is the most expensive option for storing data. If some files are rarely accessed, it would not be cost-effective.   

C. S3 Standard-Infrequent Access (S3 Standard-IA): S3 Standard-IA is suitable for data that is less frequently accessed, but it does not automatically adapt to changing access patterns like S3 Intelligent-Tiering. If access patterns are unpredictable, S3 Intelligent-Tiering is more efficient.   

D. S3 One Zone-Infrequent Access (S3 One Zone-IA): S3 One Zone-IA is a lower-cost option, but it stores data in only one Availability Zone, making it less resilient. It does not meet the requirement for resilience to the loss of an Availability Zone.   

In summary, S3 Intelligent-Tiering provides the best balance of cost-effectiveness, performance, and resilience for this scenario. 

</details>

<details>
  <summary>Question 23</summary>

A company is storing backup files by using Amazon S3 Standard storage.  The files are accessed frequently for 1 month.  However, the files are not accessed after 1 month.  The company must keep the files indefinitely.   

Which storage solution will meet these requirements MOST cost-effectively?

- [ ] A.  Configure S3 Intelligent-Tiering to automatically migrate objects.
- [ ] B.  Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.
- [ ] C.  Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month. 
- [ ] D.  Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month.

</details>

<details>
  <summary>Answer</summary>

- [ ] B.  Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.

The answer is B. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.    

Here's why:

- [ ] S3 Lifecycle Policies: These policies automate the movement of objects between different S3 storage classes.    
- [ ] S3 Glacier Deep Archive: This is the lowest-cost storage class in S3, designed for long-term archiving.  It's suitable for data that is accessed very rarely but needs to be retained indefinitely.    
- [ ] By setting up a lifecycle policy to move the files to Glacier Deep Archive after 1 month, you meet the requirements of cost-effectiveness (for long-term storage) and indefinite retention.    

Let's analyze why the other options are not ideal:

A. Configure S3 Intelligent-Tiering to automatically migrate objects: While Intelligent-Tiering optimizes costs based on access patterns, it's designed for data with unpredictable access. In this case, the access pattern is very predictable (frequent for 1 month, then never). Glacier Deep Archive will be more cost-effective for long-term storage of data that is known to be rarely accessed.

C. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month: S3 Standard-IA is cheaper than S3 Standard but more expensive than Glacier Deep Archive. Since the files are not accessed after 1 month, Glacier Deep Archive provides the best cost savings for long-term storage.

D. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month: S3 One Zone-IA is cheaper, but it stores data in only one Availability Zone, which means it has lower availability and durability than other S3 storage classes. For backup files that need to be kept indefinitely, Glacier Deep Archive is a more suitable choice in terms of data preservation and cost.

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

- [ ] B.  Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.
      
Why is this the correct answer?

- [ ] AWS Cost Explorer provides a user-friendly interface that allows for detailed analysis of AWS costs.
- [ ] It includes granular filtering capabilities, which enable users to analyze costs based on various dimensions such as instance type.
- [ ] This makes it easy to compare EC2 costs over the past two months and identify the specific instance types that contributed to the increased spending due to vertical scaling.
- [ ] Cost Explorer is designed for cost analysis and visualization, thus requiring the least operational overhead for generating the required information.    

Why are the other answers wrong?

A.  Use AWS Budgets to create a budget report and compare EC2 costs based on instance types.

Why it's wrong: AWS Budgets is primarily used for setting budgets and receiving alerts when costs exceed those budgets. While it can provide reports, it is not as granular or flexible as Cost Explorer for in-depth cost analysis based on specific dimensions like instance types.
AWS Budgets focuses on alerting and tracking against a set budget, not detailed historical analysis and graphing of cost components.
B.  Use graphs from the AWS Billing and Cost Management dashboard to compare EC2 costs based on instance types for the last 2 months.

Why it's wrong: The AWS Billing and Cost Management dashboard provides a high-level overview of costs, but it lacks the detailed filtering and analysis capabilities of Cost Explorer.
While it can show overall EC2 costs, it is not designed for granular analysis of costs by instance type over specific periods.
D.  Use AWS Cost and Usage Reports to get a detailed breakdown of each month's EC2 costs and analyze the information.

Why it's wrong: AWS Cost and Usage Reports provide the most comprehensive data about AWS costs, but they are delivered as detailed data files (e.g., CSV) that require additional tools and effort to analyze and visualize the data.
This option involves more operational overhead because it requires setting up a system to download, parse, and analyze the reports, rather than using a built-in tool like Cost Explorer.

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










