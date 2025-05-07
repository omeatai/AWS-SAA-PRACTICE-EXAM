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
  <summary>Answer 1</summary>

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
  <summary>Answer 2</summary>

  - [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    

</details>

































<details>
  <summary>Question 1</summary>

- [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    
- [ ] B.  Upload the data from each site to an S3 bucket in the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.  Then remove the data from the origin S3 bucket.    
- [ ] C.  Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region.  Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.    
- [ ] D.  Upload the data from each site to an Amazon EC2 instance in the closest Region.  Store the data in an Amazon Elastic Block Store (Amazon EBS) volume.  At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket.  Restore the EBS volume in that Region. 

</details>

<details>
  <summary>Answer 1</summary>

  - [ ] A.  Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.    

</details>










