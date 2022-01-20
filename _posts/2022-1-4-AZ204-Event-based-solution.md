---
layout: post
title:  "AZ-204: Develop event-based solutions"
categories: event-based
tags:   azure az204 event-based
---

# [AZ-204: Develop event-based solutions](https://docs.microsoft.com/en-us/learn/paths/az-204-develop-event-based-solutions/)

## Azure Event Grid

After completing this module, you'll be able to:

- Describe how Event Grid operates and how it connects to services and event handlers.
- Explain how Event Grid delivers events and how it handles errors.
- Implement authentication and authorization.
- Route custom events to web endpoint by using Azure CLI.

**Definition:**
Azure Event Grid is an eventing backplane that enables event-driven, reactive programming. It uses the publish-subscribe model. Publishers emit events, but have no expectation about how the events are handled. Subscribers decide on which events they want to handle.


**Concepts:**

- Events - What happened.
- Event sources - Where the event took place.
- Topics - The endpoint where publishers send events.
- Event subscriptions - The endpoint or built-in mechanism to route events, sometimes to more than one handler. Subscriptions are also used by handlers to intelligently filter incoming events.
- Event handlers - The app or service reacting to the event.

[**Schema**](https://docs.microsoft.com/en-us/learn/modules/azure-event-grid/3-event-grid-schema)

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```



## Azure Event Hubs
