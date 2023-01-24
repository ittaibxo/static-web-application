# USE CASE: Building a Web Application
- This will be a basic web application that displays the "My name is Taibou! Welcome to my static website".

# Tasks:
- Create a web app
- Connect the web application to a serverless backend
- Add a connection between the web application with an API Gateway 
- Create a database

# The Application Architecture


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

add image here

### Task 2: Building the Serverless Backend
- Create a Lambda function in python
- Create JSON events in the AWS console to test the function

### Task 2.1: Using the Lambda console, I created and configured a Lambda function

-add image here

### Task 2.2: Test Function Using Events

-add image here

### Task 3: Interconnectivity Between Serverless Function and Web App
- Create a new API 
- Define HTTP methods on the API
- Trigger a Lambda function from an API
- Enable cross-origin resource sharing (CORS) on the API 
- Test the API from the AWS Console

### Task 3.1: 






### Task 4: Using Amazon DynamoDB, which is a fully managed service I created a table to maintain the data
- Create a DynamoDB table 
- Create a role and manage permissions with IAM
- Write to a DynamoDB using python








```bash


```




### I will update the static website I created to invoke the REST API 

- Call an API Gateway from the HTML page
- Upload a new version of a web app to the Amplify console

```bash
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
    <!-- Add some CSS to change client UI -->
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
        // define the callAPI function that takes a first name and last name as parameters
        var callAPI = (firstName,lastName)=>{
            // instantiate a headers object
            var myHeaders = new Headers();
            // add content type header to object
            myHeaders.append("Content-Type", "application/json");
            // using built in JSON utility package turn object to string and store in a variable
            var raw = JSON.stringify({"firstName":firstName,"lastName":lastName});
            // create a JSON object with parameters for API call and store in a variable
            var requestOptions = {
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: 'follow'
            };
            // make API call with parameters and use promises to get response
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
        <!-- set button onClick method to call function we defined passing input values as parameters -->
        <button type="button" onclick="callAPI(document.getElementById('fName').value,document.getElementById('lName').value)">Call API</button>
    </form>
</body>
</html>
```






https://1d8di4ydwe.execute-api.us-west-2.amazonaws.com/dev


arn:aws:dynamodb:us-west-2:922268791266:table/HelloWorldDatabase


