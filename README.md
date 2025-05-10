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























</details>



























<details>
  <summary>Question X</summary>

- [ ]    

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn on S3 Transfer Acceleration

</details>










