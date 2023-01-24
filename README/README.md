# USE CASE: Building a Web Application
- This will be a basic web application that displays the "My name is Taibou! Welcome to my static website".

# Tasks:
- Create a web app
- Connect the web application to a serverless backend
- Add a connection between the web application with an API Gateway 
- Create a database

# The Application Architecture

![WebApp-Architecture](https://user-images.githubusercontent.com/94193627/214354950-cbbd95dc-f86b-42ff-ba6c-2fb2bff7e149.svg)


- This diagram shows all the services used and their interconnectivity. This application uses AWS Amplify, Amazon API Gateway, AWS Lambda, AWS Identity and Access Management, and DynamoDB.

# Solution 

### Task 1: Creating the Web App
- Create the Amplify app
- Upload the files for a website directly to Amplify
- Using Amplify, deploy new versions of a webpage


### Task 1.1: By using the Amplify console, I deployed the HTML file below to host the web app.

```bash
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
</head>

<body>
    My name is Taibou! Welcome to my static website
</body>
</html>
```

### Task 1.2: Testing the Web Application

After creating my environment to deploy the HTML file, I copied and pasted the URL to a new browser to test and see if my app is working. As you can see, the screenshot below shows that is is working. 

<img width="1262" alt="Screenshot 2023-01-24 at 12 37 45 AM" src="https://user-images.githubusercontent.com/94193627/214355347-5904afb4-4099-406b-ae85-1cf4c1d6be15.png">

### Task 2: Building the Serverless Backend
- Create a Lambda function in python
- Create JSON events in the AWS console to test the function

### Task 2.1: Using the Lambda console, I created and configured a Lambda function

<img width="1224" alt="Screenshot 2023-01-24 at 1 01 15 AM" src="https://user-images.githubusercontent.com/94193627/214355691-7c0c554c-bb8a-41db-b599-919f49232788.png">

I created a HelloWorldFunction using python as the code source and deployed it.
```bash
import json
def lambda_handler(event, context):
    name = event['Taibou'] +' '+ event['Diallo']
    return {
    'statusCode': 200,
    'body': json.dumps('Hello from Lambda, ' + name)
    }
```

### Task 2.2: Test Function Using Events

<img width="1246" alt="Screenshot 2023-01-24 at 1 02 40 AM" src="https://user-images.githubusercontent.com/94193627/214355668-30aa362e-aa04-4203-9653-3c7d65df6abb.png">

After selecting the test tab, the function has run successfully with the follwoing message "Execution result: succeeded."

### Task 3: Interconnectivity Between Serverless Function and Web App
- Create a new API 
- Define HTTP methods on the API
- Trigger a Lambda function from an API
- Enable cross-origin resource sharing (CORS) on the API 
- Test the API from the AWS Console

### Task 3.1: 
When creating the new API, there were different card options to choose from but for this case I choose the REST API card because REST stands for "Representational State Transfer" and is an architectural pattern for creating web services. API stands for "application programming interface." Thus, a RESTful API is one that implements the REST architectural pattern.

Therefore, the settings should look like the screenshot below.

<img width="485" alt="Screenshot 2023-01-24 at 12 29 21 PM" src="https://user-images.githubusercontent.com/94193627/214365539-3f208e27-3dc0-4bf5-bb86-ecbf9b725449.png">

Edge optimized — A resource that uses AWS global infrastructure to better serve geographically diverse clients.

### Task 3.2:
When defining HTTP methods on the API, the following settings should be selected

<img width="485" alt="Screenshot 2023-01-24 at 12 33 30 PM" src="https://user-images.githubusercontent.com/94193627/214366478-a0ae0173-2ca1-443b-ba70-face447b67b4.png">

<img width="492" alt="Screenshot 2023-01-24 at 12 33 57 PM" src="https://user-images.githubusercontent.com/94193627/214366531-df2d5797-5bf0-4a6b-bca0-d04b84f9632b.png">

The CORS browser security feature uses HTTP headers to tell a browser to allow a given web application running at one origin (domain) to access selected resources from a server at a different origin.

### Task 3.3: Testing the API from the AWS Console

<img width="1437" alt="Screenshot 2023-01-24 at 1 25 12 AM" src="https://user-images.githubusercontent.com/94193627/214355650-c8a7f74e-4c50-4c5f-8b58-3900eed2625c.png">

<img width="1449" alt="Screenshot 2023-01-24 at 1 27 34 AM" src="https://user-images.githubusercontent.com/94193627/214355626-eb09a7cb-49f3-4596-ad6c-cd34b6466439.png">

<img width="1391" alt="Screenshot 2023-01-24 at 2 13 46 AM" src="https://user-images.githubusercontent.com/94193627/214355421-237e2279-4802-46d3-b1a8-9e3a61f41694.png">

### Task 4: Using Amazon DynamoDB, which is a fully managed service I created a table to maintain the data
- Create a DynamoDB table 
- Create a role and manage permissions with IAM
- Write to a DynamoDB using python

### Task 4.1: Creating a DynamoDB table called "HelloWorldDatabase"
For the database table's primary key (Partition Key), I typed "ID" and created the table.

<img width="1239" alt="Screenshot 2023-01-24 at 1 39 18 AM" src="https://user-images.githubusercontent.com/94193627/214355583-2e6ea28d-d884-4542-a086-83de4300c4e6.png">

### Task 4.2: Creating a role with managed permissions in IAM

<img width="1185" alt="Screenshot 2023-01-24 at 1 48 53 AM" src="https://user-images.githubusercontent.com/94193627/214355516-72ff0b15-01ee-4683-a1c7-eaf7d136e65e.png">

This policy will allow the Lambda function to read, edit, or delete items, but restrict it to only be able to do so in the table created.

### Task 4.3: Using/modifying Lambda to write to write to the DynamoDB table with python

```bash
import json
import boto3
from time import gmtime, strftime
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('HelloWorldDatabase')
now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())
def lambda_handler(event, context):
    name = event['First'] +' '+ event['Last']
    response = table.put_item(
        Item={
            'ID': name,
            'LatestGreetingTime':now
            })
# return a properly formatted JSON object
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda, ' + name)
    }
```

### Task 4.4: 
I will update the static website created to invoke the REST API, this will display the text based on the input.

```bash
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
    <style>
    body {
        background-color: #232F3E;
        }
    label, button {
        color: #FF9900;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 20px;
        margin-left: 40px;
        }
     input {
        color: #232F3E;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 20px;
        margin-left: 20px;
        }
    </style>
    <script>
        var callAPI = (firstName,lastName)=>{
            var myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");
            var raw = JSON.stringify({"firstName":firstName,"lastName":lastName});
            var requestOptions = {
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: 'follow'
            };
            fetch("https://1d8di4ydwe.execute-api.us-west-2.amazonaws.com/dev", requestOptions)
            .then(response => response.text())
            .then(result => alert(JSON.parse(result).body))
            .catch(error => console.log('error', error));
        }
    </script>
</head>
<body>
    <form>
        <label>First Name :</label>
        <input type="text" id="fName">
        <label>Last Name :</label>
        <input type="text" id="lName">
        <button type="button" onclick="callAPI(document.getElementById('fName').value,document.getElementById('lName').value)">Call API</button>
    </form>
</body>
</html>
```
Copying the URL under "domain", and pasting it to a new browser and following the direction. I typed in my full name and the following message "Hello from lambda" appears.

<img width="1470" alt="Screenshot 2023-01-24 at 2 11 09 AM" src="https://user-images.githubusercontent.com/94193627/214355483-15ba00f3-1bad-4d78-aee1-47930c3ad4ab.png">

<img width="1466" alt="Screenshot 2023-01-24 at 2 11 37 AM" src="https://user-images.githubusercontent.com/94193627/214355458-5aa1265e-44e3-40be-bd51-d810bc978dd9.png">
