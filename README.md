# DynamoDB

![AWS DynamoDB](images/Database_AmazonDynamoDB.png)

Having previously used AWS DynamoDB for [Alexa](http://github.com/mramshaw/Alexa-Stuff/tree/master/DynamoDB),
it seemed to be time to investigate DynamoDB as a NoSQL database in its own right.

Specifically, I will be examining it for use in ___serverless architectures___ as a replacement for MongoDB.

## Performance

![AWS DynamoDB Accelerator (DAX)](images/Database_AmazonDynamoDBAccelerator.png)

If performance becomes an issue, it is always possible to add a caching layer with
[Amazon DynamoDB Accelerator (DAX)](http://aws.amazon.com/dynamodb/dax/).

## Offline use

DynamoDB is available for [local use](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html).

Probably the best option is to use the [Dockerized version](http://hub.docker.com/r/amazon/dynamodb-local).

Usage notes: http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.UsageNotes.html

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

## To Do

- [ ] Verify if the access permissions shown above are still current
- [ ] Investigate [AWS Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ce-enable.html)
- [ ] Investigate billing alerts
- [ ] Investigate current IAM and RBAC
- [ ] Investigate auto-scaling
- [ ] Investigate [Global Tables](http://aws.amazon.com/dynamodb/global-tables/)
