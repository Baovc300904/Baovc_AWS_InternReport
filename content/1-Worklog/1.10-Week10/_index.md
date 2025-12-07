---
title: "Week 10 Worklog"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 10 Objectives:

* Learn about AWS security best practices.
* Understand monitoring and compliance tools.
* Practice implementing security controls.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Overview of AI/ML on AWS <br> - Learn about ML support services: SageMaker, Rekognition, Comprehend, Kendra, Translate, Polly                                                                                                              | 10/11/2025 | 10/11/2025      | AWS Journey               |
| 3   | - Practice with Amazon SageMaker: <br>&emsp; + Create Notebook Instance <br>&emsp; + Train simple models (Linear Regression / Image Classification) <br>&emsp; + Deploy endpoint and test predictions                                                                                      | 11/11/2025 | 11/11/2025      | AWS Journey                     |
| 4   | - Get familiar with Amazon Rekognition <br> - Demo face and object recognition in images/videos <br> - Integrate Rekognition API into a small web application                                                                                | 12/11/2025 | 12/11/2025      | AWS Journey                 |
| 5   | - Practice Amazon Comprehend (natural language processing) <br> - Experiment with Amazon Kendra (contextual intelligent search) <br> - Compare advantages and limitations of each service                                                                             | 13/11/2025 | 13/11/2025      | AWS Journey               |
| 6   | - Summarize Week 10 knowledge: <br>&emsp; + AI/ML model development process on AWS <br>&emsp; + Real-world applications of AI/ML in business <br>&emsp; + Write practice results report and expansion directions                                        | 14/11/2025 | 14/11/2025      | AWS Journey                            |


### Week 10 Achievements:

* Learned about AWS security:
  * Understood shared responsibility model
  * Reviewed AWS security best practices
  * Studied security pillar of Well-Architected Framework

* Worked with AWS CloudTrail:
  * Enabled CloudTrail in AWS account
  * Configured trail to log to S3 bucket
  * Reviewed API call history
  * Viewed events in CloudTrail console
  * Understood log file format

* Practiced with AWS Config:
  * Enabled AWS Config service
  * Set up Config rules for compliance:
    * Check for encrypted EBS volumes
    * Verify S3 bucket versioning
    * Check IAM password policy
  * Reviewed compliance dashboard
  * Viewed non-compliant resources

* Used AWS GuardDuty:
  * Enabled GuardDuty for threat detection
  * Reviewed finding types
  * Viewed sample findings
  * Understood severity levels (Low, Medium, High)
  * Learned about common threats detected

* Explored AWS Security Hub:
  * Enabled Security Hub
  * Viewed consolidated security findings
  * Reviewed security scores
  * Understood integration with GuardDuty and Config
  * Exported findings report