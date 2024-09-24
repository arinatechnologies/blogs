---
title: 'How to Export AWS CloudWatch Logs to OpenSearch(Elasticsearch) Integration:A Complete Guide'
date: '2024-09-29'
image: 'https://raw.githubusercontent.com/arinatechnologies/blogs/f8cb218a053afcdca7c93dcea55c3780ea0cfc53/images/awscloudwatchlogtoopensearch.webp'
tags: ['AWS CloudWatch', 'OpenSearch', 'Elasticsearch', 'Log Management', 'AWS Lambda','Cloud Management','CloudWatch Logs Export','AWS IAM Roles','Cloudflare Domain Management','Cloud Monitoring','Cloud Logging Solutions','AWS Cloud Infrastructure','DevOps Tools']
---
Cloud-based applications generate massive amounts of log data, and handling this efficiently is critical. AWS CloudWatch is widely used to monitor log data, but for heavy-duty search and analysis, integrating it with OpenSearch (formerly Elasticsearch) provides enhanced performance and cost efficiency.

In this guide, I’ll walk you through exporting AWS CloudWatch logs to OpenSearch, step-by-step, including common pitfalls and solutions.
<Video id="Yeait-I_2Vk" title="How to Export AWS CloudWatch Logs to OpenSearch(Elasticsearch) Integration:A Complete Guide "/>
### Why Export CloudWatch Logs to OpenSearch?

AWS CloudWatch is great for monitoring logs, but it has limitations when dealing with large datasets:

- **High Costs**: CloudWatch can be expensive for long-term log storage.
- **Limited Search Capabilities**: Searching through vast amounts of data can be slow and cumbersome.
- **Retention Issues**: It’s challenging to access logs beyond a certain retention period.

By exporting logs to OpenSearch, you benefit from enhanced search capabilities, better performance, and reduced storage costs.
### Step 1: Create an OpenSearch Cluster

First, we need to create an OpenSearch cluster that will receive and store the CloudWatch logs.

1. **Open the AWS OpenSearch Console**: Navigate to the OpenSearch service dashboard in AWS.
2. **Create Domain**:
   - Choose **Create Domain**.
   - Select **Standard Create** (avoid “Easy Create” to avoid unnecessary resources).
   - Choose **Development/Test** for this example.
3. **Choose Instance Type**:
   - Select **M5 family** instances, as they provide a good balance between cost and performance.
4. **Set Access Options**:
   - For this demo, we’ll choose **Public Access** for ease of configuration, but in production, it’s best to use **VPC Access** for security.
   - Choose **IPv4**.
5. **Configure Master User**:
   - Set up a master user with a username and password.
6. **Create Domain**:
   - Click **Create Domain** and wait for the cluster to be provisioned (this can take around 30 minutes).

Once the OpenSearch cluster is active, you’ll have access to the OpenSearch dashboard (previously known as Kibana).
### Step 2: Configure AWS CloudWatch to Export Logs

Now that we have our OpenSearch cluster, let’s set up AWS CloudWatch to export logs.

1. **Open CloudWatch**:
   - Navigate to the **Log Groups** section in CloudWatch.
   - Choose a **Log Group** to export.
2. **Create Subscription Filter**:
   - Select **Subscription Filters** and create a new filter.
   - Set **OpenSearch** as the destination.
3. **Create Lambda Function**:
   - You’ll need to create a Lambda function to handle the log export. We’ll configure permissions in the next step.
### Step 3: Set Up IAM Roles for Lambda

To give Lambda the necessary permissions to push logs to OpenSearch, we need to create an IAM role.

1. **Open IAM Console**:
   - Create a new role for **Lambda** with **VPC Access** permissions.
2. **Attach Policy**:
   - Attach a policy to grant **OpenSearch service** permissions. You can create an inline policy that includes permissions like `opensearch:ESHttp*`.
3. **Name the Policy**:
   - Name it something relevant, like `CWL_OpenSearch_Policy`.
   
Once the role is ready, assign it to your Lambda function.
### Step 4: Customize the Lambda Function

In some cases, you’ll want to customize how the Lambda function handles logs, especially when dealing with multiple log groups.

1. **Modify the Code**:
   - In the Lambda function, you can add logic to handle different log groups and export them to different indices in OpenSearch.
   - For example, you can add a check for the log group name and map each to a specific index.
   
   Here’s a sample of how to update the code:

   ```python
   if log_group == "example-log-group-1":
       index_name = "log-group-1-index"
   elif log_group == "example-log-group-2":
       index_name = "log-group-2-index"
   ```

2. **Deploy the Function**:
   - Deploy the Lambda function and monitor if the logs are being exported correctly.
### Step 5: Check and Debug the Log Flow

Once everything is set up, verify that logs are flowing into OpenSearch.

1. **Use OpenSearch Dev Tools**:
   - Navigate to **Dev Tools** in OpenSearch and run a query to check if logs are being indexed.
   - If you encounter issues, enable debugging in Lambda by setting the logging level to `DEBUG`.
   
   For example:

   ```python
   logging.getLogger().setLevel(logging.DEBUG)
   ```

2. **Common Issues**:
   - **403 Forbidden Errors**: This can happen if the IAM role doesn’t have sufficient permissions. Ensure that your IAM role has the correct access to OpenSearch and CloudWatch.
### Step 6: Managing Multiple Log Groups

If you’re exporting multiple log groups, it’s essential to ensure that each log group maps to a different index in OpenSearch.

1. **Modify Lambda Code for Multiple Log Groups**:
   - You’ll need to update your Lambda function code to distinguish between log groups and send logs to separate indices.
   - Here’s an example:

   ```python
   if log_group == "app-log-group-1":
       index_name = "app-log-group-1-index"
   elif log_group == "app-log-group-2":
       index_name = "app-log-group-2-index"
   ```

2. **Avoid Mixing Data**:
   - Ensure that logs from different sources are kept separate to avoid confusion.
### Step 7: Create User Roles in OpenSearch

For better security and user management, create roles in OpenSearch that limit users to specific indices.

1. **Go to Security Settings**:
   - Navigate to **Security** in OpenSearch and create new roles for different users.
2. **Assign Permissions**:
   - Assign roles that give users access to only the necessary indices (log groups).
   - For example, you can allow read-only access to one index while providing full access to another.

3. **Map Users to Roles**:
   - Once the roles are created, map them to users so that they have the right level of access.
### Step 8: Final Testing and Tuning

After configuring everything, test the entire setup to ensure logs are being exported, indexed, and displayed correctly.

1. **Query Logs**:
   - Run queries in OpenSearch to check if logs from all intended log groups are being indexed.
   
2. **Optimize Performance**:
   - If you encounter performance issues, consider upgrading your OpenSearch instances or increasing the number of nodes.
### Conclusion

Exporting AWS CloudWatch logs to OpenSearch is a powerful solution to enhance log search, analysis, and storage capabilities. By following the steps outlined in this guide, you can set up a robust log export system that ensures better performance, security, and cost efficiency.

For more cloud solutions, be sure to like, share, and subscribe to our channel. If you have any questions or need assistance with AWS or OpenSearch, feel free to reach out—we’re happy to help!

--- 
 [ Refer Cloud Consulting](https://www.arinatechnologies.com/consulting) <br/>
Ready to take your cloud infrastructure to the next level? Please reach out to us [ Contact Us](https://www.arinatechnologies.com/contact) <br/>
# Other Blogs
[Step-by-Step Guide: Install and Configure GitLab on AWS EC2 | DevOps CI/CD with GitLab on AWS](https://www.arinatechnologies.com/posts/gitLabonaws) <br/>
[Simplifying AWS Notifications: A Guide to User Notifications](https://www.arinatechnologies.com/posts/user-notifications) <br/>
