# AWS Sam Tutorial

* https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html

* In this guide, you download, build, and deploy a sample Hello World application using AWS SAM. You then test the application in the AWS Cloud, and optionally test it locally on your development host.

* The following is a preview of commands that you run to create your Hello World application. For more information about each of these commands, see the sections later in this tutorial.
```
#Step 1 - Download a sample application
sam init

#Step 2 - Build your application
cd sam-app
sam build

#Step 3 - Deploy your application
sam deploy --guided
```

* `sam init` generates the project structure in `sam-app`. There are three especially important files:
    * `template.yaml`: Contains the AWS SAM template that defines your application's AWS resources.

    * `hello_world/app.py`: Contains your actual Lambda handler logic.

    * `hello_world/requirements.txt`: Contains any Python dependencies that the application requires, and is used for sam build.

* Change into the project directory where the `template.yml` file is. then run `sam build`. It will build dependencies that the app has, and copy app source code under `.aws-sam/build`:
```
 .aws-sam/
   └── build/
       ├── HelloWorldFunction/
       └── template.yaml
```

* Deploy your app `sam deploy --guided`


### Step 4: (Optional) Test your application locally
* When you're developing your application, you might find it useful to test locally. The AWS SAM CLI provides the `sam local` command to run your application using Docker containers that simulate the execution environment of Lambda. There are two options to do this:

    * Host your API locally

    * Invoke your Lambda function directly


#### Host your API Locally
* `sam local start-api`
* Now check the results at `curl http://127.0.0.1:3000/hello`

#### Invoke your Lambda function directly
* sam local invoke "HelloWorldFunction" -e events/event.json
* The `invoke` command directly invokes your Lambda functions, and can pass input event payloads that you provide. With this command, you pass the event payload in the file `event.json` that the sample application provides.
* Your initialized application comes with a default aws-proxy event for API Gateway. A number of values are pre-populated for you. In this case, the HelloWorldFunction doesn't care about the particular values, so a stubbed request is OK. You can specify a number of values to substitute in to the request to simulate what you would expect from an actual request. The following is an example of generating your own input event and comparing the output with the default event.json object:
```
sam local generate-event apigateway aws-proxy --body "" --path "hello" --method GET > api-event.json
diff api-event.json events/event.json
```