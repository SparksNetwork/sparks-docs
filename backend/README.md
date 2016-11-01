# Backend overview

> A place behind the middle of nowhere and below the back of beyond, which is significantly less pleasant to visit than either.

The backend is a set of server-side processes that run Sparks.Network business logic and persistence (i.e. saving things to a database).

Sparks.Network backend attempts to implement Kappa Architecture (http://milinda.pathirage.org/kappa-architecture.com/) whereby all canonical data is represented in a "stream" or log. Anything not in the log is ephemeral, it can be rebuilt by having an appropriate service "replay" the log.

### Components

* **Dispatcher** - https://github.com/SparksNetwork/backend-dispatch A program that takes inputs from users and adds events to a stream
* **Invoker** - https://github.com/SparksNetwork/backend-invoker A program that listens to events on streams and sends (invokes) another program to process them
* **Services** - https://github.com/SparksNetwork/backend-services A set of programs that take an event of some kind and do something with the event. **This is the core part that implements the domain logic**
* **Schemas** - https://github.com/SparksNetwork/schemas A set of Json-Schema files that describe the allowed messages that can pass between clients and the dispatch, dispatcher and streams, streams and invoker, invoker and services, services and data at rest.

### Terraform

To deploy infrastructure a tool called terraform (http://terraform.io) is used. The terraform definition has a repository https://github.com/SparksNetwork/terraform.

Terraform is responsible for setting up the required compute infrastructure.

### Kafka

Kafka is a message broker that works on the basis of an append-only log or stream. Kafka maintains a number of topics, each of which has a number of partitions. Each partition is a log.

The *dispatcher* and *invoker* programs communicate with Kafka directly.

### AWS

Sparks.Network compute infrastructure is hosted on AWS. Kafka is deployed to an ECS cluster, the dispatcher and invoker programs are deployed to an ECS cluster, and the services are deployed as AWS Lambda functions.

### Firebase

Sparks.Network frontend client uses Firebase hosted by Google. This is a No-SQL database that provides realtime updates to connected clients.

As per the Kappa Architecture the data in Firebase is epehmeral and is written based on events, so it is possible to rebuild the database from scratch at any point in time.

## Typical operation

Here is how a typical user action would be processed. Profile creation is one of the first interactions a user has with Sparks, and it looks like so:

1. User fills in profile form and clicks save
2. Client creates a command payload that fulfills the schema Profiles.create and writes this to a special location in Firebase (!queue/tasks).
3. Dispatcher sees command payload. It performs the following steps:

    a. Validate the payload against the Profiles.create schema
    
        Failure: write to responses location in firebase and abort.
    
    b. Applies authorization rules - the user is allowed to perform this action. In this case there must be no existing profile for this user id.
    
        Failure: Write to responses location in firebase and abort.
        
    c. Send command message with payload to Kafka cluster and write success response to firebase.

4. Client / User now sees that response is accepted
5. Invoker consumes the command message from Kafka.

    a. The invoker has a list of services with configuration
    
    b. Each service lists the topics it is interested in. For this example the invoker will select the services interested in the "commands" topic.
    
    c. Each service lists schemas it understands. For this example the invoker will select any service that understands command.Profiles.create.
    
    d. The invoker now has a list of services. These are the "profiles" service and the "gatewayCustomer" service.
    
    e. The invoker will invoke these two services in parallel
    
6. Profiles service receives the profile data. It performs a transformation and returns a new message destined for the "data.firebase" stream.

7. GatewayCustomer service communicates with Braintree, the payment gateway, and creates a new customer in that third party system for the profile. It now returns a new message destined fo the "data.firebase" stream.

8. Invoker takes the returned messages from "profiles" and "gatewayCustomers" and puts them on the "data.firebase" stream in Kafka.

9. Invoker consumes the messages on the "data.firebase" stream.

    a. Invoker filters the functions by message as per step 5.
    
    b. Invoker finds the service "firebase"
    
    c. Invoker invokes the firebase service with the new messages
    
10. The firebase service receives the messages and writes data to Firebase.

11. The client receives the data updates via Firebase and updates the screen.

    
--
As we can see this is a complex operation to perform a simple task. While this complexity is a definite *con* to the Kappa architecture, it is also an enourmous plus in that:

* The profiles, gatewayCustomer and firebase services are each very simple, consisting of only a few lines of runtime code.
* Each service can be tested in isolation
* Each service has no knowledge of the other services, even downstream ones.

**Note:** Reader would be correct to point out that it is a mistake to have the profiles service output messages to the "data.firebase" stream, as this is implementation specific. It would be better had this been named something generic.

* We can add a new service that listens to profile creation at two places in the lifecycle, these being at the command level and at the data level.






