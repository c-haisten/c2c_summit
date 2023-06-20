# PRISMA CLOUD CODE TO CLOUD LAB EXERCISE

### Lab Introduction

Thank you for joining today's Code to Cloud hands on lab with Prisma Cloud. The lab will focus on the ways
that Prisma Cloud can help you secure applications from code to cloud.

Before we begin the lab let's start with a brief overview of the scenario to help frame the
context.

### Scenario

The Exampli Corp, a mock corporation, is rushing to finish a new mobile banking app for their customers in time for the holiday season when spending patterns spike. Up against unrealistic deadlines the development and infrastructure teams are working around the clock to get their app built, tested, and released to hit their deadlines.

Fortunately, Exampli Corp recently integrated Prisma Cloud into their development lifecycle adopting security from code to cloud. Once in production Exampli Corps operations and security teams continue to leverage Prisma Cloud to monitor and protect runtime resources, reduce the attack surface, and enforce least privilege.

Will the Exampli Corp team take the time to build a secure app? Or will the stress of completing the Bank of Anthos app in time for deadlines lead to mistakes?

Lab Overview

The lab is broken down into three sections :

Gain Visibility and Control of Cloud Assets

In this first section you will learn how to use Prisma Cloud to quickly identify Risky Assets in Exampli’s cloud footprint, automatically inform the relevant stakeholders, and remediate misconfigurations that lead to cloud breaches. 

Protect Critical Applications

The second section covers detecting and protecting containerized workloads powering Exampli Corp’s critical Bank of Anthos application using Prisma Cloud Defenders. 

Prevent Risk 

The final section includes shifting security left to help prevent vulnerable cloud infrastructure and applications from hitting runtime.



### Architecture 
The lab primarily focuses on Exampli Corp’s Banking Application known as the Bank of Anthos. The Bank of Anthos application is an example application purpose built for Google Kubernetes Engine, you can find the repo here as well as in the Resources section.

If you are unfamiliar with Kubernetes or container orchestration do not worry below you can find a HLD of the applications architecture. Additionally, in the Resources section there are provided links to learn more about Kubernetes and container orchestration. 

Prisma Cloud is integrated with mock company Exampli Corp in the following ways : 

1. Cloud Accounts are onboarded with Prisma Cloud allowing visibility into Exampli’s resources as well as their associated behavior and configuration states.

2. Critical applications in Exampli Corp’s cloud footprint are protected by Prisma Cloud defenders.

3. Code Repositories, Image Registries, and CI Tools are on-boarded into Prisma Cloud allowing for detection and remediation of misconfigurations and vulnerabilities earlier on in the development process.

### Resources

Vulnerable IaC :

In this lab we leverage some intentionally vulnerable IaC. To learn more about the projects and
its contributors visit the link below:

##### Sample Git Repositories

- [TerraGoat - Vulnerable by design Terraform Infrastructure](https://github.com/bridgecrewio/terragoat)

- [Cfngoat - Vulnerable by design Cloudformation Template](https://github.com/bridgecrewio/cfngoat)

- [CdkGoat - Vulnerable by design AWS CDK Infrastructure](https://github.com/bridgecrewio/cdkgoat)

- [BicepGoat - Vulnerable by design Bicep and ARM Infrastructure](https://github.com/bridgecrewio/bicepgoat)

- [KubernetesGoat-Vulnerable by design KubernetesCluster](https://github.com/bridgecrewio/kubernetes-goattest)

- [KustomizeGoat - Vulnerable by design Kustomize deployment](https://github.com/bridgecrewio/kustomizegoat)

- [SupplyGoat- Vulnerable by design SCA](https://github.com/bridgecrewio/supplygoat)

Bank of Anthos Application :

In this lab there is a mock banking application that is used as the application the Exampli Corp
development team is building. To learn more about the project and its contributors visit the link
below :

- https://github.com/GoogleCloudPlatform/bank-of-anthos

### Exercise

#### Gain Visibility and Control of Cloud Assets

Let's begin the lab by exploring Exampli Corp's existing assets to see if any alerts or vulnerabilities have been detected by Prisma Cloud.

1. Login to [Prisma Cloud](https://app4.prismacloud.io/auth/signin).

2. Use the credentials provided by your Instructor to authenticate.

3. Use the navigation pane on the left hand and click the **blue arrow** on the lower left
side of the UI to open up the navigation pane and move between the different
modules within Prisma Cloud.

![Alt text for image](/screenshots/find-and-fix-insecure-infrastructure-code-3.png "Optional title")

4. Next, use the navigation pane to select the **Inventory** module and then select
**Assets.**

![Alt text for image](/screenshots/discover-risky-assets-4.png "Optional title")

5. Once in the Asset Inventory view, click on the **High Risk Assets** tab. Scroll towards the bottom and take a look at the **Google Compute Engine** service. Exampli Corps uses deployed these hosts using Googles Kubernetes service called GKE to orchestrate their container workloads that run the banking applications Exampli's customers use everyday.

Note the two columns **Assets with Alerts** and **Assets with Vulnerabilities**. 

![Alt text for image](/screenshots/discover-risky-assets-5.png "Optional title")

6. Click on one of the hyperlinked numbers in the Assets with Alerts column.

![Alt text for image](/screenshots/discover-risky-assets-6.png "Optional title")

7. Now we have a view of the assets in question along with other useful metadata.

![Alt text for image](/screenshots/discover-risky-assets-7.png "Optional title")

8. Click on the first Asset Name to open a detailed side window view.

![Alt text for image](/screenshots/discover-risky-assets-8.png "Optional title")

9. From the side window view, we can explore the Alerts and Vulnerabilities associated with the asset in question. Note in the **Alerts** tab we can see the 4 High severity alerts associated with the asset. Note the different **Policy Name** categories.

We can see a couple issues right away that are a high risk such as **GCP VM instance that is internet reachable with unrestricted access (0.0.0.0/0) to Admin ports** and **GCP VM instance with data destruction permissions**.

Addtionally in the **Findings** tab we can see a long list of CVE's that need to be addressed such as CVE-2023-24538, which allows code injection via Javascript.

Please feel free to explore the other alerts and CVE's for more insight. 

![Alt text for image](/screenshots/discover-risky-assets-9.png "Optional title")

![Alt text for image](/screenshots/discover-risky-assets-10.png "Optional title")

Well done, you helped identify alerts and vulnerabilities present in Exampli's GKE cluster. Fortunately, Exampli Corps has integrated Prisma Cloud with the organization's Jira and Slack instances. With this integration configured the appropriate system and application owners have been notified and Jira Issues have been created to track their remediation automatically.

PLACE HOLDER FOR SCREEN CAPTURE

Prisma Cloud has many out of the box integrations as well as supports the use of Web Hooks to operationalize security outcomes using other third-party tools. To learn more view our public documenation for more info :

https://docs.paloaltonetworks.com/prisma/prisma-cloud/prisma-cloud-admin/configure-external-integrations-on-prisma-cloud

Now that we have learned about some vulnerabilties and misconfigurations on Exampli's GKE cluster let's learn a little bit more information about the containerized applications running on them!

### Protect Critical Applications

#### Visualize Applications and Establish Context

The first step to protecting applications is to gain and maintain comprehensive visibility of your applications and all their associated resources. If Exampli Corp does not have oversight of their cloud environment it is impossible for them to stop emerging threats, respond to incidents, or reduce vulnerabilities. In a modern multi-cloud and hybrid world, applications are broken into different environments across the clouds like VMs, containers, serverless compute architecture types. Maintaining visibility is essential to protecting applications at runtime.

Let's help Exampli Corp get a clear picture of the application running on the vulnerable GKE cluster from the previous section.  Start by taking a look at the microservices running the Bank of Anthos application using Prisma Cloud’s Radar feature.

1. The Radar feature lets you gain a bird’s eye view to monitor and understand your cloud applications. It helps you visualize the connectivity between microservices and search for vulnerabilities. Navigate to **Compute -> Radar -> Containers** and select the cluster **bank-of-anthos**.

![Alt text for image](/screenshots/maintaining-visibility-1.png "Optional title")

2. Once you have selected the correct cluster your screen should look similar the the screenshot below:

![Alt text for image](/screenshots/maintaining-visibility-2.png "Optional title")

3. Radar provides a visual depiction of the connections between containers, apps, and cluster services across your environment. It shows the ports associated with each connection, the direction of traffic flow, and internet accessibility. Take some time to play around with the Radar feature and think through the following questions:

What type of information can you learn by clicking on the nodes?
How does this visibility help you protect your applications?

4. Let’s dive deeper into the **frontend:v0.5.5** microservice. Click on the associated container. Your screen should look similar to the screenshot below:

![Alt text for image](/screenshots/preventing-attacks-3.png "Optional title")

5. Take some time to review the **Vulnerabilities** page.

![Alt text for image](/screenshots/preventing-attacks-4.png "Optional title")

6. Of the identified OS vulnerabilities, which one has the highest CVE? Do all the identified vulnerabilities contain a fix?

7. Take a look at the **Layers** tab to view the dockerfile that built this image and find where vulnerabilities were introduced.

![Alt text for image](/screenshots/preventing-attacks-6.png "Optional title")

8. Find the file that added the most severe vulnerabilities and open it up by clicking on it to learn more about specifically what CVEs were added. Your screen should look similar to the screen capture below :

PLACEHOLDER FOR SCREEN CAPTURE

##### Investigate for Incidents

Exampli Corp has adopted Prisma Cloud and maintained consistent visibility on their applications. But seeing your resources does not make them immune to incidents during runtime. By using incident explorer the Exampli Corp Security team gets real-time detection and analysis of all potential threats that violate their security policies.

Lets begin by looking at the AI powered modeling Prisma Cloud provides. Process level detail and critical forensic information are modeled to give both visibility and aid in investigations for indicators of compromise. Spend some time reviewing the forensic information Prisma Cloud provides, feel free to look back in time.

1. Let's begin by navigating to **Monitor -> Runtime -> Incident Explorer**. On this screen we can view suspicious events collected get a feel for the types of incidents being reported.

![Alt text for image](/screenshots/investigate-incidents-at-runtime-1.png "Optional title")

2. Next, click on **Container Models** tab. Here we can view all of our containers and take a deeper look at the forensics to see if there are any security concerns that slipped through the cracks.

![Alt text for image](/screenshots/investigate-incidents-at-runtime-2.png "Optional title")

3. We know that Exampli Corp is getting ready to release the new Bank of Anthos App, so let's search **“gcr.io/bank-of-anthos-ci/frontend:v0.5.5”** in the filter box to see how the containers are looking.

![Alt text for image](/screenshots/investigate-incidents-at-runtime-3.png "Optional title")

4. Great! Now let's click on the microscope icon to take a look at the digital forensics

![Alt text for image](/screenshots/investigate-incidents-at-runtime-4.png "Optional title")

5. Here you can see all the events displayed. Let’s take a deeper look at these events to get some details and see what has been happening with our bank-of-anthos-frontend.

![Alt text for image](/screenshots/investigate-incidents-at-runtime-5.png "Optional title")

6. Spend some time looking through the events and answer the following questions:

What information can be gathered from the events?
What types of events are being displayed?
Should we be concerned about any of these events? What action if any should be taken?

#### Preventing attacks

One of the most important parts of securing the banking app during runtime is preventing attacks. Like actual banks that have preventative capabilities such as safes and security guards, Exampli Corp uses Prisma Cloud defenders to prevent attacks from happening in real-time when all  prior controls fail.

1. Navigate back to **Radar -> Containers**

2. Ensure you are on the default namespace for the bank-of-anthos cluster.  Your screen should look similar to the screenshot below:

![Alt text for image](/screenshots/preventing-attacks-2.png "Optional title")

7. In addition to image scanning, runtime visibility and protection; the Prisma Cloud defender also provides Web Application Firewall and API Security

8. To take a look let’s navigate back to **Radars -> Containers** and ensure you are viewing the **bank-of-anthos** app.

![Alt text for image](/screenshots/preventing-attacks-8.png "Optional title")

9. The green firewall log indicates that the front-end microservice is protected by Prisma Cloud.

10. Administrators can define rules to provide web application and api security capabilities to protect web applications. Prisma Cloud supports VM Hosts, Containers, Application Embedded, and function deployment architectures.

![Alt text for image](/screenshots/preventing-attacks-10.png "Optional title")

11. Administrators can define Custom Rules that provide Virtual Patching capabilities to protect against attacks exploiting CVE’s that have not yet been patched. For example check out the log4j blog where you can find more information about custom rules that were created to protect our customers. [Link](https://www.paloaltonetworks.com/blog/prisma-cloud/log-4-shell-vulnerability/)

12. Take a look at some of the attacks the defender has prevented by navigating to **Monitor -> Events -> WAAS for Containers**.

![Alt text for image](/screenshots/preventing-attacks-12.png "Optional title")

Thankfully no major events have occurred yet on the accidentally deployed banking app, however we know Exampli Corp has a bad track record for not securing their apps. Let’s look at some historical data from last holiday season.

Click in the filter bar and select **Date**. Enter Dec 1, 2021 for the **From** date and Jan 31, 2022 for the **To** date.

13. Let’s dive into one of the **OS Command Injection** attacks. Click the **total** value to view the events.

![Alt text for image](/screenshots/preventing-attacks-13.png "Optional title")

14. Select one of the entries and answer the questions below:

What was the result of the attack?
Was it blocked?
What was the http method that was used in the attack?
What container image was attacked?

#### Prevent Future Risk 

Let's begin this next exercise by exploring the power of shifting security left with Infrastructure as code
scanning. As infrastructure is being defined as code, security must be integrated with the
tools developers use.

Fortunately, Prisma Cloud can be integrated into the development pipeline and tools
developers use to secure IaC. With Bridgecrew from Prisma Cloud you can begin leveraging
the open source tool Checkov for free.

To learn more visit: https://bridgecrew.io/checkov/

The first place we will begin is with investigating Exampli Corp's infrastructure code in the company's GitHub repository. 

Follow the steps below :

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

### Exercise

Let's begin by exploring the power of shifting security left with Infrastructure as code scanning. As infrastructure is being defined as code security must be integrated with the tools developers use. In this exercise, we will take a look at some IaC templates within Exampli Corp’s repository.

### Software Composition Analysis:

1. Login to [Prisma Cloud](https://app4.prismacloud.io/auth/signin).

2. Use the credentials provided by your Instructor to authenticate.

3. Use the navigation pane on the left hand and click the **blue arrow** on the lower left side of the UI to open up the navigation pane and move between the different modules within Prisma Cloud.

![Alt text for image](/screenshots/software-composition-analysis-3.png "Optional title")

4. Next, use the navigation pane to select the **Code Security** module and then select **Projects**.

![Alt text for image](/screenshots/software-composition-analysis-4.png "Optional title")

5. Ensure that you select the **c-haisten/bank-of-anthos** repository.

![Alt text for image](/screenshots/software-composition-analysis-5.png "Optional title")

6. Once you have selected the correct repository, it will show you all the security incidents found in the code of that branch. Let’s check to see if there are any serious vulnerabilities by filtering **Category -> Vulnerabilities** and **Severity -> High.**

![Alt text for image](/screenshots/software-composition-analysis-6.png "Optional title")

7. Next, we will select a CVE ID to investigate the vulnerability and determine what next steps to take. Prisma Cloud gives us several options for how to interact with the vulnerability.

Administrators can click suppress, or fix. The suppress button allows you to dismiss all incidents that fall under that specific policy violation. The fix button will allow you to remediate the vulnerability via a bump fix. Here we can see that the vulnerability can be fixed easily by bumping from v2.7.4 to 2.7.5.

**Suppress and Fix requires increased RBAC that is not available in this lab**

8. You can also click the “Details” or “Errors” tab on the right side of the page to get some more information on the dangers of the specific vulnerability.

![Alt text for image](/screenshots/software-composition-analysis-8a.png "Optional title")

![Alt text for image](/screenshots/software-composition-analysis-8b.png "Optional title")

9. This CVE has a CVSS score of 7.5! It makes Exampli Corp’s Bank of Anthos app vulnerable to a request smuggling attack.

On this page Exampli Corp developers can gain context on the CVE and click on the link to NIST providing them with all the details and documentation regarding the vulnerability.

10. In Prisma Cloud licenses are scanned in parallel to vulnerability scanning. This means that Exampli Corp developers can make sure they are remaining compliant when working with open source packages. Adjust your filter to **Category -> Licenses** and
let's see if there are any licensing issues in the Bank of Anthos Repo.

![Alt text for image](/screenshots/software-composition-analysis-10.png "Optional title")

11. Now, let's scroll down and look at some of the licensing issues identified by Prisma Cloud.

![Alt text for image](/screenshots/software-composition-analysis-11.png "Optional title")

12. By Clicking on the license type Prisma Cloud provides developers with critical information and identifies each policy violation as a single error.

![Alt text for image](/screenshots/software-composition-analysis-12.png "Optional title")

Now that we have found some vulnerabilities and licensing violations in our Application code, let's take a look at how Prisma Cloud can give us visibility to the package manager files that comprise applications by taking a look at the supply chain and software composition analysis (SCA).

### Supply Chain Security

1. Use the navigation pane on the left hand and click the **blue arrow** on the lower left side of the UI to open up the navigation pane and move between the different modules within Prisma Cloud.

![Alt text for image](/screenshots/supply-chain-security-1.png "Optional title")

2. Next, use the navigation pane to select the Code Security module and select Supply Chain.

![Alt text for image](/screenshots/supply-chain-security-2a.png "Optional title")

![Alt text for image](/screenshots/supply-chain-security-2b.png "Optional title")

3. Use the Supply Chain Graph to view the relationships between different IaC templates and the types of infrastructure and services they provision. Be sure to select the bank-of-anthos repository from the drop-down window in the filters at the top.

![Alt text for image](/screenshots/supply-chain-security-3.png "Optional title")

4. Next, take a deeper look at the **requirements.txt** file. Feel free to test the filters on the left side of the UI and search bar at the top to quickly locate templates.

![Alt text for image](/screenshots/supply-chain-security-4.png "Optional title")

5. Once you have found the **requirements.txt** file click on the first package that is unpacked, **cryptography: 38.0.1**. Notice on the right side of the UI there is additional information about this resource.

Here we can quickly identify valuable information like the package version, license type and privileges.

6. Your screen should look similar to the screenshot below.

![Alt text for image](/screenshots/supply-chain-security-6.png "Optional title")

7. Next, click on the **Errors** tab to investigate policy violations and vulnerabilities.

![Alt text for image](/screenshots/supply-chain-security-7.png "Optional title")

8. How many Vulnerabilities are associated with the **cryptography: 38.0.1** package? Read the **Details** section to discover more about the CVE and confirm the **Fix Version**. Are there other packages with a higher CVSS score?

Now that you have helped to protect the Bank of Anthos banking app Software Supply Chain, let’s take a look at the recent activity of the Exampli Corp developers.

## Summary \ Resources

The Prisma Cloud team here at Palo Alto sincerely hopes you enjoyed this workshop. Today, organizations need a growing set of capabilities to secure modern cloud applications. Implementing AppSec into your development and security workflows with Prisma Cloud can provide the increased speed and agility your organization needs to implement zero trust throughout the entire development lifecycle. Check out the resources below for more information!

[Live Workshops](https://bridgecrew.io/resource/workshops/)

[DevSecTalks Podcast](https://www.paloaltonetworks.com/devsectalks/)

[Supply Chain Security](https://bridgecrew.io/software-supply-chain-security/)

[Cloud DevSecOps](https://bridgecrew.io/cloud-devsecops/)

[Learn More](https://www.paloaltonetworks.com/prisma/cloud)
