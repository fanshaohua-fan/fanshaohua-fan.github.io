---
layout: post
title:  "AZ-204: Practice topic 2"
categories: az204
tags:   azure az204 practice
---


1.
N   X

N

N

Y


2.
Y
N
Y


### [Page 9](https://www.examtopics.com/exams/microsoft/az-204/view/9/)

3. D    X   B

While a blob is in the Archive tier, it can't be read or modified. To read or download a blob in the Archive tier, you must first rehydrate it to an online tier, either Hot or Cool. Data in the Archive tier can take up to **15 hours** to rehydrate


4. 
BoundedStaleness

--enable-automatic-failover

eastus=1


5. 
--sku SHARED    X

image-name: latest

container set images.azurecr.io

Should be: 
--sku B1 --is-linux


**Windows:**
--deployment-source-url -u

**Linux:**
--deployment-container-image-name -i



6.
Service bus queue

Active Message Count

Average


7.
Fetch

Metadata.Add

SetMetadataAsync


### [Page 10](https://www.examtopics.com/exams/microsoft/az-204/view/10/)

8. N

https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services

|Service|	Purpose|	Type|	When to use|
|--|--|--|--|
|Event Grid|	Reactive programming|	Event distribution (discrete)|	React to status changes
|Event Hubs|	Big data pipeline|	Event streaming (series)|	Telemetry and distributed data streaming
|Service Bus|	High-value enterprise messaging|	Message|	Order processing and financial transactions

9. A


10. 
Create
Upgrade
Copy


11. C


12. A

### [Page 11](https://www.examtopics.com/exams/microsoft/az-204/view/11/)

13. 
metadata/identiry

new NetworkCredential

14. 
orderBy X

descending

https://docs.microsoft.com/en-us/azure/cosmos-db/sql/how-to-manage-indexing-policy?tabs=dotnetv2%2Cpythonv3#composite-index



15. 
4   X

Department  X

Partitions relate to producers - and the logical way to partition the incoming data is by the only value you have at that point, the highway name/id. So the selected answer is correct (6 Partitions, by Highway).


16. 
Helm

kubectl

ingress controller

17. 
NoFilter    X should be SQL/Correlation Filter

SQLFilter*3

NoFilter


https://docs.microsoft.com/en-us/azure/service-bus-messaging/topic-filters#filters

![](/images/2022-02-06-06-25-32.png)

### [Page 12](https://www.examtopics.com/exams/microsoft/az-204/view/12/)

18. 
User request -> No edge servers has image in the cache -> origin server returns images -> subsequent

19. 
CE  X   DE

C. a single property value that appears frequently in the documents

D. a concatenation of multiple property values with a random suffix appended

https://docs.microsoft.com/en-us/azure/cosmos-db/synthetic-partition-keys


20.
Y

Y

Y 

21. 
Host    X

Lease container

Monitored container X

Delegate

![](/images/2022-02-07-19-38-20.png)