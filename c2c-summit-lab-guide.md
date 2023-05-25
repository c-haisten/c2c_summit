# PRISMA CLOUD IAC SECURITY BOOTCAMP


### Lab Introduction

Thank you for joining today’s Infrastructure as Code (IaC) camp. The lab will focus on the ways
that Prisma Cloud can help you secure IaC from the code to runtime.

Before we begin the lab let's start with a brief overview of the scenario to help frame the
context.

### Scenario

The Exampli Corp, a mock corporation, product group is rushing to finish a banking app for
their customer Bank of Anthos in time for the holiday season when spending patterns spike.
Up against unrealistic deadlines the development and infrastructure teams are working
around the clock to get their app built, tested, and released to hit their deadlines.

Fortunately, Exampli Corp recently integrated Prisma Cloud into their development lifecycle
adopting shift left security from code to cloud. Once in production Exampli Corps operations
and security teams continue to leverage Prisma Cloud to monitor and protect runtime
resources, reduce the attack surface, and enforce least privilege.

Will The Exampli Corp team take the time to build a secure app? Or will the stress of
completing the Bank of Anthos app in time for the holiday season lead to mistakes?

### Resources

Vulnerable IaC :

In this lab we leverage some intentionally vulnerable IaC. To learn more about the project and
its contributors visit the link below:

- https://github.com/bridgecrewio/terragoat

Bank of Anthos Application :

In this lab there is a mock banking application that is used as the application the Exampli Corp
development team is building. To learn more about the project and its contributors visit the link
below :

- https://github.com/GoogleCloudPlatform/bank-of-anthos

### Exercise

Let's begin by exploring the power of shifting security left with Infrastructure as code
scanning. As infrastructure is being defined as code, security must be integrated with the
tools developers use.

Fortunately, Prisma Cloud can be integrated into the development pipeline and tools
developers use to secure IaC. With Bridgecrew from Prisma Cloud you can begin leveraging
the open source tool Checkov for free.

To learn more visit: https://bridgecrew.io/checkov/

In this exercise, we will take a look at some IaC templates within Exampli Corp’s repository.

#### Find and Fix Insecure Infrastructure Code

1. Login to [Prisma Cloud](https://app4.prismacloud.io/auth/signin).

2. Use the credentials provided by your Instructor to authenticate.

3. Use the navigation pane on the left hand and click the **blue arrow** on the lower left
side of the UI to open up the navigation pane and move between the different
modules within Prisma Cloud.

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-3.png "Optional title")

4. Next, use the navigation pane to select the **Code Security** module and then select
**Projects.**

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-4.png "Optional title")

5. Use the search function to select the **umman-manda/Exampli** repository

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-5.png "Optional title")

6. Once you have selected the correct repository, filter by “High” severity and let's take a
look at some troubling misconfigurations found in **/terraform/gcp**

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-6.png "Optional title")

7. Now that we have filtered by severity, explore the different misconfigurations and
identify the one that says **_GCP SQL database is publicly accessible._**

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-7.png "Optional title")

8. Click the Policy details button ![Alt text for image](/screenshots/policy-details-button.png "Optional title") to view guidelines on how to resolve the
misconfiguration. Below is an example of how the policy box will look and the type of information you will see.

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-8.png "Optional title")

9. Reviewing the guidelines we can see the description of this misconfiguration, its potential risk, and how to remediate at build time and runtime.

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-9.png "Optional title")

10. Now navigate back to the Prisma Cloud platform. Click on the associated SQL database resource and view the helpful information about the misconfiguration such as its history, errors, details, and traceability. It also will give you an option to view the resource in its VCS.

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-10.png "Optional title")

11. This misconfiguration makes the application vulnerable to attacks that could reveal sensitive data such as user credentials and financial information. This error could lead to big problems for the Bank of Anthos. By using Prisma Cloud code security, the Exampli Corp security team can avoid costly mistakes and protect the integrity of the application.

**There are many other examples to explore in the Exampli repo, feel free to use the filters and search bar to explore additional resources and findings.**

#### Generating Pull Requests

1. Depending on the nature of a misconfiguration, Prisma Cloud can provide single click remediation. This capability creates a pull request that is sent to the version control system where the IaC template is stored.

Looking at the same **big_data.tf** template, there is another HIGH severity misconfiguration for lack of SSL on SQL database connections.

![Alt text for image](/screenshots/generating-pull-requests-1.png "Optional title")

2. This resource is a fully managed relational database service for MySQL, PostgreSQL and SQL Server. It is recommended to enable SSL but we can see it is not configured in this resource.

**The Suppress and Fix capability requires increased RBAC capabilities and is not available to read-only users**

3. Pull requests have been created previously for various findings. To access the VCS associated with this IaC resource click the git link. We can also see the developer who committed the last change associated with this IaC resource.

![Alt text for image](/screenshots/generating-pull-requests-3.png "Optional title")

4. After clicking the git link we see the last commit associated with the **big_data.tf** IaC template. Highlighted is the **ip_configuration** parameterwhich does not include the SSL enablement.

![Alt text for image](/screenshots/generating-pull-requests-4.png "Optional title")

5. To view the fix generated by Prisma Cloud let’s take a look at the pull requests tab at the top of the github UI.

![Alt text for image](/screenshots/generating-pull-requests-5.png "Optional title")

6. Once on the Pull Requests view take a look at [PR #34](https://github.com/umman-manda/Exampli/pull/34) and its associated [commit](https://github.com/umman-manda/Exampli/pull/34/commits/d1360871240084945b5d7d09d087e195241b9e22). Examining the changes, the ‘require_ssl = true’ setting has been added to the big_data.tf template.

![Alt text for image](/screenshots/generating-pull-requests-6.png "Optional title")

#### Identify Exposed Secrets

Prisma Cloud also gives administrators the ability to detect exposed secrets. This is a powerful
tool that can be utilized from the Project page.

1. Clear out all of your filters and select **“Secrets”** under **category**. Now you can view any exposed secrets in your repository. Let's take a look at **/terraform/aws**

![Alt text for image](/screenshots/identify-exposed-secrets-1.png "Optional title")

2. Once you click on **/terraform/aws** you will see that there are several misconfigurations that expose secrets. Take a closer look at the **providers.tf** template. Note that you may have to search for the file if the filters don’t work due to concurrent users within the lab. Here we can see a variety of issues with this terraform template. The first being hard coded plain text secrets.

![Alt text for image](/screenshots/identify-exposed-secrets-2.png "Optional title")

3. When accessing AWS programmatically users can select to use an access key to verify their identity, and the identity of their applications. An access key consists of an access key ID and a secret access key. Anyone with an access key has the same level of access to AWS resources.

**We recommend you protect access keys and keep them private. Specifically, do not store hard coded keys and secrets in infrastructure such as code, or other version-controlled configuration settings.**

![Alt text for image](/screenshots/identify-exposed-secrets-3.png "Optional title")

**According to the [2022 Verizon Data Breach Report](https://www.verizon.com/business/resources/T14f/reports/dbir/2022-data-breach-investigations-report-dbir.pdf), stolen access credentials are used in 80% of successful data breaches.**

Fortunately, Prisma Cloud makes it easy for anyone to quickly determine how the exposed secret can harm your organization and what steps must be taken. These exposed AWS access credentials could let an unauthorized attacker access the Exampli Corp AWS account.

#### Investigating the Supply Chain

1. Use the navigation pane on the left hand and click the **blue arrow** on the lower left side of the UI to open up the navigation pane and move between the different modules within Prisma Cloud.

![Alt text for image](/screenshots/investigating-the-supply-chain-1.png "Optional title")

2. Next, use the navigation pane to select the **Code Security** module and select **Supply Chain**.

![Alt text for image](/screenshots/investigating-the-supply-chain-2a.png "Optional title")

![Alt text for image](/screenshots/investigating-the-supply-chain-2b.png "Optional title")

3. Use the Supply Chain Graph to view the relationships between different IaC templates and the types of infrastructure and services they provision.

Be sure to select the **umman-manda/Exampli** repository from the drop-down window in the filters at the top.

![Alt text for image](/screenshots/investigating-the-supply-chain-3.png "Optional title")

4. Next, take a deeper look at the **gke.tf** template. This gke.tf template defines the Kubernetes deployment that Exampli is looking to push to production.

Feel free to test the filters on the left side of the UI and search bar at the top to quickly locate templates.

![Alt text for image](/screenshots/investigating-the-supply-chain-4a.png "Optional title")

![Alt text for image](/screenshots/investigating-the-supply-chain-4b.png "Optional title")

![Alt text for image](/screenshots/investigating-the-supply-chain-4c.png "Optional title")

5. Once you have found the **gke.tf** template click on the first resource **google_container_cluster.workload_cluster**. Notice on the right side of the UI there is additional information about this resource

6. Your screen should look similar to the screenshot below.

![Alt text for image](/screenshots/investigating-the-supply-chain-6.png "Optional title")

7. Here we can quickly identify valuable information like related resources that this resource may depend on. The amount of infrastructure dependencies that modern software relies on is only increasing.

Thankfully, Prisma Cloud allows the Exampli Co. security team to scan all their IaC and remediate findings with single click Pull Requests to mitigate any vulnerabilities found.

8. You can easily view the original scan by tabbing over and seeing where errors were identified.

![Alt text for image](/screenshots/investigating-the-supply-chain-8.png "Optional title")

9. To easily fix these issues, authorized administrators can initiate a pull request with a single click from the Prisma Cloud UI. **This feature is not available for read-only users.**

![Alt text for image](/screenshots/investigating-the-supply-chain-9.png "Optional title")

10. We have submitted a pull request in advance that can be viewed at the following [link](https://github.com/c-haisten/ZT-Road-Show/pulls).

![Alt text for image](/screenshots/investigating-the-supply-chain-10.png "Optional title")

11. Dig into the PR and take a look at the requested changes to the **gke.tf** template.

![Alt text for image](/screenshots/investigating-the-supply-chain-11.png "Optional title")

12. Feel free to explore other templates and PR’s that have been submitted.

#### Cloud Security Posture Management

Cloud-native applications allow organizations to build and run scalable applications with great agility and resilience. However, they also present unique security challenges. Ensuring applications and services are secure at runtime is a core responsibility for security teams.

In this section, you will explore some use cases for securing GKE resources and services.

1. Let’s start with taking a look at the **Asset Inventory** page to get an idea of the scale of resources that are being monitored by this particular Prisma Cloud tenant.

2. Begin by using the navigation panel on the left hand side of the UI.

3. Ensure to use the filter and select **GCP** as the CloudType.

![Alt text for image](/screenshots/cloud-security-posture-management-3.png "Optional title")

4. The **Asset Inventory** can be a helpful place to get a high level view on cloud assets and their compliance with defined policies. Without constant visibility of what is being deployed in your cloud footprint, you cannot begin to secure it.

5. Your screen should look similar to the one below:

![Alt text for image](/screenshots/cloud-security-posture-management-5.png "Optional title")

6. We know the Exampli Corp team is using Kubernetes. Let’s investigate further by clicking on the **Google Kubernetes Engine**.

![Alt text for image](/screenshots/cloud-security-posture-management-6a.png "Optional title")

![Alt text for image](/screenshots/cloud-security-posture-management-6b.png "Optional title")

7. Here we can get a filtered view of the risky GKE clusters and some helpful information including the resource ID and Account ID, as well as where this resource is located. Let’s take a deeper look into **bank-of-anthos** and see this resource's history.

8. Use the column on the left to quickly view Config and Alert information for this GKE cluster.

![Alt text for image](/screenshots/cloud-security-posture-management-8.png "Optional title")

9. Let's take a look at the raw config for this resource by clicking on the **code** icon.

![Alt text for image](/screenshots/cloud-security-posture-management-9.png "Optional title")

10. Here we can see the raw configuration for this GKE cluster. Do you see anything that might present risks?

![Alt text for image](/screenshots/cloud-security-posture-management-10.png "Optional title")

11. To learn more let's dig into the alerts associated with this GKE cluster and see if we can find out how to remediate any issues. Close out the resource config and click on the red exclamation point.

![Alt text for image](/screenshots/cloud-security-posture-management-11.png "Optional title")

12. Your screen should be similar to the screenshot below.

![Alt text for image](/screenshots/cloud-security-posture-management-12.png "Optional title")

13. Click on the number next to the **GCP Kubernetes cluster intra-node visibility disabled** alert in the **Alert Count** column.

14. Your screen should look similar to the screen capture below.

![Alt text for image](/screenshots/cloud-security-posture-management-14.png "Optional title")

15. Here we see the bank-of-anthos resource is exposing east-west network traffic. If we click on the **Recommendation** tab we can see the recommended steps to resolve this issue.

![Alt text for image](/screenshots/cloud-security-posture-management-15.png "Optional title")

16. We can also choose to remediate this alert by clicking on the **remediate** option under the actions column. (This step requires admin access via a free trial or existing Prisma
Cloud account)

![Alt text for image](/screenshots/cloud-security-posture-management-16a.png "Optional title")

![Alt text for image](/screenshots/cloud-security-posture-management-16b.png "Optional title")

17. In addition to the asset inventory and leading cloud management database Prisma Cloud makes compliance incredibly easy. Let’s move on to the next exercise on compliance...

#### Compliance in the Cloud

Many organizations struggle maintaining a grasp on compliance in the cloud. So far we can inspected Exampli’s IaC and found some troubling trends. Additionally, at runtime many assets are still vulnerable due to risky configurations and careless development practices.

With multiple upcoming compliance audits bearing down on Exampli’s CISO’s calendar it's time to begin reporting on what is in a failed state and what needs to be done to get the
organization’s cloud footprint compliant.

1. Start by using the navigation bar and locate the **Compliance** module. Select **Overview**.

![Alt text for image](/screenshots/compliance-in-the-cloud-1.png "Optional title")

2. Your screen should look similar to the screen capture below:

![Alt text for image](/screenshots/compliance-in-the-cloud-2.png "Optional title")

3. This page provides a high level view of all the unique assets and their associated adherence to various compliance frameworks and policies.

When Prisma Cloud detects a violation of a policy it generates an alert.

4. Leverage the filters to adjust the results and select GCP.

![Alt text for image](/screenshots/compliance-in-the-cloud-4.png "Optional title")

5. Your screen should look similar to the screen capture below :

![Alt text for image](/screenshots/compliance-in-the-cloud-5.png "Optional title")

6. We can now see Exampli’s associated assets and their compliance posture. Looking at the trend graph it is easy to see how Exampli Corp compliance has changed over time.

![Alt text for image](/screenshots/compliance-in-the-cloud-6.png "Optional title")

7. Leverage the filters and select the compliance framework **CIS v1.1.0 (GCP)**.

![Alt text for image](/screenshots/compliance-in-the-cloud-7a.png "Optional title")

![Alt text for image](/screenshots/compliance-in-the-cloud-7b.png "Optional title")

8. This provides a focused view of Exampli’s GCP footprint and its adherence to this CIS
Standard.

To view the associated policies with CIS click the **value** under the policies tab. Your screen should look similar to the screen capture below :

![Alt text for image](/screenshots/compliance-in-the-cloud-8.png "Optional title")

9. To learn more about a particular policy in Prisma Cloud you can leverage the table at the bottom of the UI and read the description to gain more context.

![Alt text for image](/screenshots/compliance-in-the-cloud-9.png "Optional title")

10. Explore the different policies and their associated impact then navigate back to the compliance tab in your browser or click on the compliance module on the left hand side of the UI. Once on the **Compliance** page select the **Reports** tab.

Your screen should look similar to the screen capture below :

![Alt text for image](/screenshots/compliance-in-the-cloud-10.png "Optional title")

On this page administrators of Prisma Cloud can generate compliance reports that provide a summary of the information that is presented in the Prisma Cloud UI. This can be very helpful for sharing with auditors and leaders.

Please explore some of the generated reports and their information.

In this lab we covered various topics around IaC security and CSPM but there are many additional features and capabilities within Prisma Cloud. Feel free to explore the UI and investigate different asset types.

## Summary \ Resources

The Prisma Cloud team here at Palo Alto sincerely hopes you enjoyed this workshop. Today, organizations need a growing set of capabilities to secure modern cloud applications. Implementing DevSecOps into your development and security workflows with Prisma Cloud can provide the increased speed and agility your organization needs to implement zero trust throughout the entire development lifecycle. Check out the resources below for more information!

[Live Workshops](https://bridgecrew.io/resource/workshops/)

[DevSecTalks Podcast](https://www.paloaltonetworks.com/devsectalks/)

[Supply Chain Security](https://bridgecrew.io/software-supply-chain-security/)

[Cloud DevSecOps](https://bridgecrew.io/cloud-devsecops/)

[Learn More](https://www.paloaltonetworks.com/prisma/cloud)

##### Sample Git Repositories

[TerraGoat - Vulnerable by design Terraform Infrastructure](https://github.com/bridgecrewio/terragoat)

[Cfngoat - Vulnerable by design Cloudformation Template](https://github.com/bridgecrewio/cfngoat)

[CdkGoat - Vulnerable by design AWS CDK Infrastructure](https://github.com/bridgecrewio/cdkgoat)

[BicepGoat - Vulnerable by design Bicep and ARM Infrastructure](https://github.com/bridgecrewio/bicepgoat)

[KubernetesGoat-Vulnerable by design KubernetesCluster](https://github.com/bridgecrewio/kubernetes-goattest)

[KustomizeGoat - Vulnerable by design Kustomize deployment](https://github.com/bridgecrewio/kustomizegoat)

[SupplyGoat- Vulnerable by design SCA](https://github.com/bridgecrewio/supplygoat)
