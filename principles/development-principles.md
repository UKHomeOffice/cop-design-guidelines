---
category: Development Principles
expires: 2020-12-31
---

# Development Principles

## Introduction
This section is intended to provide guidance to the best practice in designing BPMNs, DMNs, and Forms. Care around naming conventions on BPMN, methods and style, and effective testing result in a bug-free, business-focuessed, process model.

DMNs should be designed to only use the essential information from the BPMN that is required to move onto the next process or activity. DMN should also be tested in isolation, apart from the BPMN, because it should be tested over several different scenarios. To do this with the BPMN each scenario would have to be set up within the BPMN which is cumbersome and time-consuming. It's much quicker and easier to write one test case for the DMN, with paramaterised data, which is then run through in a very short space of time.

Best practice guidelines ensure that forms are simple, so that they run quickly on the browser, keep users engaged until the end of the process, and the UI doesn't 'time out' before the form has been completed.    

## BPMNs

The main principles to follow when designing your BPMN are (these can be found at Method and Style by Bruce Silver):

* Don't over complicate your BPMNs i.e don't add too many technical constructs into your process.

* Ensure you are working with your business to build the BPMN that they actually need/want.

* For all activities use verb and noun e.g. 'approve (verb) holiday request (noun)'

* Label all decision gateways with a question mark e.g. 'holiday request approved?'.

* Business rule tasks should be names with 'determine...' e.g. 'determine if request needs approval'.

* Use call activities carefully. A re-usable subprocess means it can be used. Before creating a one-off subprocess investigate whether it can be a reusable subprocess rather than a that is only applicable to one particular workflow.

* Don't use message events for inter-lane communication. You might be tempted to use a message event because it is often used to communicate between different pools or external services. However between lanes you should connect the activities directly.  
