The event bus targets (API Gateway endpoint, AWS Step Functions state machine, and Amazon SNS topic) have been provisioned for you. 
The goal is for you to write event bus rules to match events and verify delivery to the appropriate target.

Events in Amazon EventBridge are represented as JSON objects and have the following envelope signature:

{
  "version": "0",
  "id": "6a7e8feb-b491-4cf7-a9f1-bf3703467718",
  "detail-type": "EC2 Instance State-change Notification",
  "source": "aws.ec2",
  "account": "111111111111",
  "time": "2017-12-22T18:43:48Z",
  "region": "us-west-1",
  "resources": [
    "arn:aws:ec2:us-west-1:123456789012:instance/ i-1234567890abcdef0"
  ],
  "detail": {
    "instance-id": " i-1234567890abcdef0",
    "state": "terminated"
  }
}


Rules use event patterns to select events and route them to targets.
A pattern either matches an event or it doesn't. Event patterns are represented as JSON objects with a structure that is similar to that of events.
The above example - chosen source is EC2 and the action is to terminate the instance.


When following the tutorial on AWS - you must have a set up a cloudformation - Through Cloudformation with we can connect to userpools and this is how 
we will link to an API.

Open the AWS Management Console for EventBridge  in a new tab or window, so you can keep this step-by-step guide open.

On the EventBridge homepage, select API destinations from the left navigation.

On the API destinations page, select Create API destination.

On the Create API destination page

Enter api-destination as the Name
Enter the API URL identified in Step 1 as the API destination endpoint
Select POST as the HTTP method
Select Create a new connection for the Connection
Enter basic-auth-connection as the Connection name
Select Basic (Username/Password) as the Authorization type
Enter myUsername as the Username
Enter myPassword as the Password
![image](https://user-images.githubusercontent.com/71371405/217516110-40631e0b-1bd6-499b-9b30-574647852bc4.png)

Then hit create!

Step 3: Configure an EventBridge rule to target the EventBridge API Destination
From the left-hand menu, select Rules.

From the Event bus dropdown, select the Orders event bus.

Click Create rule

On the Define rule detail page

Enter OrdersEventsRule as the Name of the rule
Enter Send com.aws.orders source events to API Destination for Description
Under Build event pattern

Choose Other for your Event source
Copy and paste the following into the Event pattern, and select Next to specify your target:

{
    "source": [
        "com.aws.orders"
    ]
}

![image](https://user-images.githubusercontent.com/71371405/217518273-0dd14369-75f2-489f-9bb3-63bd13c94e15.png)

You can use the generator to send traffic then look at cloudwatch logs to see the API receive the tarrfic via logs.

WELL DONE THAT'S YOUR API SET UP. MOVING ON TO STEP FUNCTIONS 

Add a rule to the Orders event bus with the name EUOrdersRule
Define an event pattern to match events with a detail location in eu-west or eu-east
Target the OrderProcessing Step Functions state machine

Using the Event Generator, send the following Order Notification events from the source com.aws.orders:
{ "category": "office-supplies", "value": 300, "location": "eu-west" }

NOW YOU HAVE A STEP FUNCTION TO PROCESS THE ORDERING! AND FINALLY SNS BECAUSE WE ALL NEED TO BE NOTIFIED.

Step 1: Implement an EventBridge rule to target SNS
Add a rule to the Orders event bus with the name USLabSupplyRule
With an event pattern to match events with a detail location in us-west or us-east, and a detail category with lab-supplies.
Target the Orders SNS topic

here's a sample 
{
    "version": "0",
    "id": "6e6b1f6d-48f8-5dff-c2d2-a6f22c2e0086",
    "detail-type": "Order Notification",
    "source": "com.aws.orders",
    "account": "111111111111",
    "time": "2020-02-23T15:35:41Z",
    "region": "us-east-1",
    "resources": [],
    "detail": {
        "category": "lab-supplies",
        "value": 300,
        "location": "us-east"
    }
}

If the event sent to the Orders event bus matches the pattern in your rule, then the event will be sent to the Orders SQS Queue (via Orders SNS Topic).

Open the AWS Management Console for SQS  in a new tab or window, so you can keep this step-by-step guide open.

On the SQS homepage, select the Orders queue.

Select the Send and receive messages button.

Select Poll for Messages and verify the first message was delivered and the second was not.

To clean up, select the event, select the Delete button, and select the Delete button again on the Delete Messages confirmation dialog.

If the event sent to the Orders event bus matches the pattern in your rule, then the event will be sent to the Orders SQS Queue (via Orders SNS Topic).

Open the AWS Management Console for SQS  in a new tab or window, so you can keep this step-by-step guide open.

On the SQS homepage, select the Orders queue.

Select the Send and receive messages button.

Select Poll for Messages and verify the first message was delivered and the second was not.

To clean up, select the event, select the Delete button, and select the Delete button again on the Delete Messages confirmation dialog.

CONGRATULATION YOU MADE IT.   






  
