---
category: Development Principles
expires: 2020-12-31
---

# Development Principles

## Introduction
This section is intended to provide guidance to the best practice in designing BPMNs, DMNs, and Forms. Care around naming conventions on BPMN, methods and style, and effective testing result in a bug-free, business-focuessed, process model.

DMNs should be designed to only use the essential information from the BPMN that is required to move onto the next process or activity. DMN should also be tested in isolation, apart from the BPMN, because it should be tested over several different scenarios. To do this with the BPMN each scenario would have to be set up within the BPMN which is cumbersome and time-consuming. It's much quicker and easier to write one test case for the DMN, with paramaterised data, which is then run through in a very short space of time. 

Best practice guidelines ensure that forms are simple, so that they run quickly on the browser, keep users engaged until the end of the process, and the UI doesn't 'time out' before the form has been completed.    
