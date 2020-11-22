---
layout: post
title: An Intelligent and Unified Log Analytics Infrastructure (Ongoing)
index: 1
description: Current ongoing R&D project. The system aims to get insights from application, system logs and user generated events inside a DMZ to facilitate sensitive data leakage prevention (PHI, HIPAA), understanding usage trends, resource provisioning, problem diagnosis etc.
---

<h5><u>Abstract</u></h5>
Automating manual Operations work is a necessity these days as manual log parsing and checking is a pretty daunting task and human error can also lead to leaking sensitive data to the application logs that might have adverse effect on security, auditing, and might also contribute to additional vulnerabilities. Hence, having a tool that can automatically read and understand sensitive information from logs might be helpful in terms of both prompt action and the reduction of manual labour. Even though such a system might have false positives and times but in such case a false positive is less alarming than a false negative and therefore this makes it a ver good case for utilising Natural Language Processing and Machine Learning based technologies to find insights from the logs. Furthermore, understanding user activity and site usage pattern is a very important for the system designers and developers not only for optimisation but also to ensure better user experience.In a large system this becomes even more important and gradually built-up systems may lose this focus. Hence, a system which can recognise and group common and effective usage pattern is a very useful tool and might improve the product in general.

<h5><u>Vision</u></h5>
The infrastructure is planned to have the following components:
1. Automated alerting system whenever sensitive data (PHI, HIPAA breach) is being leaked in the logs
2. Automated forwarding system to the corresponding dev/team whenever a fatal error occurs for a specific module
3. A Google Chat bot to pass build and merge instruction to CI/CD pipeline
4. A unified dashboard showing usage trends
5. Anomaly detection in Application usage

<h5><u>Data Collection</u></h5>
Logs are essentially a stream of events. We are collecting logs using an open source data collector and aggregator named [Fluentd](https://www.fluentd.org/).
Fluentd can be used to collect, filter, split and aggregate logs from various sources such as access logs, application logs, system logs (syslogd) in a HA architecture 
and then forwarding the logs in the desired format into an [Elasticsearch](https://www.elastic.co/elasticsearch/) cluster. Elasticsearch allows storing and querying 
as of the logs. For a backup the logs are also fed to a Hadoop Distributed File System (HDFS) cluster from where further analysis is conducted.

<h5><u>HIPAA based PHI De-identification Model</u></h5>
Our work is greatly influenced by the 2014 i2b2 (Informatics for Integrating Biology and the Bedside) De-identification Challenge Task where the task was to identify and extract various types of Protected Health Information (PHI) data from clinical free-texts and patient data forms. Later we want to remove or eliminate those sensitive information while keeping the logs otherwise intact. 

For the training and testing purpose we are using the open dataset provided by i2b2 consisting of 1304 medical records among which nearly 60% are being used for training and 40% for testing/validating. The PHI categories that we are considering are: Date, Name, Age, Phone, Contacts, IDs (SSN, Medicare/Medicaid), and Location.

Our current plan consists of the following four steps:
1. <strong>Pre-processing</strong>: In this phase, we are splitting the texts, tokenising and trying to obtain word lemmas.
2. <strong>Feature extraction</strong>: We are extracting a wide variety of linguistic features both syntactic and word surface oriented, which attempt to characterise the semantics of HIPAA based PHI terms. We are trying to benchmark between multiple feature types such as token, contextual, orthographic, and task specific features. 
3. <strong>Model selection</strong>: We are designing a Machine Learning algorithm named Conditional Random Fields (CRF). Here, each token is being assigned one of the so-called BIO scheme tags: B (the first word of a PHI entity mention), I (inside an entity mention), O (outside, not in an entity mention). Several CRFs-based PHI classifiers are created, each of which is targeted for the sub-categories under one particular main PHI category.
4. <strong>Post-processing</strong>: In this step the plan is to correct errors in PHI data or find more potential PHI candidates. In the training and testing dataset there are a lot of malformed data which might induce some bias towards the learning model. That should be corrected in this stage. 

<h5><u>Reporting and Alerting</u></h5>
We are using [Kibana](https://www.elastic.co/kibana) for creating an unified dashboard with charts and diagrams from the data 
stored into the Elasticsearch.
 
