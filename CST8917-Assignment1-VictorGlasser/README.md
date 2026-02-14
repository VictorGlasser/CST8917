# Assignment 1
Victor Glasser

CST8917

Serverless Computing - Critical Analysis

Feb 13th, 2026

## Part 1: Paper Summary
The paper starts by introducing serverless computing and functions as a service (FaaS) as well as explaining that they were a popular time when this article was written. The paper explains that serverless options in public cloud providers were practical at the time of this article's writing and gives the reader more context on AWS’ implementation of FaaS, Lambda, going into details such as what resources Lambda can interact with. The paper then explains that the practical uses for a function such as this are limited at this point in time, limited to niche scenarios such as very parallel functions where functions don’t have to interact with each other and orchestrating calls to autoscaling services. The paper asserts that FaaS options such as Lambda cannot be used for complex workloads and have several severe drawbacks. For Lambda these problems include a bandwidth that reduces as more functions are called, a lifetime of only fifteen minutes without a guarantee the same VM will be used for the next run, the reliance on intermediary services like S3 to communicate, and pre-defined options for CPU and RAM for lambda functions. This makes FaaS impractical in many ways including lower speeds due to the need to use a database to communicate between functions and the tremendous reduction in bandwidth due to AWS putting several Lambda functions on one VM. The paper goes into case studies which go into detail on the magnitude of these problems. One study showed that lambda was 21 times slower and 7.3 times more expensive than using EC2, another reported 127 times faster results for a serverful implementation using a SQS as apposed to lambda, and 57 times cheaper. At this point the paper takes a turn, looking at the potential benefits of the limited nature of FaaS. One point that the paper brought up is that limitations force us to simplify things and looking for a simple solution is beneficial for debugging. The paper also says that limitations result in architectural and platform innovation. The article then responds to questions and statements that they have heard about their work, such as informing the audience that they are not against serverless computing, but they wish to inform readers of the limits of serverless computing at the time of this article’s release. The article finished by declaring the components of the environment that are missing most when FaaS was used, such as the ability to send code to data as opposed to data to code, the offer of specialized hardware to meet the needs of the workload, persistent virtual agents with known identities, and small units of data and computation that can be moved around as needed. (Hellerstein et al., 2019)


## Part 2: Azure Durable Functions Deep Dive
### Orchestraion model
#### How do orchestrator, activity, and client functions work together? How does this differ from basic FaaS?

According to Azure, orchestrator functions determine how "actions are executed and the order in which actions are executed" (Microsoft, 2022) and the functions that they order are activity functions. The orchestrator function ensures that each activity function is run at least once during an execution(Function Types in Azure Durable Functions, 2022). Client functions are responsible for triggering orchestrator and entity functions through their respective trigger bindings that client functions target. The difference between this and basic FaaS is that basic FaaS is not stateful. This is significant because there could be race conditions, where the program only works properly if one chain of functions is completed before another set of functions reaches a specific point.


### State management
#### How does Durable Functions manage state (event sourcing, checkpointing, replay)? How does this address the paper's criticism that functions are stateless?

Event sourcing is when the series of events taken on an object is stored as a record of what has occurred thus far. (Microsoft, 2026) Checkpointing is when the durable task framework saves the execution state of the function at various checkpoints into a back end (Durable Orchestrations - Azure Functions, 2025). During a replay, the durable function retrieves the result of a function that has already been run and then the orchestrator continues to run from that point (Durable Orchestrations - Azure Functions, 2025). Regarding the paper's criticisms, although the functions themselves are technically stateless, if data is ever lost, these are external stateful elements which are in place to describe how a point was reached and what the state was at a certain point, which can then be worked from to re-obtain the lost data.

### Execution timeouts
#### How do orchestrators bypass the timeout limits that apply to regular Azure Functions? What limits still apply to activity functions?
As we learned in class, orchestrator functions are able to bypass the timeout limits that apply to regular Azure functions through a process called dehydration. During the dehydration process, they are paused and preserved along with their storage, waiting for an activity timer or external event (Week 4, slide 19). Unlike orchestrator functions, activity functions are not dehydrated, however they can be if the user has selected either a premium or a dedicated plan.


### Communication between functions
#### How do orchestrator and activity functions communicate? Does this address the paper's criticism about functions needing slow storage intermediaries?
Orchestrators and activity functions communicate with each other through orchestrator functions. These functions can be called synchronously or asynchronously, they are durable, and their progress is saved as a checkpoint so that they aren't lost thanks to durable functions' ability to replay and recover previous states along with the data that was present at that time. they can span for as long as the user needs them to last, similarly to a database, but without the slow additional step. The criticism in the paper that slow storage intermediaries is resolved since they are no longer necessary (Durable Orchestrations - Azure Functions, 2025). 


### Parallel execution (fan-out/ fan-in)
#### How does the fan-out/fan-in pattern work? How does it address the paper's concern about distributed computing?
In the fan-out/fan-in pattern, multiple functions are executted at the sam time and then a an aggregation is performed on all of the results. (Fan-Out/Fan-in Scenarios in Durable Functions - Azure, 2023). fan-out/fan-in shows us how several tasks can be completed by several different functions in a distributed way. A significant issue with a distributed approach without durable functions is that it is that, even if there is a problem that causes a function to not fully complete its task, the orchestrator notices this and creates a new activity function. This way, even if the system is distributed, one can be confident that no element of the end result is missing.

## Part 3: Critical Evaluation (400-600 words)
I would argue that the progress over the past nine years shows that cloud providers took critisisms like those of the authors and developed genuine solutions to issues as opposed to mere work-arounds. A very significant aspect of this is the complexity of the workflows that serverless functions allow users to architect. As discussed in previous sections, durable functions have resolved several of the issues discussed in the article, such as the lack of control that architects had over the sequence of tasks and the lack of persistent storage. The lack of customizability of RAM and CPU has been resolved with time and interest in the feature. On AWS' price calculator, there are currently 483 available instances (AWS, 2024).

## References
AWS. (2024). AWS Lambda – Pricing. Amazon Web Services, Inc. https://aws.amazon.com/lambda/pricing/

Cheranga Hatangala. (2020, August). Azure Durable Function Patterns — External events. Medium; Cheranga. https://medium.com/cheranga/azure-durable-function-patterns-external-events-5e545b40f2d7

Durable Orchestrations - Azure Functions. (2025, December 11). Microsoft.com. https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-orchestrations?tabs=csharp-inproc

Fan-out/fan-in scenarios in Durable Functions - Azure. (2023, March 23). Microsoft.com. https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-cloud-backup?tabs=csharp

Function types in Azure Durable Functions. (2022, June 17). Microsoft.com. https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-types-features-overview

Hellerstein, J., Faleiro, J., Gonzalez, J., Schleier-Smith, J., Sreekanti, V., Tumanov, A., & Wu, C. (2019). Serverless Computing: One Step Forward, Two Steps Back. https://www.cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf

Microsoft. (2026). Event Sourcing pattern - Azure Architecture Center. Learn.microsoft.com. https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing

## AI disclosure
I used ChatGPT to
* Explain the "Heterogenous Hardware Support" section on page 7 and to 
* Ensure that Microsoft documentation was exhaustive (ex. do orchestrators and activity functions only communicate through the orchestrator functions)
* Generating examples from the text to discuss in the final question.