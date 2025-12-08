---
title: "Blog 3"
date: 2024-01-01T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# How Launchpad from Pega Enables Secure SaaS Extensibility with AWS Lambda

**Authors:** Anton Aleksandrov, Giridhar Ramadhenu, Rajesh Kumar Maram, and Anubhav Sharma  
**Published:** 30 MAY 2025  
**Categories:** AWS Lambda, Best Practices, Customer Solutions, Serverless, Technical How-to

Large organizations increasingly adopt software as a service (SaaS) solutions to focus on business priorities, reduce infrastructure management overhead, and optimize costs. These organizations expect SaaS vendors to provide customizability facilities for tailoring the solution behavior according to their needs. Although traditional approaches like feature flags and webhooks offer some flexibility, they often fall short of providing a high degree of customizability. A new emerging pattern in this space is **tenant-supplied custom code execution**, which allows tenants to inject their own code into specific workflow points, enabling deep customization while preserving the core SaaS solutions' integrity and security.

In this post, we share how **Pegasystems (Pega)** built **Launchpad**, its new SaaS development platform, to solve a core challenge in multi-tenant environments: enabling secure customer customization. By running tenant code in isolated environments with AWS Lambda, Launchpad offers its customers a secure, scalable foundation, eliminating the need for bespoke code customizations.

---

## Solution Overview

Launchpad, which is built on AWS, is an end-to-end platform on which software providers can build, launch, and operate workflow-centric B2B SaaS applications and AI solutions. It provides a managed, secure, scalable cloud environment for hosting multi-tenant applications and data. It accelerates the build experience with generative AI-powered low code tools, prebuilt capabilities, and subscriber-level configuration. Being a multi-tenant platform at its core, Launchpad had to maintain stringent isolation across tenants in its architecture.

One of the requirements Launchpad had was to allow their tenants to augment the workflows natively by providing custom code. Some common scenarios included:
- Communicating with external systems with proprietary non-industry-standard protocols
- Reuse of existing business logic
- SDK-based custom code development

The solution necessitated the ability for tenants to provide custom code that would implement the required business logic, which Launchpad would be executing. This required architecting a secure runtime environment for custom code execution that maintains the highest degree of cross-tenant isolation within the multi-tenant architecture, at the same time allowing sufficient access to platform APIs and services. It was essential to build an architecture that would decouple the environment running tenant code from the core SaaS platform.

---

## Architecting the Solution Topology

To achieve the required high level of compute isolation for running code provided by different tenants, Launchpad has adopted **Lambda functions** in its architecture as the secure ephemeral compute environment. Each untrusted code snippet provided by tenants is bootstrapped as a stand-alone Lambda function, with strong **Firecracker-based isolation** across different functions and execution environments addressing Launchpad's requirements. This isolation provides:
- Dedicated resources
- Customizable access permissions
- Independent monitoring and operations
- Automatic scaling for each function
- Complete separation from other functions and their execution environments

With Lambda being a serverless compute service, adopting it for the Launchpad architecture yielded several significant benefits:

**Major Business Benefit:** Tenants could implement thousands of custom workflow augmentations on their own simply by providing code snippets, instead of the Launchpad engineering team being responsible for implementing them in the core platform code.

**Technical Benefits:**
- **Managed runtimes** – AWS handles patching and updating the underlying infrastructure, operating system, and runtimes for customers, reducing the potential attack surface
- **Fine-grained permissions** – Each function can have its own set of access policies to tightly control what resources and actions it can access
- **No need to pre-provision and pay for overprovisioned capacity** – Lambda functions scale up and down automatically based on traffic patterns
- **Built-in monitoring** – Lambda functions emit detailed metrics, logs, and traces through Amazon CloudWatch and AWS X-Ray out of the box, making it straightforward to monitor tenant code execution

To further reduce risks, Launchpad runs these Lambda functions with untrusted code in a **dedicated AWS account**, separated from the core SaaS platform account. When end-users create a new function in the Launchpad authoring portal, they upload their code and specify the code handler to be executed during the invocation. Users can also map function input and output to Launchpad fields for further processing to enable an even higher degree of customizability and integration.

The multi-tenant authoring service is a Control Plane component that runs as a microservice on the **Amazon Elastic Kubernetes Service (Amazon EKS)** cluster and uses the Lambda API for function lifecycle management. After a function resource is created, it can be used for further invocations.

---

## Runtime Architecture

At runtime, when Launchpad needs to invoke a function, it calls the Lambda Invoke API. Before the function is invoked, the multi-tenant runtime service performs a **tenancy check** to make sure the request is coming from an authorized tenant by doing the token validation. After a successful validation, the service invokes the required Lambda function. To invoke functions hosted in a different AWS account, the multi-tenant runtime service uses an **AWS Identity and Access Management (IAM) role** to assume the required permissions and invokes the Lambda service using the AWS SDK.

**The workflow consists of the following steps:**

1. An incoming user request reaches the application gateway service
2. The application gateway authenticates the request using the tenancy security service
3. After it's authenticated, the request is forwarded to the multi-tenant runtime service
4. The multi-tenant runtime service validates the supplied token and performs a tenancy check (ensuring tenants can only invoke own functions they have permissions for)
5. The multi-tenant runtime service pod assumes the IAM role required for invoking the tenant-specific Lambda function in a different AWS account
6. The multi-tenant runtime service pod invokes the required Lambda function

Invoking the platform API from custom code is as straightforward as connecting to any external API. The custom code can authenticate with the platform using **OAuth2**. To facilitate the authentication, the developer can pass along the credentials as input parameters to the function from the core platform. Then the developer can create a corresponding record (isolated by tenant) in the platform that stores the credentials per function, and pass credentials as input parameters during invocation.

---

## Distributed Architecture Observability

Operating a distributed architecture that runs untrusted code across multiple AWS accounts requires a comprehensive observability strategy. Launchpad's approach combines centralized logging and monitoring with cross-account aggregation to provide a unified operational view of the platform.

The monitoring architecture uses **CloudWatch Metrics** to observe the Lambda functions, aggregating them through a centralized observability layer. This setup empowers platform operators to correlate Lambda function metrics with the core platform services running on Amazon EKS. Launchpad also collects per-function telemetry like:
- Function invocations
- Error rates
- Execution time

These telemetry dimensions enable both a platform-wide and tenant-specific monitoring perspective.

For logging and troubleshooting, Launchpad implements a unified logging pipeline that aggregates Lambda function logs with application gateway and runtime service logs. Each request flowing through the system carries a **correlation ID**, so operators can trace execution paths across the core SaaS services and into the tenant functions running in the AWS account running tenant Lambda functions.

With this multi-layer observability architecture, Launchpad can maintain operational excellence while running tenant code securely at scale. Regular operational reviews drive continuous improvements in monitoring coverage and incident response procedures. Having per-tenant Lambda functions make it possible for Launchpad to use **tenant-specific cost allocation tags**, further empowering them to understand the cost footprint of running tenant custom code.

---

## Best Practices

When building a SaaS solution, maintaining a unified core code base is essential for scalability and manageability. Implementing per-tenant variations within the core platform code can lead to maintenance complexity and technical debt. Instead, architect your SaaS solution to have **extension points**, which allow your tenants to inject their custom code at specific points in the workflow, enabling customization without compromising the platform's maintainability. This pattern makes sure the core SaaS platform remains clean and standardized while offering the flexibility that customers demand.

**Additional best practices include:**

1. **Use separate accounts** for running Lambda functions with untrusted tenant-provided code to ensure it's isolated from your core SaaS platform code
2. **Grant absolute minimum required access permissions** to the execution role assigned to the function. The custom code running within the execution environment gets permissions defined in the execution role when making requests to AWS API endpoints. If the function doesn't need to reach out to AWS API endpoints, remove all permissions from the execution role and add an explicit AWSDenyAll policy
3. **Use separate Lambda functions** for each code snippet and each tenant. This will provide the highest degree of cross-tenant isolation. Resources are not reused across different functions and execution environments
4. **Use Lambda layers** in case you need to add a layer of vendor-provided code in order to keep it separated from the untrusted tenant-provided code
5. **Implement additional security controls**, such as using Amazon Virtual Private Cloud (Amazon VPC) constructs to restrict network access and VPC Flow Logs for network activity monitoring

---

## Conclusion

The implementation of a secure untrusted code execution environment within SaaS platforms addresses a critical need for tenant customization while maintaining architectural integrity. Lambda offers a built-in isolation model, fine-grained security controls, and serverless scalability, so SaaS providers such as Launchpad can address the requirements of executing tenant-provided code in a multi-tenant environment and offer robust customization capabilities while maintaining strict security boundaries and operational efficiency. 

This architectural pattern enables providers to focus on core platform development while confidently supporting tenant-specific workflows through the secure and scalable Lambda execution environment.

**To learn more:**
- Security Overview of AWS Lambda white paper
- Serverless architectural patterns at Serverlessland.com

---

## About the Authors

**Anton Aleksandrov** - Principal Solutions Architect for AWS Serverless and Event-Driven architectures. With over two decades of hands-on engineering and architecture experience, Anton works with major ISV and SaaS customers to design highly scalable, innovative, and secure cloud solutions.

**Giridhar Ramadhenu** - Seasoned software architect with over 2 decades of expertise, specializing in microservices, event-driven, and layered architectures. As a Fellow Software Architect for Launchpad at Pegasystems and an influential member of the Architecture Guild, Giridhar plays a pivotal role in shaping the architecture of various products.

**Rajesh Kumar Maram** - Senior Principal Software Engineer for Launchpad at Pegasystems with over a decade of experience. He leads with innovation in solving challenging problems and explores the latest AWS technologies for Pega business use cases.

**Anubhav Sharma** - Principal Solutions Architect at AWS with over 2 decades of experience in coding and architecting business-critical applications. He specializes in guiding ISVs and enterprises through their journey of building, deploying, and operating SaaS solutions on AWS.
