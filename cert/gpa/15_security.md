# Cloud Identity

Cloud Identity is an Identity as a Service (IDaaS) solution that centrally manages users and groups. 

Features:
- can configure Cloud Identity to federate identities between Google and other identity providers, such as Active Directory and Azure Active Directory.
- CI gives you more control over the accounts that are used in your organization, f.e. over personal accounts, such as Gmail accounts - when you adopt Cloud Identity, you can manage access and compliance across all users in your domain via creation of a Cloud Identity account for each of your users and groups. You can then use Identity and Access Management (IAM) to manage access to Google Cloud resources for each Cloud Identity account.

# Identity and Access Management

In Identity and Access Management (IAM), access is granted through _allow policies_ (aka IAM policies). An allow policy is attached to a Google Cloud resource. Each allow policy contains a collection of _role bindings_ that associate one or more principals, such as users or service accounts, with an IAM role. 

# Identity-Aware Proxy (IAP) 

Identity-Aware Proxy (IAP) access policies use access levels and the Identity and Access Management (IAM) Conditions Framework to decide who can get access to GCP resources (Zero-Trust security model).
IAP uses the access level name to associate it with a IAP-secured app.

Depending on the policy configuration, your sensitive app can:

- Grant access to all employees if they're using a trusted corporate device from your corporate network.
- Grant access to employees in the Remote Access group if they're using a trusted corporate device with a secure password and up-to-date patch level, from any network.
- Only grant access to employees in the Privileged Access group if the URL path starts with /admin.

# Access Context Manager

Access Context Manager allows Google Cloud organization administrators to define fine-grained, attribute based access control for projects and resources in Google Cloud.
Access Context Manager lets you reduce the size of the privileged network and move to a model where endpoints do not carry ambient authority based on the network. Instead, you can grant access based on the _context_ of the request, such as device type, user identity, and more, while still checking for corporate network access when necessary.

# Access policies

An access policy is a container for all of your Access Context Manager resources, such as access levels and service perimeters.
One can create an access policy in the context of an organization and use the organization-level access policy anywhere in your organization. To delegate administration of an access policy, you can create a scoped access policy and set the scope of the policy at the folder or project level.


There are two ways to define access levels:

- basic access level: via collections of Conditions which are a group of attributes that you want to test (f.e. device type, IP address, or user identity)
- custom access levels: created using a subset of Common Expression Language - in addition to the request context used for basic access levels, one can permit requests based on data from third-party services. 


# Billing data

After you enable the data export, it takes about a day for the dataset to be populated with Cloud Billing data. 
Billing alerts are used on a per-project basis with thresholds set at 50%, 75%, 90%, and 95%.


# Encryption

Cloud Storage always encrypts your data on the server side, before it is written to disk, at no additional charge.

There are the following ways to encrypt your data when using Cloud Storage:
 
- Google-managed encryption keys: standard, Google-managed behavior

- Customer-managed encryption keys (CMEK): You can create and manage your encryption keys through Cloud Key Management Service. Customer-managed encryption keys can be stored as software keys, in an HSM cluster, or externally.

- Customer-supplied encryption keys (CSEK): You can create and manage your own encryption keys. These keys act as an additional encryption layer on top of the standard Cloud Storage encryption.

Each project has a special Cloud Storage service account called a service agent that performs encryption and decryption with customer-managed encryption keys.

NOTE: You must create the Cloud KMS key in the same location as the data you intend to encrypt.

For Customer-supplied encryption keys with Cloud EKM, you can use keys that you manage within a supported external key management partner to protect data within Google Cloud. In all cases, the key resides on the external system, and is never sent to Google.

External keys can be stored in the following external key management partner systems:

- Fortanix
- Futurex
- Thales
- Virtru

# Security Command Center

Security Command Center is Google Cloud's centralized vulnerability and threat reporting service. Security Command Center helps you strengthen your security posture by evaluating your security and data attack surface; providing asset inventory and discovery; identifying misconfigurations, vulnerabilities and threats; and helping you mitigate and remediate risks.

# PCI Data Security Standard compliance

PCI =  Payment Card Industry Data Security Standard (PCI DSS)


# Best practices

- Combine Cloud Identity and Google Workspace in a single account: Cloud Identity and Google Workspace share a common platform, you can combine access to the products in a single account.
- Use as few accounts as possible, but as many as necessary:  let your users collaborate by using Google Workspace, and to minimize administrative overhead, it's best to manage all users through a single Cloud Identity or Google Workspace account
- Use separate accounts for staging and production
- Use disjoint DNS domains among Cloud Identity and Google Workspace accounts
- Don't separate Google Workspace and Google Cloud
- Secure your external IdP when using single sign-on: Cloud Identity and Google Workspace let you set up single sign-on with your external IdP such as Active Directory, Azure Active Directory, or Okta
- Use a separate staging organization



