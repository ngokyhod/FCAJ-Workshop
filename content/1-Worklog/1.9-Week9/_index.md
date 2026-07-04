---
title: "Week 9 Worklog"
date: 2026-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

*   Optimize vector database queries using pgvector.
*   Finalize the integrated data orchestration flow between Chunking, Embedding, and the Database.
*   Prepare the Agentic environment using the Google Gemini API to generate Unit Tests.
*   Prepare to deploy a secure network and content delivery infrastructure architecture with Amazon Route 53, CloudFront, and AWS WAF.

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
      <td class="col-task">- Review the entire infrastructure deployment architecture of the project, comparing the current Backend components with the planned AWS infrastructure diagram. <br> - Research the Hybrid Search query mechanism in vector databases to optimize search accuracy for source code.</td>
      <td class="col-date">06/15/2026</td>
      <td class="col-date">06/15/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Write advanced SQL queries combined with the pgvector library in Spring Boot. <br> - Apply pgvector's Cosine Similarity distance measurement to retrieve and rank source code blocks with the most similar meaning to the user's query.</td>
      <td class="col-date">06/16/2026</td>
      <td class="col-date">06/16/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Finalize the VectorStoreService data orchestration flow in the Backend layer. <br> - Assemble the Code Chunking and Data Embedding modules into a closed-loop, automated processing cycle.</td>
      <td class="col-date">06/17/2026</td>
      <td class="col-date">06/17/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Research the configuration process for services serving the content delivery and network infrastructure security layer on AWS. <br> - Analyze the deployment model of Amazon Route 53, Amazon CloudFront, and AWS WAF security rules to protect the API Gateway for Backend processing flows.</td>
      <td class="col-date">06/18/2026</td>
      <td class="col-date">06/18/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Test the Backend data flow system: Build internal APIs to simulate and evaluate the End-to-End source code ingestion flow. <br> - Run experiments to confirm that vector data from the Gemini API is generated in a standard format and stored correctly in the PostgreSQL database.</td>
      <td class="col-date">06/20/2026</td>
      <td class="col-date">06/20/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Prepare the Agentic environment: Plan the integration and build the Prompt structure for the Google Gemini API so the system can receive contextual data just retrieved from the Vector Database. This is a crucial step for the automated Unit Test generation phase. <br> - Consolidate design documents and prepare the environment to start the actual configuration of AWS infrastructure services next week.</td>
      <td class="col-date">06/21/2026</td>
      <td class="col-date">06/21/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Week 9 Achievements:

*   Successfully programmed the context search feature based on pgvector's Cosine Similarity, ensuring the AI finds the correct related code snippets.
*   Completed the VectorStoreService cycle with smooth handling of Chunking, calling the Embedding API, and Batch Insert.
*   Successfully built an internal API system to test the data flow, confirming that vector data is stored correctly.
*   Developed a deployment plan for AWS WAF, Route 53, and CloudFront security to protect the Backend.
*   Prepared the Context data flow to be fed into Google Gemini for automatic Unit Test code generation.