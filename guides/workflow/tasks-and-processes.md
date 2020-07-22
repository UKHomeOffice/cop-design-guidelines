---
category: Workflow
expires: 2020-12-31
order: 6
---

# Tasks and Processes in relation to BPMN

## The difference between 'tasks' and 'processes'

### 'task'

A 'task' is a unit of work that is executed inside a 'process'. A 'task' is the smallest unit that a 'process' can be broken down to in terms of BPMN. For example if your process is 'make lasagne' a task might be 'open tin of tomatoes'.

**A 'task' is not a 'process' and the two terms are not interchangeable (although you can usefully think of a reusable subprocess as a task, see below)**

There are several different 'tasks' within a BPMN. The main tasks we use are:

* 'service task'. A 'service task' is used to execute business logic e.g. 'send an email', 'generate PDF'.

* 'user task'. A 'user task' has to be performed by a human being. The BPMN alerts the user to perform a task. The BPMN does not proceed until the user has completed the task (or the task has been cancelled/timed out).  

* 'business rule task'. This task executes the business rules that have been created in DMN. This involves applying business rules to the input data to create an output. Then using the output data to configure a decision gateway. E.g. an insurance claim. When the insurance claim is submitted the data is passed into a 'business rules task' that can determine whether the claim is valid.

* There are other lesser used tasks. Descriptions of them all can be found [here](https://docs.camunda.org/manual/latest/reference/bpmn20/)


### 'process'

The 'process' is composed of a number of 'tasks'. It is important to differentiate between 'tasks' and 'processes'. One way of not confusing the two is to ask yourself if your 'process' can be broken down further into 'tasks'. If it can't then what you are thinking of as a 'process' is actually a 'task'.

As mentioned above a 'process' can be thought of as a recipe e.g. 'make lasagne'. The entirety of your BPMN is a process and broadly speaking each component is a task.

Small units of BPMN that are used as reusable subprocesses can be usefully thought of as 'tasks'. In terms of 'make lasagne' a reusable subprocess that also operates as a 'task' might be 'boil kettle'. The subprocess 'boil kettle' can be broken down further into at least two 'tasks' but it is not useful to do so. 'boil kettle' is a reusable subprocess because it could appear in many 'processes', including 'make cup of tea' or 'cook pasta'.

## 'Task priority'

In COP UI it is possible to assign a priority level against a task. This will be derived by consulting with your user researcher/BA. The levels that can be assigned are 'high', 'medium, and 'low'. This enables tasks to be categorised and displayed based on priority.

The priority is always assigned at the 'user task' level. In the BPMN modeller on a 'user task' in the 'general category' tab there is an 'input' box labelled 'priority'. If the task is 'low priority' then fill in the number 50 (this is an arbitrary number i.e. it has no meaning other than its designation of 'low priority' in the BPMN process). If the task is a 'medium priority' fill the 'priority' input box with 100. If the priority is 'high' then fill in 150. If you leave the 'priority' input box blank then it will default to 'high priority'.
