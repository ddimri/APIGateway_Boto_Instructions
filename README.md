# APIGateway Instructions

```
Create dynamodb table "comments" and key item "commentsId"
Create index with primary key as "pageId" and add attributes "pageId","userName","message"
Create IAM role "apigw-dynamodb-role" with permissions for "get,put,list" to you db table

Create  new API "commentsApi"
Create resource "comments"
Create new method "post" --> integration type --> AWS Service ; region --> us-west-2; AWS Service --> DynamoDB;  HTTP Method --> POST; content handling --> passthrough
Create body mapping template content type --> application/json

	{ 
    "TableName": "comments",
    "Item": {
	"commentId": {
            "S": "$context.requestId"
            },
        "pageId": {
            "S": "$input.path('$.pageId')"
            },
        "userName": {
            "S": "$input.path('$.userName')"
        },
        "message": {
            "S": "$input.path('$.message')"
        }
    }
}
```

Test the APIGateway

```
	{ "commentsId": "1.0"
  "pageId":   "AWS Training June 21st",
  "userName": "deepakprasad",
  "message":  "Loved the training!!!"
}
```
