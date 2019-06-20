# DynamoDB

![AWS DynamoDB](images/Database_AmazonDynamoDB.png)

Having used AWS DynamoDB for [Alexa](http://github.com/mramshaw/Alexa-Stuff/tree/master/DynamoDB),
it was time to investigate DynamoDB as a NoSQL database in its own right.

## Contents

The contents are as follows:

* [Motivation](#motivation)
    * [DBaaS](#dbaas)
    * [Auto-scaling](#auto-scaling)
    * [Global Tables](#global-tables)
    * [Advanced query capabilities](#advanced-query-capabilities)
    * [Ease of use](#ease-of-use)
* [Alternatives](#alternatives)
    * [AWS](#aws)
    * [Third-Party](#third-party)
* [Performance](#performance)
* [Offline use](#offline-use)
    * [Docker Tags](#docker-tags)
* [Security](#security)
* [Reference](#reference)
    * [Tracking Your Free Tier Usage](#tracking-your-free-tier-usage)
    * [AWS Billing and Cost Management](#aws-billing-and-cost-management)
    * [Billing alarm](#billing-alarm)
    * [Local usage](#local-usage)
* [To Do](#to-do)

## Motivation

I will be examining DynamoDB for use in ___serverless architectures___ as a replacement for MongoDB.

#### DBaaS

Specifically, I will be examining DynamoDB (and alternatives) for __Database as a Service (DBaaS)__ capabilities
 - as a principal component of serverless architectures is the ability to outsource the bulk of database operations.

In particular, the ability to scale and maintain MongoDB at enterprise levels seems to involve substantial time and
costs.

#### Auto-scaling

DynamoDB auto-scaling is relatively simple but probably does not extend across AWS regions.

#### Global Tables

AWS offers [Global Tables](http://aws.amazon.com/dynamodb/global-tables/) for replication across regions.

[This is a premium service, and may well be overkill for most use cases.]

#### Advanced query capabilities

To the extent that they are needed, Advanced query capabilities (text search, JSON parsing) are nice to have.

Of course, for more complicated use cases GraphQL is an option, but this is to be avoided if at all possible.

#### Ease of use

Consistent with usage and query requirements, it should as simple as possible to use and manage.

[DocumentDB is simple to set up and administrate, although the IAM aspects can be tricky.]

## Alternatives

The following is a quick list of alternatives to DynamoDB.

Some notes will be listed, otherwise how to evaluate DocumentDB against the alternatives?

[It will be assumed that what is required is a JSON-friendly NoSQL DBaaS that can auto-scale.]

Wikipedia has a good (if slightly out-of-date) summary of the alternatives: http://en.wikipedia.org/wiki/DBaaS

> NoSQL databases are built to service heavy read/write loads and can scale up and down easily

[The article makes no mention of either AWS DocumentDB or Azure DocumentDB.]

#### AWS

[Amazon DocumentDB](http://aws.amazon.com/documentdb/)

* Seems to have been designed as a drop-in replacement for MongoDB
* Apparently runs on Aurora PostgreSQL under the covers
* Does not support all [MongoDB data types](http://docs.aws.amazon.com/documentdb/latest/developerguide/mongo-apis-data-types.html)
* Probably overkill for simple use cases
* Probably not a good choice for greenfield applications
* Apparently has the same price structure as MongoDB Atlas

#### Third-Party

[MongoDB](http://www.mongodb.com/)

* The original
* Has cloud offerings, but seems to be playing catch-up with AWS and Azure (both of which have competing offerings)
* Apparently has the same price structure as AWS DocumentDB

[Azure Cosmos DB](http://azure.microsoft.com/en-us/services/cosmos-db/)

* Apparently rebranded from Azure DocumentDB in 2017

[I am unfamiliar with Azure so I won't be examining this product.]

[CouchBase](http://www.couchbase.com/)

* Seems to have better scaling and replication options
* Probably more JSON-friendly than its competitors
* Has its own query language (N1QL)
* Not really DBaaS

[Check out my [Couchbase repo](http://github.com/mramshaw/RESTful-Couchbase).]

## Performance

![AWS DynamoDB Accelerator (DAX)](images/Database_AmazonDynamoDBAccelerator.png)

If performance becomes an issue, it is always possible to add a caching layer with
[Amazon DynamoDB Accelerator (DAX)](http://aws.amazon.com/dynamodb/dax/).

## Offline use

DynamoDB is available for [local use](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html).

Probably the best option is to use the [Dockerized version](http://hub.docker.com/r/amazon/dynamodb-local):

```bash
$ docker run -p 8000:8000 amazon/dynamodb-local
```

Usage notes: http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.UsageNotes.html

#### Docker Tags

You can find the Docker tags [here](http://hub.docker.com/r/amazon/dynamodb-local/tags).

The example shown above really defaults to:

```bash
$ docker run -p 8000:8000 amazon/dynamodb-local:latest
```

And a better option is to use a tagged version as follows:

```bash
$ docker run -p 8000:8000 amazon/dynamodb-local:tag
```

## Security

My approach to security involves the principle of ___Least Privilege___.

Accordingly, it's better to allocate 'YourTableName' manually rather than give Create permission.

For online use, restrict access as follows:

	{
	  "Version": "2012-10-17",
	  "Statement": [
	    {
	      "Effect": "Allow",
	      "Action": [
	        "dynamodb:PutItem",
	        "dynamodb:GetItem"
	      ],
	      "Resource":["arn:aws:dynamodb:us-east-1:xxxxxxxxxxxx:table/YourTableName"],
	    }
	  ]
	}

## Reference

As always with the cloud, documentation is voluminous. Some useful links are listed below.

#### Tracking Your Free Tier Usage

Tracking Your Free Tier Usage: http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html

#### AWS Billing and Cost Management

What Is AWS Billing and Cost Management?: http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html

#### Billing Alarm

Billing alarm: http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html

> You must be signed in using AWS account root user credentials; IAM users cannot enable billing alerts for your AWS account.

#### Local usage

Local usage notes: http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.UsageNotes.html

## To Do

- [ ] Investigate MongoDB cloud offerings (Atlas, etc)
- [ ] Investigate offline use
- [ ] Investigate [AWS Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ce-enable.html)
- [ ] Investigate AWS Budgets (budgeting, cost allocation tags, alerts, consolidated billing)
- [ ] Investigate billing alerts
- [ ] Investigate current IAM and RBAC
- [ ] Verify if the access permissions shown above are still current
- [ ] Investigate auto-scaling
- [ ] Investigate [Global Tables](http://aws.amazon.com/dynamodb/global-tables/)
