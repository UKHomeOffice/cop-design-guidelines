---
category: Workflow
expires: 2020-12-31
order: 1
---
# Developers - Run the workflow engine locally

## Introduction
It is absolutely imperative that developers run the workflow engine locally. Doing this hugely improves productivity because it allows the quick testing and correcting of BPMNs. It will take some time to set up initially but saves time and effort in the long-run.

Running the workflow engine locally allows you to:
* get quicker feedback if there's a problem with your BPMN without having to ask DevOps to look at the server logs, or use Kibana logs.

* create 'breakpoints' within the engine code to see the inputs and the outputs and get quick feedback on any problems.

* be self-sufficient and develop and test your BPMNs without having to rely on external resources or people.

Running the workflow engine locally also prevents problems such as:

* clogging up the databases in the development environment.

* deploying faulty BPMNs which then break other BPMNs requiring you to painstakingly revert all your changes.

* increasing the cost of the cloud compute resources, especially Elasticsearch, which are increased every time you save a form in the engine.  

Running the workflow engine locally is best practice and is a fundamental part of your developer toolkit.

## How to run the workflow engine locally

### Get started

The first thing you need is an Integrated Development Environment. We strongly recommend that you download IntelliJ [here](https://www.jetbrains.com/idea/download/). You can use the 'Community' edition which is free and is sufficient for your development needs. IntelliJ is an excellent tool specifically tailored for development in Java code.

You will also need to install Elasticsearch, PostgreSQL, and Redis. You can use the shared instance of Keycloak SSO, which is already configured to support a workflow engine, and saves you the trouble of setting it up locally.

### Installing Elasticsearch

You can find a step-by-step guide to installing Elasticsearch [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started-install.html#run-elasticsearch-local)

Once you have started Elasticsearch if you run the following command from your terminal:

```bash
curl -X GET "localhost:9200/_cat/health?v&pretty"
```
it should return some information about the Elasticsearch cluster. This is how you know it has installed properly. If there is a problem/exception, copy/paste it into Google and you'll find the solution.

### Installing PostgreSQL
The Camunda engine requires a relational database, so we use PostgreSQL as the data source. You can find an example of how to start a PostgreSQL Docker container [here](https://hackernoon.com/how-to-install-postgresql-with-docker-quickly-ki7g363m). You can use [this](https://www.pgadmin.org/) tool to connect to the database. This will ensure that when you start the workflow engine it will talk to the database.

### Installing Redis
Redis is an 'in-memory' database used for session management within the workflow engine. Instal Redis [here](https://www.ionos.com/community/hosting/redis/using-redis-in-docker-containers/).





### Using Keycloak
In the workflow engine there is a Keycloak configuration section. AWS Secrets Manager has all the Keycloak settings for the engine. Your DevOps can provide the following properties from the AWS Secrets Manager that you can use in your local engine:

* ```auth.url```

* ```auth.clientId```

* ```auth.clientSecret```

* ```auth.realm```

### How to git clone the workflow engine
The first thing you need to do is git clone the workflow engine by running the following command:

```
git clone git@github.com:UKHomeOffice/workflow-service.git
```
This will download the code to a directory on your local machine.
Then you have to import the project into IntelliJ. Do this by opening up the IntelliJ IDE. Click on 'file'. You will see this:

![IntelliJ open]({{ '/images/IntelliJopen.jpeg' | relative_url }})

**Figure 1. IntelliJ - select 'Open'**

Select 'Open', then navigate to the directory where the git clone has  downloaded the source code. Select the directory which contains the downloaded code and open the 'build.gradle' file.

![build.gradle]({{ '/images/build-gradle.jpeg' | relative_url }})

**Figure 2. Selecting 'build.gradle' file**


The following option should appear:

![Open as Project]({{ '/images/open-as-project.jpeg' | relative_url }})

**Figure 3. Selecting 'Open as Project'**

Select 'Open as Project'. IntelliJ will set the project up and download all the necessary dependencies.

![project structure]({{ '/images/project-structure.jpeg' | relative_url }})

**Figure 4. Project structure**

### Starting the Workflow Engine

Right click on the 'workflow service application' class, then select 'Edit 'WorkflowServiceApplication' Configuration'.

![Edit configuration]({{ '/images/edit-configuration.jpeg' | relative_url }})

**Figure 5. 'Edit 'WorkflowServiceApplication' Configuration'**

Once you have done this you will see:

![Run debug window]({{ '/images/run-debug-window.jpeg' | relative_url }})

**Figure 6. Run debug window**

In this window you need to populate the 'VM options' and you need to set the 'Environment variables'.

#### 'VM Options'

When you select the 'expand window' button on the 'VM options' bar it will expand to show an empty box:

![Empty VM options]({{ '/images/empty-vm-options.jpeg' | relative_url }})

**Figure 7. 'VM option' window expanded**

You need to populate this box with the following:

````
-Dencryption.passPhrase=test
-Dencryption.salt=test
-Daws.elasticsearch.region=eu-west-2
-Daws.elasticsearch.endpoint=localhost
-Daws.elasticsearch.port=9200
-Daws.elasticsearch.scheme=http
-Dauth.url=<CONSULT DEVOPS>
-Dauth.realm=<CONSULT DEVOPS>
-Dauth.clientId=<CONSULT DEVOPS>
-Dauth.clientSecret=<CONSULT DEVOPS>
-Ddatabase.url=jdbc:postgresql://localhost:5432/workflow-service
-Ddatabase.username=
-Ddatabase.password=
-Ddatabase.driver-class-name=org.postgresql.Driver
-Daws.s3.formData=<CONSULT DEVOPS>
-Daws.s3.case-bucket-name=<CONSULT DEVOPS>
-Daws.elasticsearch.credentials.access-key=x
-Daws.elasticsearch.credentials.secret-key=x
````


For some of these property values you will need to consult with your DevOps.

#### Environment Variables

Select the 'Environment Variables' window. A box will appear. Untick the 'include system environment variables' click on the '+' and add the environment variables using the '+' button. You will need the following variables:

![Environment variables]({{ '/images/environment-variables.jpeg' | relative_url }})

**Figure 8. Environment Variables**

The AWS Access and Secret Keys should have permissions to access both the s3 buckets. You will need to get the appropriate keys from your DevOps. Once you have completed both the 'Environment variables' and the 'VM options' click the 'Apply' button and the 'OK' button.
Then at the top of the screen select either the 'run' arrow or the 'debug' bug. The 'run' button will start the application, the 'debug' button will start the application in debug mode. This allows you to put 'breakpoints' in your code, so you can see the inputs and the outputs of the code in execution. We would recommend that you always run it in 'debug' mode which will allow you to place 'breakpoints' where you want them without having to re-start the server every time you want to add a 'breakpoint'.

#### How To Get More Detailed Error Information

By default the workflow engine logs its output to your local console as JSON. This means when an exception occurs the stack trace is not always visible or it has been stripped making it difficult to identify an exception.

To solve this problem locally, update the 'logback-spring.xml' file located in the 'resources' directory of the engine. Replace the contents of the file with the following code.

```
<configuration>
    <springProfile name="test">
        <springProperty scope="context" name="springAppName" source="spring.application.name"/>
        <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
                <providers>
                    <timestamp>
                        <timeZone>UTC</timeZone>
                    </timestamp>
                    <pattern>
                        <pattern>
                            {
                            "severity": "%level",
                            "service": "${springAppName:-}",
                            "pid": "${PID:-}",
                            "thread": "%thread",
                            "class": "%logger{40}",
                            "message": "%message",
                            "userId": "%X{userId}"
                            }
                        </pattern>
                    </pattern>
                </providers>
            </encoder>
        </appender>
        <root level="INFO">
            <appender-ref ref="STDOUT"/>
        </root>
    </springProfile>

    <springProfile name="local">
        <include resource="org/springframework/boot/logging/logback/base.xml"/>
    </springProfile>
</configuration>
```
This will now output console messages. So if an exception occurs you will be able to see the full stack trace and identify what has gone wrong.

**Remember do not commit this 'logback-spring.xml' file. Just use it locally. If you commit this you will break Kibana**
