# AWS-SAA-PRACTICE-EXAM
AWS-SAA-PRACTICE-EXAM

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
  <summary>Question X</summary>

- [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    
- [ ] B.  Upload the data from each site to an S3 bucket in the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.  Then remove the data from the origin S3 bucket.    

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    

</details>










