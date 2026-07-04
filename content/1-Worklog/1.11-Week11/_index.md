---
title: "Week 11 Worklog"
date: 2026-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

*   Integrate the deployed AWS infrastructure into the entire team's shared system.
*   Test the entire Spring Boot Backend system, AI Agent, and data flow on the server.
*   Adjust Cloud configurations and resolve communication errors between services to ensure stable system operation.

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
      <td class="col-task">- Integrate the deployed AWS infrastructure (Lambda functions, S3, RDS) into the team's main Spring Boot source code flow. <br> - Connect to the main server and check compatibility between Backend components via the AWS SDK, ensuring Spring Boot successfully calls Serverless services.</td>
      <td class="col-date">06/29/2026</td>
      <td class="col-date">06/29/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Check connection and distribution network flow: Monitor the request flow from the Frontend through Route 53, CloudFront, AWS WAF, and directly to the API Gateway / Backend Controller. <br> - Test routing and content delivery capabilities, ensuring CloudFront's Caching mechanism does not affect the real-time nature of the Unit Test generation APIs.</td>
      <td class="col-date">06/30/2026</td>
      <td class="col-date">06/30/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Test the system's access flow: Run a test of the complete RAG business flow (Upload source code -> Chunking -> Vector DB -> Gemini AI -> Return Unit Test results). <br> - Coordinate with Frontend members to test interface functions, ensuring test history and results are displayed correctly after integration.</td>
      <td class="col-date">07/01/2026</td>
      <td class="col-date">07/01/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Fix Backend and Cloud errors that arose during integration, such as Timeout errors when Lambda calls the Gemini API, or CORS errors between the Frontend and Spring Boot.</td>
      <td class="col-date">07/02/2026</td>
      <td class="col-date">07/02/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Re-test the entire system after bug fixes. <br> - Evaluate the query performance of pgvector's Hybrid Search and load capacity, and assess the security of the deployment architecture against simulated requests.</td>
      <td class="col-date">07/03/2026</td>
      <td class="col-date">07/03/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Finalize AWS infrastructure configuration documentation, noting necessary environment variables for Spring Boot and Lambda. <br> - Update the architecture diagram to the actual deployment version (including details of the Google Gemini API, Vector DB, and AWS Serverless integration flow).</td>
      <td class="col-date">07/04/2026</td>
      <td class="col-date">07/04/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Consolidate the deployment results of the entire Backend and Cloud layer. <br> - Prepare the source code, API documentation, and hand over the stable infrastructure/Backend to serve the finalization phase of the entire group project.</td>
      <td class="col-date">07/05/2026</td>
      <td class="col-date">07/05/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Week 11 Achievements:

*   Successfully integrated the AWS infrastructure (Serverless, Storage, Database, Networking) into the team's shared source code system.
*   Successfully tested the access flow and core functional flows of the system on the server.
*   Completed configuration and definitively resolved errors that arose during integration.
*   Ensured the system operates smoothly, stably, and securely before entering the finalization and acceptance week of the project.