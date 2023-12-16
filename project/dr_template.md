# Infrastructure

## AWS Zones
Identify your zones here

**Primary:** us-east-2a , us-east-2b

**Secondary :** us-west-1b , us-west-1c

## Servers and Clusters

### Table 1.1 Summary
| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Asset name | Brief description | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere |
| EC2 instances |  App Servers  |     t3.micro                       |     6                               |    2 zones, 3 instances each zone for DR purpose                |
|S3 Bucket|  For Terraform |   |  2 |      1 in Each region  |
| EKS Cluster |  Kubernetes cluster |  | 2 | 1 EKS cluster in each region | 
| RDS Cluster | Databse |  | 2 | Replicated from Primary zone to Secondary Zone    |
| Application Load Balancer | For Traffic distribution |   | 2  | One in each region |
| VPC |  Virtual Network |   | 2  | One in us-east-1 and the other one in us-west-1 |

### Descriptions
More detailed descriptions of each asset identified above.

**3 Bucket :**

we have created 2 s3 buckets at 2 regions, to save terraform state.

**Key pairs:**

2 key pairs with same name "udacity" creted at 2 regions, it's ssh key-pairs used to connect to ec2 instances of web servers.

**VPC:**

we have created Virtual Private Cloud (Amazon VPC) to launch AWS resources into it, we created subnets public and private in each availability zone of each region.


**EC2 instances for web server:**

we need to create EC2 in every availability zone in the region
AMI images are using to hold the application executable. You have to create and store these AMI images in both regions. Also, these AMI images are copied from us-east-1 region.

**RDS cluster:**

we are deploying two RDS cluster. One RDS cluster as primary cluster is deployed in us-east-2 region. This RDS cluster has one write instance and one read instance. The another RDS cluster as secondary cluster is deployed in us-west-1 region with replication from the primary cluster in us-east.2. This secondary cluster has 2 read instances.

**Application Load Balancer :**

Elastic Load Balancing automatically distributes incoming traffic across multiple EC2 instances more Availability Zones.


## DR Plan
### Pre-Steps:
List steps you would perform to setup the infrastructure in the other region. It doesn't have to be super detailed, but high-level should suffice.

Restore & Create AMI images at 2 regions.
Create S3 buckets for terraform state.
Create private Keypairs with name "udacity" at 2 regions.
Provision: VPC, Application Load Balancer (ALB), Security groups, EC2 instances web - servers and EKS cluster in another region.
Provision primary RDS cluster in us-east-2 region replicated to a secondary RDS cluster in us-west-1 region.
Using Postman collections to initiate the flask app, make traffic.
Provision monitoring stack: prometheus configuration, Grafana dashboard


## Steps:
You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region
