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

- [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    

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










