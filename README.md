# CloudGoat
 IAM Privilege Escalation by Rollback Scenario
# Objective
To understand and practice privilege escalation techniques in AWS IAM (Identity and 
Access Management) using the CloudGoat scenario "iam_privesc_by_rollback." 

# Skills Learned
- AWS CLI Proficiency: Extensive use of the AWS Command Line Interface (CLI) for initial setup, configuration (aws configure --profile linux-dev ), user verification (aws sts get-caller-identity --profile raynor ), and reconnaissance (aws iam get-account-authorization-details --profile raynor , aws iam list-attached-user-policies ).
- IAM Privilege Escalation Technique (Rollback): Understood and exploited the vulnerability where an IAM policy allows a user to set the default version of an IAM policy (iam:SetDefaultPolicyVersion) to a prior version that contains overly permissive rights (e.g., Action: "*", Resource: "*"). This is often possible when policies are initially created with broad permissions and then "downgraded" without deleting the older, vulnerable versions
- Policy and Policy Version Enumeration: Used the AWS CLI to enumerate policies attached to the compromised user (raynor-cgidypszbitagi) and then iterated through the versions of the attached managed policy to find the overly permissive one (e.g., Version 5, which granted administrative permissions).
- CloudGoat Deployment and Management: Experience with setting up and deploying an intentional vulnerability environment using CloudGoat  (a vulnerable-by-design AWS setup tool by Rhino Security Labs) via Terraform on a Kali Linux virtual machine.
- Security Best Practices & Mitigation: Gained an understanding of mitigation strategies, including the principle of least privilege access , regular policy reviews , and ensuring older, overly-permissive policy versions are deleted or never created in production environments.
  
# Commands Used
Step	Command	Purpose
Setup	

pip install cloudgoat 

Install the CloudGoat tool.
Setup	

aws configure --profile linux-dev 

Configure the initial AWS profile with access/secret keys.
Launch	

cloudgoat create iam_privesc_by_rollback 

Deploy the vulnerable scenario infrastructure using Terraform.
Verify	

aws sts get-caller-identity --profile raynor 

Verify the access keys of the compromised user (raynor).
Recon	

aws iam list-attached-user-policies --user-name raynor-cgidypszbitaqi 

Identify the policy attached to the user.
Recon	aws iam list-policy-versions --policy-arn ...	
Identify all policy versions to find the one with Action: "*".

Exploit	

aws iam set-default-policy-version --policy-arn ... --version-id v5 

Set the default policy version to the privileged one (V5) to escalate permissions.

# Steps
1. Setup: 
• Ensure you have an AWS account or access to an AWS environment where you 
can perform IAM actions. 
• Install and configure the AWS CLI if you haven't already.

<img width="630" height="309" alt="Screenshot (638)" src="https://github.com/user-attachments/assets/6f4da574-5b40-4fda-9b43-ebf28151b447" />

2. Accessing CloudGoat: 
• Navigate to the CloudGoat GitHub repository: 
https://github.com/RhinoSecurityLabs/cloudgoat 
• Follow the instructions provided to deploy CloudGoat in your AWS environment.
-Installing System Requirements

<img width="809" height="394" alt="2" src="https://github.com/user-attachments/assets/d2d209c1-2035-4e68-9ae2-65edc1f56b2f" />

-Installing Cloudgoat

<img width="809" height="394" alt="1" src="https://github.com/user-attachments/assets/b42694b3-ac43-4886-969c-f1b1bf32b2a8" />

4. Configuring Profile

<img width="809" height="394" alt="3 1" src="https://github.com/user-attachments/assets/a024430f-4cd8-4089-be9d-965deca624d8" />

- Configuring profile to cloudgoat

<img width="809" height="394" alt="4" src="https://github.com/user-attachments/assets/14043fe7-f267-4965-8eb1-7bc22b82adf9" />

4. Starting the Scenario
  - Once CloudGoat is deployed, access the CloudGoat environment using the 
provided credentials or IAM user.
  - Launch the "iam_privesc_by_rollback" scenario from the CloudGoat menu or 
command-line interface.

<img width="809" height="394" alt="5" src="https://github.com/user-attachments/assets/1055c23f-2d96-4fee-b808-553456f6f6f1" />

<img width="809" height="394" alt="6" src="https://github.com/user-attachments/assets/7b19c258-021f-4740-a041-f423ea6d17d6" />

<img width="809" height="394" alt="7" src="https://github.com/user-attachments/assets/e553d50a-c53b-421c-a983-0803531b1c09" />

5. Exploring Initial Configuration
  - Use the AWS CLI or AWS Management Console to examine the initial IAM 
configuration, including existing users, roles, and policies.

<img width="809" height="394" alt="8" src="https://github.com/user-attachments/assets/9cd4fb1f-96df-4cda-b374-359c73d594da" />

  - Identify any permissions assigned to the user or role provided in the scenario.
 -Configuring profile raynor

<img width="809" height="394" alt="10" src="https://github.com/user-attachments/assets/828bd242-0a65-4ca7-bc00-b7501b8e6f12" />

-Authorization details on the profile raynor

<img width="809" height="394" alt="11" src="https://github.com/user-attachments/assets/a584791c-37ee-4c0c-b53b-dd5feddfb237" />

-Attached user policies on the profile

<img width="809" height="394" alt="12" src="https://github.com/user-attachments/assets/b04acf16-d68f-4ef4-b8d1-df3af2fcb706" />

6. Performing Privilege Escalation:
 -  Follow the steps outlined in the scenario to exploit IAM permissions and escalate 
privileges.
 - Pay attention to any rollback mechanisms or configuration changes that can be 
abused to gain elevated access.
-Attached User Policies on Raynor profile

<img width="809" height="394" alt="12" src="https://github.com/user-attachments/assets/b2b0ecc2-2045-4af6-a63a-f206085d75eb" />

-Enumerating User Policies

<img width="809" height="394" alt="13" src="https://github.com/user-attachments/assets/45e5288f-6b36-4867-9e9d-e810a5a983b1" />

<img width="809" height="394" alt="14" src="https://github.com/user-attachments/assets/c7bf47d8-623b-48b8-b5fe-151f89accb91" />

7. Enumerating policy versions
   - Version 1
 
<img width="809" height="394" alt="15" src="https://github.com/user-attachments/assets/96844b9d-425f-44bb-ace7-5358754a55d1" />

  - Version 2

<img width="809" height="394" alt="16" src="https://github.com/user-attachments/assets/905d8713-7650-4835-a81d-11ed24cd829d" />

  - Version 3

<img width="809" height="394" alt="17" src="https://github.com/user-attachments/assets/48b348d2-f613-42fe-a1c8-b3680d19d354" />

  - Version 4

<img width="809" height="394" alt="18" src="https://github.com/user-attachments/assets/60d39c84-0900-430d-8c97-38a78f30ed79" />

  - Version 5

<img width="809" height="394" alt="19" src="https://github.com/user-attachments/assets/61fc2342-971d-4647-b22d-e162c986821b" />

8. Breaking this apart it grants all actions ("Action": "*") to all resources. Since the current 
policy version grants the ability to change versions let switch to use this.

<img width="809" height="394" alt="20" src="https://github.com/user-attachments/assets/cc207ad2-0354-4ddd-8355-9677d4f142f1" />

- We now have administrative permissions
  
<img width="809" height="394" alt="21" src="https://github.com/user-attachments/assets/3ac28489-e11b-456c-b1fc-54b2abbe9fab" />

9. The Secrets manager keys

<img width="809" height="394" alt="22" src="https://github.com/user-attachments/assets/d1a71c2e-6cae-4c43-b4fa-43cbf0d13ae2" />

# Reflection and Analysis:
 - Consider how such vulnerabilities can be mitigated or prevented in real-world 
AWS environments.
-Effectively managing privilege policies in AWS environments is crucial for security. It involves 
implementing least privilege access, continuous monitoring, and automated policy management 
through Infrastructure as Code. Regular policy reviews, incident response readiness, and ongoing 
training ensure policies align with security best practices and business needs, mitigating 
vulnerabilities and preventing unauthorized access.












 







