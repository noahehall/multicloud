# Step Functions

- serverless orchestration: any computational problem or business process that can be subdivided into a series of steps

## my thoughts

- the serverless workload worhorse

## links

- [landing page](https://aws.amazon.com/step-functions/?did=ap_card&trk=ap_card)
- [activities](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-activities.html)
- [error handling](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html)
- [tasks](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-task-state.html)
- [connecting to resources](https://docs.aws.amazon.com/step-functions/latest/dg/connect-to-resource.html)
- [standard vs express workflows](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-standard-vs-express.html)
- [dev guide intro](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)
- [integrations](https://docs.aws.amazon.com/step-functions/latest/dg/connect-supported-services.html)
- [handling error conditions](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-handling-error-conditions.html)
- [lambda service exceptions](https://docs.aws.amazon.com/step-functions/latest/dg/bp-lambda-serviceexception.html)
- [limits overview](https://docs.aws.amazon.com/step-functions/latest/dg/limits-overview.html)

## best practices

- A common use case for Step Functions is tasks that requires human intervention, such as a manual approval process in a workflow
- orchestrate long running tasks and reduce costs with wait states and callbacks
- use timeouts to avoid stuck executions
  - theres no default timeout, a tasks could be stuck forever
- be aware of step functions API limits!
  - payload size passed across steps
    - for larger payloads use S3 to store the data, instead of passing it around

### anti patterns

## features

- drag and drop interface with the workflow studio
- automate workflows across most AWS services
- visualize an develop resilent workflows for event driven architectures
- automate ETL processes, security and IT functions
- orchestrate microservices, coordinate distributed components and other large-scale parallel workflows without the need for code changes
  - retry logic, rollbacks, debugging, etc

### pricing

- do not pay for idle time, regardless of how long each state persists (up to one year).
- pay for each transition from one state to the next
  - metered by state transition
  - charged based on the number of state transitions required to execute a workflow
  - charged for the total number of state transitions across all state machines, including retries

## basics

- centrally manages a workflow by breaking it into multiple steps, adding flow logic, and tracking the inputs and outputs between the steps.
- maintains the application state, tracking exactly which workflow step your application is in, and stores an event log of data that is passed between application components
- high level

### Scaling

- automatic scaling: scales the operations and underlying compute to run the steps of your application for you in response to changing workloads

### availability

- built-in fault tolerance and maintains service capacity across multiple Availability Zones in each region

### Workflows

- a collection of states that can do work (Task states)
- determine which states to transition to next (Choice states), or stop an activity with an error (Fail states), and so on

#### States

- task: is a state in a workflow that represents a single unit of work that another AWS service performs
  - referred to by its name, can be any unique string within the scope of the entire state machine.
  - Each step in a workflow is a state.
- perform work using aws services or passing parameters to API actions of other services
- use cases
  - Perform some work in your state machine.
  - Make a choice between branches of activity.
  - Stop an activity with a failure or success.
  - Simply pass its input to its output or inject some fixed data.
  - Provide a delay for a certain amount of time or until a specified time or date.
  - Begin parallel branches of activity.
  - Dynamically iterate steps.

#### State Primitives

- wait: delays the state machine from continuing for either a duration or timestamp
  - charges
    - you incur charges for transitions, not time within a state
    - you dont incur charges for awaited lambdas;
- pass: passes its input to its output without performing any other work
  - useful when constructing and debugging state machines
- task: represents a single unit of work performed by:
  - an activity
  - lambda fn
  - passing params to API actions of other aws services
- choice: adds branching logic to a state machine
- succeed: stops an activity successfully
  - useful target for Choice state branches that dont do anything except stop the activity
- fail: stops and marks the activity as a failure unless its caught by a Catch block
- parallel: create parallel branches of activity
  - invokes multiple branches of steps using the SAME input
- map: run a set of steps for each element in an input array
- terminal states: have no `Next` field, nor need an `End` field
  - succeed
  - fail

#### States Language

- a JSON-based, structured language used to define your state machine
- States can have multiple incoming transitions from other states. The process repeats itself until it reaches a terminal state, a Success or Fail state, or until a runtime error occurs.
- All non-terminal states must have a Next field, except for the Choice state
  - Next fields must be identical to state keys

```jsonc
// callback: pause a workflow indefinitely until a task token is returned
// ALL, Retry, Catch
{
  "Comment": "about this workflow",
  "startAt": "This Task", // initial state
  "States": {
    "Some other task": {},
    "This Task": {
      "Type": "Task", // state type
      "Resource": "arn:aws:SERVICE:us-east-1-abcd:etc:etc",
      "End": false,
      "Catch": [
        {
          "ErrorEquals": ["abc", "defg"],
          "Next": "SomeState"
        }
      ],
      "Retry": [
        {
          "ErrorEquals": ["This", "or", "that"],
          "IntervalSeconds": 1,
          "MaxAttempts": 2,
          "BackoffRate": 2.0
        }
      ],
      "Fallback": {...}
    }
  }
}
```

#### integration patterns

- sequential: Iterates through each state in your state machine in sequential order
- parallel: Used to create parallel branches in your state machine
- conditional: Adds branching logic to your state machine
- loops: retry logic; Iterates on your state machine task a specific number of times
- try catch finally: for un/known errors; Deals with errors and exceptions automatically based on your defined business logic
- sagas: manage failures where each step in a distributed workflow includes compensating transactions that undo changes made by its predessor
  - its all about faking ACID across datastores: e.g. in a purchase workflow, if any step fails, there needs to be logic to rollback changes made by previous steps

#### failure management

- its often preferred to
  - handle retry and backoff logic via step functions (looping) vs in application logic
  - use try/catch/finally for unknown errors
  - ensure you're handling AWS resource, service invocation and SDK errors at a minimum
    - depending on the downstream service (e.g. lambda) will determine the type of errors you need to handle
- tasks and parallel states can use fields name Catch and Retry for handling
  - when a step reports an error, it will look through the catchers for a matching error and transitions to the state named in Next field
  - each catcher can specify multiple errors to handle

#### activities and workers

- activities: your application
- activity workers: execute application code and report success/failure

#### Express Workflows

- support event rates greater than 100,000 per second
- coordinate AWS Lambda function invocations, AWS IoT Rules Engine actions, and Amazon EventBridge events.

## considerations

## integrations

- includes SDK integrations to call over two hundred AWS services

### lambdas

- for tasks <= 15 minutes; best to use with wait states

### ECS

- fargat etasks 15 minutes

### EKS

### batch

- for batch jobs

### dynamodb

### SNS

### SQS

### Athena

### Glue

### EMR

### sagemaker

### API Gateway

### cloudwatch

### cloudtrail

### EventBridge

### IoT
