# Infrastructure

## AWS Zones
_Identify your zones here_

**Primary Zone ( Zone 1 ) :** us-east-2a , us-east-2b

**DR Zone ( Zone 2)  :** us-west-1b , us-west-1c

## Servers and Clusters

### Table 1.1 Summary
| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Asset name | Brief description | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere |
| EC2 instances |  App Servers  |     t3.micro                       |     6                               |    3 instances deployed to DR zone exactly like Primary zone       |
|Key Pair | SSH key for accessing EC2 instances |  | 2  | 1 each for Primary and DR Zone |
|S3 Bucket|  To save Terraform state |   |  2 |      1 each in Primary and DR Zone |
| EKS Cluster |  Kubernetes cluster |t3.medium  | 2 | 1 EKS cluster each in Primary and DR Zone | 
| RDS Cluster | Database |  | 2 | Replicated from Primary zone to DR Zone    |
| Application Load Balancer | For Traffic distribution |   | 2  | One in Primary and other in DR Zone |
| VPC |  Virtual Network |   | 2  | One in Primary Zone and ther one in DR Zone|

### Descriptions
_More detailed descriptions of each asset identified above._


**EC2 instances :**

3 EC2 instances are created in every availability zone in the region

**3 Bucket :**

One s3 bucket named "udacity-tf-jees" is created at us-east-2 region ( Primary Zone) and another one named "udacity-tf-jees-west" created at us-west-1 (DR Zone) . S3 buckets are created for saving the terraform state.

**Key pairs:**

One key pair named "udacity" is created at us-east-2 region ( Primary Zone) and another named "udacity_west" is created at us-west-1 (DR Zone) . These key-pairs are used for connecting to ec2 instances of web servers.

**VPC:**

Virtual Private Cloud (Amazon VPC) is created to launch AWS resources into it. There is one VPC for Primary zone and another zone in DR Zone.  

**RDS cluster:**

Primary RDS cluster (which has one write instance and one read instance) is deployed in us-east-2 region. The secondary RDS cluster (which has 2 read instances) is deployed in us-west-1 region (DR Zone) with replication from the primary cluster . 

**Application Load Balancer :**

Elastic Load Balancing automatically distributes incoming traffic across multiple EC2 instances .


## DR Plan
### Pre-Steps:
_List steps you would perform to setup the infrastructure in the other region. It doesn't have to be super detailed, but high-level should suffice._

Restore & Create AMI image at DR Zone.

Create an S3 bucket for terraform state in DR Zone

Provision VPC, Application Load Balancer (ALB), Security groups, EC2 instances web - servers and EKS cluster in DR Zone.

Provision secondary RDS cluster in us-west-1 region ( DR Zone ) which is replicated from primary RDS cluster in us-east-2 region.

## Steps:
_You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region_

Create a load balancer and point DNS to the load balancer. 

During a failover scenario, DNS entry can be pointed to the DR site. 

Perform a failover on the database.

