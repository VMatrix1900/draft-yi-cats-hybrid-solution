---
title: Hybrid Computing and Network Awareness and Routing Solution for CATS
abbrev: Hybrid Solution for CATS
docname: draft-yi-cats-hybrid-solution-latest
obsoletes:
updates:
date:
category: std
submissionType: IETF

ipr: trust200902
area: Routing Area
workgroup: CATS
keyword: Internet-Draft

author:
 -
  ins: X.Yi
  name: Xinxin Yi
  organization: China Unicom
  email: yixx3@chinaunicom.cn
  role: editor
  city: Beijing
  country: China
 -
  ins: R.Pang
  name: Pang Ran
  organization: China Unicom
  email: pangran@chinaunicom.cn
  role: editor
  city: Beijing
  country: China
 -
  ins: H.Shi
  name: Hang Shi
  organization: Huawei
  email: shihang9@huawei.com
  city: Beijing
  country: China


normative:
  I-D.ldbc-cats-framework:

informative:

...

--- abstract

Computation Aware Traffic Steering (CATS) is a traffic engineering architecture that takes the dynamic changes of computing and network resources into account  when forwarding traffic to appropriate service instances for processing.
For the development of the current network, it is important to have a solution that meets different types of service requirements and can be deployed reasonably  Therefore, this document proposes a hybrid solution to provide differentiated and flexible traffic streering capabilities  for different service while saving the cost of retrofitting existing network equipment.



--- middle

# Introduction {#intro}

CATS enables large-scale interconnected collaboration  at the edge, providing optimal service access and load balancing to adapt to dynamic service. The computing power and network based on the actual processing delay condition can dynamically process the service request to switch to the appropriate service node,  thereby improving the quality of service resource utilization and user experience{{I-D.ldbc-cats-framework}} .
In service scenarios, CATS needs to provide diversified and differentiated service capabilities so that different service traffic can be forwarded to appropriate service instance. This document proposes a hybrid solution, on the one hand, it uses centralized computing information awareness and distributed routing decision. On the other hand, it can provide the service capability of distributed routing or centralized routing for different services to achieve service differentiation.

##  Terminology

##  Requirements Language

{::boilerplate bcp14-tagged}

# Background and Motivation
There are three main aspects of CATS 'work:
- Computing information awareness: The network of the resource utilization status and service status of service instanct needs to be notified, so that the network can perceive the status of service contact instance.
- Select the optimal service contact instance : The optimal service instance needs to be calculated based on the status of computing and network.
- Calculate the optimal forwarding path: After determining the optimal service instance, the optimal forwarding path to the service needs to be calculated. The optimal forwarding path can be determined based on network factors such as delay, packet loss rate, and bandwidth.

In the implementation of the above work, CATS still has some problems:
- A number of devices will be upgraded and the cost will be high, if the computing information needs to be notified between service instance and engress router.
- As business scenarios become more and more diverse, CATS needs to provide differentiated network and computing capabilities for different requirements of different businesses.

This document designs a hybrid solution from two aspects:
- Collect the computing information through the cloud management platform, and then process and send it to the network device on demand. This kind of centralized computing information awareness is more easier to achieve and less costly to deploy compared to collect the computing information distributedly.
- Distributed or centralized routing decision-making methods for different services.  For intelligence transportation：The location of vehicles is constantly changing when vehicles are driving normally on the road. CATS needs to re-select the optimal service instance and the optimal forwarding path according to the latest vehicle location. In addition, the scenario of intelligence transportation has very high requirements on delay, and the delay value will directly affect the driving safety of vehicles. Therefore, it is recommended that the ingress router of CATS makes routing decision, rather than the network controller or other upper layer system recalculating and sending to the ingress router. In this way, the signaling transmission time between the upper-layer system and the ingress router is reduced. Thus, the service quality is improved.
For other scenario such as  VR/AR, SDWAN, which generally prioritize global utilization,  it is suitable  to use the centralized routing decision method since centralized way has a global perspective.

# Service Flow

~~~

               +--------------+       +------------------+
               |  network     |       | cloud management |
               |  controller  |       | platform         |
               +--------------+       +------------------+
                         /                         \
            +------------------+      +---------------+
            |     R2     R3    |------|service instance|
            |                  |      +----------------+
   Client---|R1(ingress router)|
            |                  |      +---------------+
            |     R4     R5    |------|service instance|
            +------------------+      +---------------+
~~~
{: #fig-Hybrid-Solution-Arhicteture title="Hybrid-Solution-Arhicteture"}

## Service Overview

During the deployment of the service, the cloud management platform or other upper-layer systems collect computing information, process it, and then send it to the required network devices, which are generally network ingress router.
Then, CATS  determines whether to use the centralized routing decision-making mode or the distributed routing decision-making mode based on the service type. Generally, it is recommended that the services that have strict requirements on time delay adopt the distributed routing decision making mode, and the other services adopt the centralized routing decision making mode.
When the distributed routing decision-making mode is adopted, the CATS ingress router selects the optimal service instance and calculates the optimal forwarding path according to the status of computing and network, and then directs the traffic to the service instance.
When the centralized routing decision-making mode is adopted, the network controller selects the optimal service instance and calculates the optimal forwarding path according to the status of computing and network, and sends the results to the ingress router. The ingress router directs user traffic to the path until the service instance.

## Work Flow Overview
1. The service instance reports the computing information to the cloud management platform.
2. The cloud management platform processes computing information and send is to the network controller.
3.1 Distributed routing decision mode: The network controller sends computing information to the network ingress router.
3.2 Centralized routing decision mode: The network controller selects the optimal service instance and calculates the optimal forwarding path.
4.1 Distributed routing decision mode:  The ingress router selects the optimal service instance and calculates the optimal forwarding path.
4.2 Centralized routing decision mode: The network controller sends the result of routing desicion to the ingress router.
5. The ingress router performs traffic guidance and forwarding.

# Security Considerations
TBD
