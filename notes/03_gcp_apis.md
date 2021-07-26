# Google cloud APIs

# Installing the Google Cloud SDK

Tools can be downloaded from here:

https://cloud.google.com/sdk

The Google Cloud SDK installs the following tools by default:

 *  gcloud : A command-line interface for managing cloud resources
 
 *  bq : Commands for interacting with Google BigQuery
 
 *  gsutil : Tools for Google Cloud Storage


# Installation
 
1. Add the Cloud SDK distribution URI as a package source:

```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```

Make sure you have apt-transport-https installed:

```
sudo apt-get install apt-transport-https ca-certificates gnupg
```

Note: Make sure you do not have duplicate entries for the cloud-sdk repo in /etc/apt/sources.list.d/google-cloud-sdk.list.

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

Run gcloud init to get started:

```
gcloud init
```


