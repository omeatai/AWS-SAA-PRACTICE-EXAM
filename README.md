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





















</details>



























<details>
  <summary>Question X</summary>

- [ ]    

</details>

<details>
  <summary>Answer</summary>

- [ ] A.  Turn on S3 Transfer Acceleration

</details>










