# Google cloud APIs

# Installing the Google Cloud SDK

Tools can be downloaded from here:

https://cloud.google.com/sdk

The Google Cloud SDK installs the following tools by default:

 *  gcloud : A command-line interface for managing cloud resources (the majority of commands in gcloud are specific to a given product or service)
 
 *  bq : Commands for interacting with Google BigQuery
 
 *  gsutil : Tools for Google Cloud Storage

 *  cubectl : cli for Kubernetes


# Installation
 
1. Add the Cloud SDK distribution URI as a package source:

```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```

Make sure you have apt-transport-https installed:

```
sudo apt-get install apt-transport-https ca-certificates gnupg
```

Note: Make sure you do not have duplicate entries for the cloud-sdk repo in /etc/apt/sources.list.d/google-cloud-sdk.list

2. Import the Google Cloud public key:

```
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
```

Note: If you are unable to get latest updates due to an expired key, obtain the latest apt-get.gpg key file.

3. Update and install the Cloud SDK:

```
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

4. Optionally, install any of these additional components:

```
google-cloud-sdk-app-engine-python
google-cloud-sdk-app-engine-python-extras
google-cloud-sdk-app-engine-java
google-cloud-sdk-app-engine-go
google-cloud-sdk-bigtable-emulator
google-cloud-sdk-cbt
google-cloud-sdk-cloud-build-local
google-cloud-sdk-datalab
google-cloud-sdk-datastore-emulator
google-cloud-sdk-firestore-emulator
google-cloud-sdk-pubsub-emulator
kubectl
```

For example, the google-cloud-sdk-app-engine-java component can be installed as follows:

```
sudo apt-get install google-cloud-sdk-app-engine-java
```

Run gcloud init to get started (will require creds):

```
gcloud init
```

# Command structure

```
gcloud  {Group: compute, app, etc} {Resource Type: instances, disks, etc} {Action: list, create} {Flags: --zone, --filter, etc}
```


The command 

```
gcloud compute instances list --zone <ZONE>
```

would be the equivalent to making an API call to ```https://www.googleapis.com/compute/v1/projects/<PROJECT_ID>/zones/<ZONE>/instances```


# Authentication

Run ```gcloud auth login``` to open a new browser window and confirm the authorization request. If your session is not aware of a display (as with headless servers), you will be presented with a URL to perform an offline login. Simply copy this link into your browser, retrieve the verification code, and paste the code into your shell. The offline flow can be manually triggered by passing the --no-launch-browser flag.

The Google Cloud SDK has an established process for storing authentication tokens on a system. For local development, the default location is in ```$HOME/.config/gcloud``` for Mac and Linux and ```$HOME\AppData\Roaming\gcloud``` for Windows.

Google Cloud SDKs and libraries are able to load credentials from known default locations, generally referred to as application default credentials. 

When running locally, the Google Cloud Datastore API for Java is able to locate and authenticate using the application-default credentials on your machine. When you deploy the application to Google App Engine, the same method will work here to load the application-default credentials available in the App Engine environment.

During local development, it is often preferable to have Google SDKs and libraries load a specific application-default credential. To do this, users can set the GOOGLE_APPLICATION_CREDENTIALS environment variable to point to a valid credentials file. This will usually be a service account's JSON key file, but you may instead one can use users own credentials by running ```gcloud auth application-default login```. This default credential can then be removed by running ```gcloud auth application-default revoke```.

# Components

Listing all installed components:

```
gcloud components list
```

Components can be installed or removed on an individual basis at any time with 

```
gcloud components <install|remove> <COMPONENT_ID>
```

# Configuration

Run ```gcloud config list --all```. Because configuration properties are stored as key-value pairs, you can also directly read them with ```gcloud config get-value <PROPERTY>``` and change them with ```gcloud config set <PROPERTY> <VALUE>```

