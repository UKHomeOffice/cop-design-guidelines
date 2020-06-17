---
category: Development Principles
expires: 2020-12-31
---

# Development Principles

## Introduction
This section is intended to provide guidance to the best practice in designing BPMNs, DMNs, and Forms.
Best practice guidelines ensure that BPMNs are simple and efficient. The forms arising from good BPMNs run quickly on the browser, ensuring users remain engaged until the end of the process, and the UI doesn't 'time out' before the form has been completed. Taking care with naming conventions, methods, and style in BPMN, and effective testing, result in a bug-free, business-focussed, process model.

## BPMNs

### Testing

You should test the BPMN without having deployed your process to COP. Your BPMN should not be deployed to COP until it has been thoroughly unit tested i.e. **deploying to COP is the last step in your development process.**

It's also better to test DMNs in isolation, apart from the BPMN, because it should be tested over several different scenarios. It's best not to do this in the BPMN as each scenario would have to be set up within the process, which is cumbersome and time-consuming. It's much quicker and easier to write one test case for the DMN, with paramaterised data, which is then run in a very short space of time.

What you should do is test your BPMN using [Camunda BPM Assert](https://github.com/camunda/camunda-bpm-assert). This allows you to test different aspects of the BPMN and achieve 'code coverage'. It will load the process in an 'in-memory' engine and validate the model. BPM Assert finds problems such as language expression errors, and missing properties in your spin objects, which are common mistakes and easily fixed at this stage, before deployment.  

![BPM Assert]({{'/images/bpm-code-coverage.jpeg' | relative_url }})

**Figure 1. BPM Assert showing 'code coverage' i.e. fully tested and functional BPMN**

If you have multiple BPMNs for a given use case don't try and test them all in a single test case. So, for a given BPMN create a corresponding test class. Then in the test class define multiple test scenarios. Keep these scenarios small and simple.

Where you have a BPMN that includes a reusable subprocess, test the BPMN without the subprocess, and test the subprocess itself separately, otherwise it will be too complicated and it will involve a lot of data setup. So, for 'call activity' where these link to reusable subprocesses, stop at the 'call activity' of the BPMN and then test after the 'call activity'. This means the reusable subprocess within the 'call activity' can be tested separately.

There are examples of how to use BPM Assert in the Home Office COP Process Spec GitLabs Project. Please consult your Tech Lead/Arch to gain access, as these example tests are not open source.

Write simple tests, use natural language for your 'given when then' clauses, write a test for one scenario at a time, and never write one massive test that is intended to cover everything because it will just be confusing and time-consuming.


### Design Principles in Brief

The main principles to follow when designing your BPMN are listed below, more detailed guidelines can be found at [Method and Style by Bruce Silver](https://methodandstyle.com).

* Don't over complicate your BPMNs i.e don't add too many technical constructs into your process.

* Ensure you are working with your business to build the BPMN that they actually need/want.

* For all activities use verb and noun e.g. 'approve (verb) holiday request (noun)'

* Label all decision gateways with a question mark e.g. 'holiday request approved?'.

* Business rule tasks should be names with 'determine...' e.g. 'determine if request needs approval'.

* Use 'call activities' carefully. A re-usable subprocess can be used in other processes. Before creating a one-off subprocess investigate whether it can be a reusable one rather than an embedded one that is only applicable to one particular workflow.

* Don't use message events for inter-lane communication. You might be tempted to use a message event because it is often used to communicate between different pools or external services. However between lanes you should connect the activities directly, because message events put the process into a 'wait' state.

* Don't use external tasks, the COP central team are looking to move away from using external tasks. Should you need a service that isn't available please consult the COP technical team.

Following these guidelines should ensure you design effective BPMNs and test them thoroughly and efficiently.

## DMNs

DMN stands for decision model and notation. It is an industry specification for designing, modelling, and executing business rules.
The rules are based on a 'when', 'then' framework. It is represented in a spreadsheet of 'inputs' and 'outputs'.

![example DMN]({{'/images/example-dmn.jpeg' | relative_url }})

**Figure 2. Example decision model: 'Input' equates to 'when', 'Output' equates to 'then'**

You use DMNs in conjunction with BPMNs to execute. So when you have a need for complex business rules, you should use DMNs rather than creating custom Javascript or Java code to build rules. Also because it's DMN it's a specification that allows business users to understand the constructs. Normally you'd use a business rule to determine some action then use a gateway to take the path based on the outcome of the decision model.

**There can be a collection of 'Inputs' and a collection of 'Outputs'**

The DMN is built separately and then is referenced in the BPMN, so it works in a similar way to a 'call activity'.

To build your DMN, open the BPMN modeller and hover over the 'new document' icon in the top left hand corner. When the drop down menu appears select 'Create new DMN table'.

![new DMN dropdown box]({{'/images/new-dmn-dropdown.jpeg' | relative_url }})

**Figure 3. 'New DMN' drop down box**

An empty unsaved DMN table appears.

![new DMN diagram]({{'/images/new-dmn-table.jpeg' | relative_url }})

**Figure 4. 'New DMN' table**

The DMN needs a label that makes sense to a person understanding the DMN. The label should be entered in the 'Decision Name' box. In this box fill in the title of your decision model, in this instance 'Suitable Drink'. The DMN also needs an id. The id is required to uniquely identify a decision model within the run-time engine. It can be the same as the 'Decision Name', if this makes sense, or slightly different. In this example the id is 'determine-suitable-drink'.

 ![decision name and id]({{'/images/decision-name-and-id.jpeg' | relative_url }})

 **Figure 5. 'Decision Name' and decision id**

 Next populate the 'Input' column. Click on the 'add' button. This presents a modal box with the 'Input Label' and 'Input Expression'. The 'Input Label' is used to describe the data that is being fed into the decision model. In this example it is 'Meal'. The 'Input Expression' is the variable that contains the data. In this instance 'meal'.

![input label box]({{'/images/input-label-modal.jpeg' | relative_url }})

**Figure 6. Input mapping**

 Now populate the 'Output' column. Click on the 'add' button. This presents a modal with the 'Output Label' and 'Output Name'. **It is possible to have multiple inputs and outputs, but this simple example only has one of each** In this example the 'Output Label' is 'Drink' the 'Output Name' is 'drink'. The 'Output Name' is the container that holds the result of the 'Input' column. The 'Output Label' is there to make the DMN easier to interpret.

![output label]({{'/images/output-label.jpeg' | relative_url }})

**Figure 7. Output mapping**

 In this example rows 1,2, and 3 of the 'Input' column would be filled with 'breakfast', 'lunch', and 'dinner' respectively.

 Rows 1,2, and 3 of the 'Output' column would be filled with 'coffee', 'juice', 'wine' respectively. Row 4 has a default value of 'water' in case the 'Input' is none of the three strings provided.

 ![full DMN]({{'/images/full-dmn.jpeg' | relative_url }})

**Figure 8. Full DMN**

For more detailed information on DMN using Camunda visit [Camunda's DMN reference manual](https://docs.camunda.org/manual/latest/reference/dmn/).


### Testing

It is necessary to test your DMN in isolation to discover any bugs or issues, without having to wait for full deployment. Also it is quicker if you do not test it within your BPMN.

This is how to test a DMN using BPMN Assert and a Spock testing framework.

```groovy
import org.camunda.bpm.engine.test.Deployment
import org.camunda.bpm.engine.test.ProcessEngineRule
import org.camunda.bpm.extension.process_test_coverage.junit.rules.TestCoverageProcessEngineRuleBuilder
import org.junit.ClassRule
import spock.lang.Narrative
import spock.lang.Shared
import spock.lang.Specification
import spock.lang.Unroll

import static org.camunda.bpm.engine.test.assertions.bpmn.BpmnAwareTests.decisionService

@Narrative('''
A specification that asserts the correct behaviour of the determine suitable drink decision model
''')
@Deployment(resources = ['./determine-suitable-drink.dmn'])
class DetermineSuitableDrinkDMNSpec extends Specification {

    @Shared
    @ClassRule
    public ProcessEngineRule processEngineRule = TestCoverageProcessEngineRuleBuilder
            .create()
            .withDetailedCoverageLogging()
            .build()


    @Unroll
    def 'beverage for #meal should be #drink'() {
        when: 'a meal is chosen'
        def decision = decisionService().evaluateDecisionByKey('determine-suitable-drink')
                .variables(['meal': meal])
                .evaluate()

        then: 'beverage should be #drink'
        decision.first().get('drink') == drink

        where: 'meal is #meal'
        meal        | drink
        'breakfast' | 'coffee'
        'lunch'     | 'juice'
        'dinner'    | 'wine'
        'snack'     | 'water'
    }
}
```

**Figure 9. Example test: a specification that asserts the behaviour of the DMN**

This test is setting up the decision engine:

```groovy
@Shared
@ClassRule
public ProcessEngineRule processEngineRule =   TestCoverageProcessEngineRuleBuilder
        .create()
        .withDetailedCoverageLogging()
        .build()
```
It is possible to write a single test for all the input data:



```groovy
@Unroll
def 'beverage for #meal should be #drink'() {
  when: 'a meal is chosen'
  def decision = decisionService().evaluateDecisionByKey('determine-suitable-drink')
          .variables(['meal': meal])
          .evaluate()

  then: 'beverage should be #drink'
  decision.first().get('drink') == drink

  where: 'meal is #meal'
  meal        | drink
  'breakfast' | 'coffee'
  'lunch'     | 'juice'
  'dinner'    | 'wine'
  'snack'     | 'water'
}
```

So, all the input data 'breakfast', 'lunch', etc can be tested one at a time within the same test, as above. This means you do not have to write a test for each input/output instance.
The test then outputs 'Test Results' showing which 'Inputs' provided the correct 'Output' and showing up any that did not.

![test results]({{'/images/test-dmn-result.jpeg' | relative_url }})

**Figure 10. Test results**

There is room to make loads of mistakes in a DMN so it's a good idea to create separate tests for your DMN so you don't waste time looking for problems in your entire BPMN that are caused by the DMN.

If your DMN is in production and your users discover a bug. You can write a test case for that bug, reproduce the issue and then fix the DMN and then re-run the test to make sure you've fixed it properly.

**It is important to write a test to recreate the bug first, rather than fixing the bug and then testing it afterwards, as you need to be sure you are fixing the actual issue.**
