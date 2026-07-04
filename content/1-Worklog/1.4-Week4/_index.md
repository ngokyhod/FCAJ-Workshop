---
title: "Week 4 Worklog"
date: 2026-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

*   Practice setting up a custom AWS Virtual Private Cloud (VPC) network infrastructure instead of using the default network.
*   Design and configure a tiered security network architecture (Public and Private Subnets).
*   Deploy and attach advanced network routing flows through Internet Gateway (IGW) and NAT Gateway.
*   Set up server-level security firewalls (Security Groups) and test the system's connectivity.

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
      <td class="col-task">- Study and practice Module 02-Lab03-03.1 - Create VPC. <br> - Successfully create a custom VPC with a total IP address range of 10.0.0.0/16.</td>
      <td class="col-date">05/11/2026</td>
      <td class="col-date">05/11/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">2</td>
      <td class="col-task">- Practice Module 02-Lab03-03.2 - Create Subnet. <br> - Implement a tiered security network architecture including Public Subnet and Private Subnet.</td>
      <td class="col-date">05/12/2026</td>
      <td class="col-date">05/12/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">3</td>
      <td class="col-task">- Practice Module 02-Lab03-03.3 - Create an Internet Gateway. <br> - Practice Module 02-Lab03-03.4 - Create Route Table for Outbound Internet Routing via Internet Gateway. <br> - Deploy and attach an Internet Gateway (IGW) to the VPC for the Public zone via the Public Route Table (0.0.0.0/0).</td>
      <td class="col-date">05/13/2026</td>
      <td class="col-date">05/13/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">4</td>
      <td class="col-task">- Practice Module 02-Lab03-04.3 - Create NAT Gateway. <br> - Study the nature and practical use cases to practice configuring a NAT Gateway. <br> - Configure a one-way routing flow for the Private Route Table to the NAT Gateway.</td>
      <td class="col-date">05/14/2026</td>
      <td class="col-date">05/14/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">5</td>
      <td class="col-task">- Practice Module 02-Lab03-03.5 - Create security groups. <br> - Study Inbound/Outbound rules and set up instance-level security policies (Security Groups).</td>
      <td class="col-date">05/15/2026</td>
      <td class="col-date">05/15/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">6</td>
      <td class="col-task">- Practice Module 02-Lab03-04.1 - Create EC2 Instances in Subnets. <br> - Create Amazon EC2 virtual servers and allocate them to the correct Subnets (Public/Private).</td>
      <td class="col-date">05/16/2026</td>
      <td class="col-date">05/16/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
    <tr>
      <td class="col-day">7</td>
      <td class="col-task">- Practice Module 02-Lab03-04.2 - Test connection. <br> - Practice Module 02-Lab03-04.5 - EC2 Instance Connect Endpoint. <br> - Perform Ping, SSH tests to EC2 Instances to validate connectivity.</td>
      <td class="col-date">05/17/2026</td>
      <td class="col-date">05/17/2026</td>
      <td class="col-ref"><https://cloudjourney.awsstudygroup.com/></td>
    </tr>
  </tbody>
</table>


### Week 4 Achievements:

*   Successfully created a custom VPC (lab-vpc-01) with a total address range of 10.0.0.0/16.
*   Successfully divided the network into a tiered security architecture including Public Subnet and Private Subnet.
*   Correctly deployed and attached an Internet Gateway (IGW) to the VPC for the Public zone.
*   Successfully configured a NAT Gateway and a one-way routing flow for the Private Route Table, securing the internal server system.
*   Completed the setup of instance-level security policies (Security Groups) and successfully tested system connectivity.