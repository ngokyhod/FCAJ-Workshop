---
title: "Week 10 Worklog"
date: 2026-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

*   Refactor the system architecture, transitioning the static local processing model to a dynamic Serverless architecture on AWS to improve performance.
*   Finalize the "AI Core" with the Retrieval-Augmented Generation flow and Google Gemini API.
*   Develop a project management API system and integrate Amazon RDS (PostgreSQL) to manage metadata.

### Tasks to be carried out this week:
<table class="worklog-table">
<colgroup>
  <col class="col-day" style="width:5%">
  <col class="col-task" style="width:42%">
  <col class="col-start" style="width:13%">
  <col class="col-end" style="width:13%">
  <col class="col-ref" style="width:27%">
</colgroup>
  <thead>
    <tr>
      <th>Day</th>
      <th>Task</th>
      <th>Start Date</th>
      <th>Completion Date</th>
      <th>Reference Material</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="col-day">1</td>
      <td class="col-task">- System Refactoring: Begin transitioning heavy processing flows from local to the AWS Cloud infrastructure to increase scalability. <br> - Prepare Cloud Environment: Create an IAM User and grant necessary access permissions for S3 and RDS services. Check permissions and configure the AWS CLI for deployment.</td>
      <td class="col-date">06/22/2026</td>
      <td class="col-date">06/22/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Database Design: Create a database schema on Amazon RDS (PostgreSQL) to store metadata, including: User information, Project list (Git/Zip, creation time), and AI code generation history. <br> - Routing Configuration: Configure Amazon Route 53, create a Hosted Zone for the system's domain, and create a DNS Record pointing to the CloudFront Distribution as per the designed architecture.</td>
      <td class="col-date">06/23/2026</td>
      <td class="col-date">06/23/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Develop Lambda 1 (Project Import - Git/Zip): Receive source code, unzip, and sync raw data to Amazon S3, while recording project metadata in RDS. Use a VPC Endpoint to optimize S3 access and avoid Timeouts. <br> - CDN Configuration: Deploy Amazon CloudFront Distribution, configure the Origin to connect to API Gateway, and set up Cache Behaviors for API & Static Assets.</td>
      <td class="col-date">06/24/2026</td>
      <td class="col-date">06/24/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Finalize Core AI: Successfully deploy the RAG flow that executes reading contextual data from Amazon S3 storage and communicates with the Google Gemini API to automatically generate Unit Test source code. <br> - Secure Transmission Flow: Configure SSL Certificate, attach a Custom Domain to CloudFront, and test secure access via HTTPS. <br> - Develop Lambda 2 (File Tree Service): Build a function to read the directory structure, traverse the source code files stored on Amazon S3, and return a Tree format for the Frontend to display the file list.</td>
      <td class="col-date">06/25/2026</td>
      <td class="col-date">06/25/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Develop Lambda 3 (API Invoke Service): Build a bridge function to communicate directly with the AI. The function is responsible for securely transmitting Prompt and Context data. <br> - Develop Lambda 4 (Rag Context Lambda): Build logic to group and preprocess context before feeding it into the large language model.</td>
      <td class="col-date">06/26/2026</td>
      <td class="col-date">06/26/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Develop Lambda 5 (Result And History Service): A function to receive responses from the AI, filter out redundant Markdown syntax, save the pure Unit Test results to S3, and record them in the History table in RDS for Frontend retrieval. <br> - Integrate Spring Boot Backend: Set up the ProjectApiController class as a bridge to communicate between the interface and AWS Lambda functions via the AWS SDK. <br> - Troubleshooting: Thoroughly resolve API mapping conflicts and configure to fix missing Bean errors in Spring Boot to ensure a smooth business flow.</td>
      <td class="col-date">06/27/2026</td>
      <td class="col-date">06/27/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- End-to-End (E2E) Testing: Run a full test of the automated Unit Test generation cycle from: Frontend -> Backend -> AWS Lambda -> S3 & RDS. <br> - Evaluation: Review all configured AWS infrastructure, assess the processing performance of Lambda functions, and the response capability of the Google Gemini API. <br> - Record deployment results, complete code standardization, and update technical documentation.</td>
      <td class="col-date">06/28/2026</td>
      <td class="col-date">06/28/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Week 10 Achievements:

*   Successfully transitioned the architecture from Local to Serverless by packaging and deploying 5 AWS Lambda functions (Import, Result, History, Invoke, FileTree).
*   Successfully deployed the Core AI RAG flow with a stable connection to the Google Gemini API, generating Unit Test code with automatic markdown cleaning.
*   Fully integrated the Amazon RDS database to manage user and project Metadata.
*   Seamlessly connected the entire system from the Frontend through the Backend to the AWS Serverless ecosystem.