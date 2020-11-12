---
layout: post
title: Web Application Security and Code Quality Analysis with Static Code Analyzers
index: 3
description: Used multiple open source and proprietary static code analysis tools (Sonarqube, Checkstyle, PMD, FindBugs/SpotBugs) on the full codebase of multiple platforms (Web, Android, iOS) to detect potential vulnerabilities (OWASP Top 10 Security Vulnerabilities). Later fixed those findings using Pareto Principle (80% of consequences come from 20% of the causes) which fixed more than 95% of the reported issues. 
---

<h5><u>Abstract</u></h5>
Security and software quality is becoming more and more important these days. Code reviewing process are also being automated to optimise developers time and ensure less human intervention for early detection of design or security flaws in the development phase. Static code analysers play a significant role in this area and detect some of these flaws such as SQL injection, some potential runtime failures (dereferencing a null pointer), cross site scripting, and simple logical inconsistencies (a control flow which will always be unreachable) etc. 

While the idea of such a system sounds amazing in theory, reality often becomes too complicated as static code analysers operate on a fixed set of rules most of the time and real-life projects usually have their own structure depending on the problem they intend to solve. Hence, the inception of multiple static code analysers (e.g. FindBugs/SpotBugs, PMD, Checkstyle etc) which offers their own set of rules and advantages.

In this experiment, we attempted to run three of the most prominent open source static code analysers (SpotBugs, PMD: Programming Mistake Detector, Checkstyle) for a web application and an android application which essentially rely on the web app for APIs. Next, we used a proprietary platform called [SonarQube](https://www.sonarqube.org/) to analyse the same codes and integrated the system into the **Continuous Integration** pipeline so that each pull request gets evaluated before human review and set up a standard *Quality Profile* to ensure all the code going to production pass these basic criteria.  

<h5><u>Test Environment Setup</u></h5>
The web application codebase had the following technology and code distribution:
<pre>
---------------------------------------------------------------------------------
Language                       files          blank        comment           code
---------------------------------------------------------------------------------
Java                            7273         225902          62468         826657
Freemarker Template              543          29784           8649         226932
JavaScript                       446          40997          35331         205241
JSP                             1092          19627           3257         167284
CSS                              217           4790           2076         114812
XSLT                             491           9867             95         114590
SQL                              786           8722           2916          59462
XML                              348           7474           1220          50838
TypeScript                         3            564           1208          24570
XHTML                            116           2382            232          14385
LESS                              44           2306            743           9992
SVG                               14              0              2           3364
HTML                              15              4            259           1824
Sass                              14             34             34           1768
Gradle                            41            222             14           1382
JSON                               8              0              0           1121
XSD                                1              7              0            747
Bourne Shell                      11            123             85            286
Markdown                           4             49              0            114
---------------------------------------------------------------------------------
SUM:                           11467         352854         118589        1825369
---------------------------------------------------------------------------------
</pre>

The codebase for the android application:
<pre>
--------------------------------------------------------------------------------
Language                      files          blank        comment           code
--------------------------------------------------------------------------------
Java                            330           7993           1823          29458
XML                             175           1647            136           8345
JSON                              2              0              0            330
Gradle                            3             30              3            167
Bourne Again Shell                1             19             20            121
DOS Batch                         1             24              2             64
Markdown                          1              8              0             11
--------------------------------------------------------------------------------
SUM:                            513           9721           1984          38496
-------------------------------------------------------------------------------- 
</pre>

Our primary build tool was gradle and all the tools that we used had corresponding gradle plugin to integrate with the project.
[SpotBugs](https://spotbugs.github.io/) (formerly FindBugs) is a plugin based tool. For each type of technology one has to configure a plugin. Like most bug-detection tools based on static analysis, FindBugs issues some warnings that do not correspond to real bugs. So we had to take those into account.
 
<h5><u>Experiments and Implementation</u></h5>
While SpotBugs offer analysis on various categories such as bad practice, malicious code vulnerability, correctness, performance, security, dodgy code, experimental, multithreaded correctness and internationalisation.
Analyzing and fixing everything at once was a mammoth task hence we looked for the [OWASP Top 10 Web Application Security Risks](https://owasp.org/www-project-top-ten/) 

That provided us with a clear goal towards what to aim for at first and following five seemed to contribute most to the overall technical debt of the project:
1. Bad Practice
2. Malicious Code
3. Vulnerability
4. Security
5. Performance

We decided to start with these five at first and later expand into more specialised cases like correctness, multithreaded correctness and so on.

Later, we integrated two more tools [PMD](https://pmd.github.io/) and [Checkstyle](https://github.com/checkstyle/checkstyle) where we mostly focused on finding bad practices.

The analysis coverage spectrum looked like this:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/coverage_spectrum.png' | relative_url }}" alt="" title="SonarQube Report"/>
    </div>
</div>
Note: This and all other images used here are collected from Google as the codebase used for the experiment is subject to proprietary software product and not publishable for business reasons.

Finally, we moved onto the proprietary platform called SonarQube. SonarQube offers both a deployable product and a SaaS (Software-as-a-Service) cloud. We went for the deployable product and installed in one of our test servers. Later, we integrated the SonarQube gradle plugin to our project to allow SonarQube analyze our code. 
SonarQube presents it's findings in a report like this:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/sonar-report.png' | relative_url }}" alt="" title="SonarQube Report"/>
    </div>
</div>
<br>
 
<h5><u>Results</u></h5>
The tools generated a lot of warnings and suggestions. Fixing everything at once didn’t seem like the way to go as all these fixes had to be tested again and will involve a lot of development and testing resources. Later we came to know about a study conducted by Microsoft where they found that: **80% of the errors and crashes in Windows and Office were caused by 20% of the entire pool of bugs detected**.
                                                       
We opted for the same strategy which is known as Pareto Principle or 80-20 rule and decided to fix top 10% of the reported issues at first. That fixed roughly 80% of the codebase issues and then we went for the next 10% which removed 95% of what we started with initially. Hence, that study conducted by Microsoft seemed valid. 

<h5><u>Conclusion</u></h5>
Finally, we established a **Quality Profile** in SonarQube like the following:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/sonar_quality_gate.png' | relative_url }}" alt="" title="SonarQube Report"/>
    </div>
</div>
<br>

This to ensure all the future codes will go through the same screening and will save a lot of time and effort in future as these bugs will be detected in a very early stage of development and over the time these technical debts will not pile up.

<h5><u>References</u></h5>
1. V.  Okun,  R.  Gaucher,  and  P.E.  Black,  ”Static  analysis  tool  exposition(SATE) 2008, NIST Special Publication 500-279, June 2009.
2. G.  Diaz  and  J.  R.  Bermejo,  ”Static  analysis  of  source  code  security:assessment  of  tools  against  SAMATE  tests,”  Information  and  softwaretechnology 55 (2013) 1462-1476.
3. OWASP Top 10 Web Application Security Risks (https://owasp.org/www-project-top-ten/)
4. [Application of 80/20 Rule in Software Engineering Rapid Application Development (RAD) Model](https://link.springer.com/chapter/10.1007/978-3-642-22203-0_45)
5. SpotBugs - Find bugs in Java program (https://spotbugs.github.io/)
6. PMD (https://pmd.github.io)
7. Checkstyle (https://github.com/checkstyle/checkstyle)
8. SonarQube (https://www.sonarqube.org/) 