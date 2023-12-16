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
| EKS Cluster |  Kubernetes cluster |  | 2 | 1 EKS cluster each in Primary and DR Zone | 
| RDS Cluster | Database |  | 2 | Replicated from Primary zone to DR Zone    |
| Application Load Balancer | For Traffic distribution |   | 2  | One in Primary and other in DR Zone |
| VPC |  Virtual Network |   | 2  | One in Primary Zone and ther one in DR Zone|

### Descriptions
_More detailed descriptions of each asset identified above._


**EC2 instances :**

EC2 instances are created in every availability zone in the region

**3 Bucket :**

2 s3 buckets are created at 2 regions, to save terraform state.

**Key pairs:**

2 key pairs with name "udacity" and "udacity_west" are created at 2 regions, it's ssh key-pairs for connecting to ec2 instances of web servers.

**VPC:**

Virtual Private Cloud (Amazon VPC) is created to launch AWS resources into it, subnets public and private created in each availability zone of each region.

**RDS cluster:**

we are deploying two RDS cluster. One RDS cluster as primary cluster is deployed in us-east-2 region. This RDS cluster has one write instance and one read instance. The another RDS cluster as secondary cluster is deployed in us-west-1 region with replication from the primary cluster in us-east.2. This secondary cluster has 2 read instances.

**Application Load Balancer :**

Elastic Load Balancing automatically distributes incoming traffic across multiple EC2 instances more Availability Zones.


## DR Plan
### Pre-Steps:
_List steps you would perform to setup the infrastructure in the other region. It doesn't have to be super detailed, but high-level should suffice._

Restore & Create AMI images at 2 regions.

Create S3 buckets for terraform state.

Create private Keypairs with name "udacity" at 2 regions.

Provision: VPC, Application Load Balancer (ALB), Security groups, EC2 instances web - servers and EKS cluster in another region.

Provision primary RDS cluster in us-east-2 region replicated to a secondary RDS cluster in us-west-1 region.

Using Postman collections to initiate the flask app, make traffic.

Provision monitoring stack: prometheus configuration, Grafana dashboard


## Steps:
_You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region_

Create a cloud load balancer and point DNS to the load balancer. This way you can have multiple instances behind 1 IP in a region. During a failover scenario, you would fail over the single DNS entry at your DNS provider to point to the DR site. This is much more intelligent than pointing to a single instance of a web server. Have a replicated database and perform a failover on the database. While a backup is good and necessary, it is time-consuming to restore from backup. In this DR step, you would have already configured replication and would perform the database failover. Ideally, your application would be using a generic CNAME DNS record and would just connect to the DR instance of the database.
