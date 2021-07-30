# Developing Cloud Functions

There are two primary ways to develop and test Cloud Functions: 

* locally using the Cloud Functions simulator
* directly in the cloud via the Cloud Console

# Using Cloud Console

1. Navigate to Navigation menu | Cloud Functions (must enable the Cloud Functions API, which can be done from this page by clicking Enable API)

2. Click Create Function to get started.

3. The function creation page allows developers to specify a function name, memory allocation, the function trigger type, and code to execute. With the Source code option set to Inline editor, a simple example function will be provided in the editor pane based on the type of trigger selected (use the sample from code/06 directory)

4. Once created, the console will navigate to the Cloud Functions overview page, where developers can view and interact with their functions. 

To test the function we just created, click on the function ( function-1 by default), and navigate to the Trigger tab. Because we're using an HTTP trigger, a URL is provided to invoke this function. One can test the function by performing an HTTP GET on this URL in the Cloud Shell. Simply open the Cloud Shell and run:

```
curl <PROVIDED_URL>?name=user
```

# Local Development

Google provides a Cloud Functions emulator to facilitate local development, which can be installed as a traditional npm package. To install the Cloud Functions emulator, run:

```
npm install -g @google-cloud/functions-emulator
```

# Deployment

The most straightforward - using Cloud Console inline editor (see the section above)

For real-world applications, there are a few better alternatives:

* Deploying from a GCS bucket
* Deploying from a local filesystem
* Deploying from a Google Cloud Source Repository

 