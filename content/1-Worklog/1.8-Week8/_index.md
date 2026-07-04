---
title: "Week 8 Worklog"
date: 2026-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

*   Set up the Vector Database infrastructure (PostgreSQL with pgvector).
*   Configure ORM Hibernate to support vector data format.
*   Integrate security and connect the Spring Boot application with the AWS infrastructure.
*   Deploy the data embedding process using the Gemini API.

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
      <td class="col-task">- Set up Vector Database infrastructure: Install and launch a PostgreSQL server with the pgvector extension. <br> - Configure the environment for the Backend Spring Boot to connect to the database container.</td>
      <td class="col-date">06/08/2026</td>
      <td class="col-date">06/08/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Configure ORM Hibernate in Spring Boot: Build @Entity classes that support array data types. <br> - Define the configuration for the embedding column with the vector(1024) format to ensure compatibility when storing vector matrices later.</td>
      <td class="col-date">06/09/2026</td>
      <td class="col-date">06/09/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Check the readiness of service clients (S3, RDS) to ensure the Backend has full access to cloud resources.</td>
      <td class="col-date">06/10/2026</td>
      <td class="col-date">06/10/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Switch data embedding strategy: Modify the logic to replace the old Titan Embedding model by directly calling the Google Gemini API. <br> - Write a Service to handle the connection, sending text data from Document Chunks via the Gemini API to retrieve the corresponding vector matrices.</td>
      <td class="col-date">06/11/2026</td>
      <td class="col-date">06/11/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Develop vector storage logic: Program a function to store the Embedding results from the Gemini API into the vector(1024) column in PostgreSQL. <br> - Unit test the data flow from reading the file to successfully storing it in the Database.</td>
      <td class="col-date">06/12/2026</td>
      <td class="col-date">06/12/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Optimize data: Handle cases where text exceeds the Gemini API's token limit by chunking it before sending. <br> - Ensure the output format of the vector completely matches the structure required by pgvector ([0.1, 0.2, ...]).</td>
      <td class="col-date">06/13/2026</td>
      <td class="col-date">06/13/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Consolidate and test the data embedding flow: Run a test of the process of loading data from a local machine to the vector database. <br> - Confirm that the data has been embedded and stored correctly, ready for the Cosine Similarity query phase in the next week.</td>
      <td class="col-date">06/14/2026</td>
      <td class="col-date">06/14/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Week 8 Achievements:

*   Successfully deployed PostgreSQL with the pgvector extension, optimizing cost and local development resources.
*   Successfully configured Hibernate for Spring Boot to interact with the vector(1024) data type.
*   Successfully transitioned to using the Google Gemini API for data embedding of source code blocks.
*   Systematized the embedding and vector storage process, ensuring source code data is ready for the similarity query step next week.