---
layout: post
title:  "AZ-204: Practice topic 4"
categories: az204
tags:   azure az204 practice
---

1. 
AB  X BD

https://docs.microsoft.com/en-us/azure/azure-monitor/app/custom-operations-tracking

2. 
Y

Y   X   N

Y 


https://docs.microsoft.com/en-us/azure/frontdoor/front-door-caching

3. 
Set if missing  X   Override

1 hour 

Cache every unique URL

4. 
Standard

Enable

Add

Configure

### [Page 21](https://www.examtopics.com/exams/microsoft/az-204/view/21/)

5. 
N

6. 
N

Instead deploy and configure Azure Cache for Redis. Update the web applications.

7. 
ICache  X   IDatabase
ValueDel

8. 
Create
trigger on message
Action to read IoT
Add a condition to compare
Add to send email


9. 
Funnels

Impact

User flows  X Retention

User flows


- Retention - how many users come back?

- Users tool: How many people used your app and its features. Users are counted by using anonymous IDs stored in browser cookies. A single person using different browsers or machines will be counted as more than one user.

- Sessions tool: How many sessions of user activity have included certain pages and features of your app. A session is counted after half an hour of user inactivity, or after 24 hours of continuous use.

- Events tool: How often certain pages and features of your app are used. A page view is counted when a browser loads a page from your app, provided you've instrumented it.

- Funnel: If your application involves multiple stages, you need to know if most customers are progressing through the entire process, or if they're ending the process at some point. The progression through a series of steps in a web application is known as a funnel

- User flows: 

- - How do users navigate away from a page on your site?
- - What do users click on a page on your site?
- - Where are the places that users churn most from your site?
- - Are there places where users repeat the same action over and over?

### [Page 22](https://www.examtopics.com/exams/microsoft/az-204/view/22/)

10. B   X   A

11. AD

12. A

13. A

14. 
ago

distinct

where

summarize count()

### [Page 23](https://www.examtopics.com/exams/microsoft/az-204/view/23/)


15. 
config 

--docker-container-log

webapp

tail


```sh
az webapp log config -h

Command
    az webapp log config : Configure logging for a web app.

Arguments
    --application-logging      : Configure application logging.  Allowed values: azureblobstorage,
                                 filesystem, off.
    --detailed-error-messages  : Configure detailed error messages.  Allowed values: false, true.
    --docker-container-logging : Configure gathering STDOUT and STDERR output from container.
                                 Allowed values: filesystem, off.
    --failed-request-tracing   : Configure failed request tracing.  Allowed values: false, true.
    --level                    : Logging level.  Allowed values: error, information, verbose,
                                 warning.
    --slot -s                  : The name of the slot. Default to the productions slot if not
                                 specified.
    --web-server-logging       : Configure Web server logging.  Allowed values: filesystem, off.

az webapp log -h

Group
    az webapp log : Manage web app logs.

Subgroups:
    deployment [Preview] : Manage web app deployment logs.

Commands:
    config               : Configure logging for a web app.
    download             : Download a web app's log history as a zip file.
    show                 : Get the details of a web app's logging configuration.
    tail                 : Start live log tracing for a web app.
```

16. BC

### Types of tests
There are four types of availability tests:

- URL ping test (classic): You can create this simple test through the portal to validate whether an endpoint is responding and measure performance associated with that response. You can also set custom success criteria coupled with more advanced features, like parsing dependent requests and allowing for retries.
- Standard test (Preview): This single request test is similar to the URL ping test. It includes SSL certificate validity, proactive lifetime check, HTTP request verb (for example GET, HEAD, or POST), custom headers, and custom data associated with your HTTP request.
- Multi-step web test (classic): You can play back this recording of a sequence of web requests to test more complex scenarios. Multi-step web tests are created in Visual Studio Enterprise and uploaded to the portal, where you can run them.
- Custom TrackAvailability test: If you decide to create a custom application to run availability tests, you can use the TrackAvailability() method to send the results to Application Insights.

https://docs.microsoft.com/en-us/azure/azure-monitor/app/availability-overview


17. 

internal    X   External

consumption plan doesn't support build-in cache.

private

Authorization

https://docs.microsoft.com/en-us/azure/api-management/api-management-caching-policies#CachingPolicies

![](/images/2022-03-02-20-28-54.png)

18. D

19. A

### [Page 23](https://www.examtopics.com/exams/microsoft/az-204/view/23/)

20. 

21. 

22. 

23. 

24. 

### [Page 24](https://www.examtopics.com/exams/microsoft/az-204/view/24/)


25. 

26. 

27. 

28. 

29. 