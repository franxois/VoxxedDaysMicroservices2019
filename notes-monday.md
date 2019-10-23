# Conference

monday schedule https://voxxeddays.com/microservices/monday-schedule/

## Get started with microservices on kubernetes

date : lundi 21 11h15

conseil : regarder les projets de la CNCF

main interest : manage application instead of managing directly machine. Define the application state
Describe the target we want to achieve

Dans un service, `selector` sert à linker le pod

Secrets are encrypted when stored. If we stole a backup, we don't have access to secret (?)

We can use SecretMap to create env variables

When CI/CD don't work, we have to get our hand dirty
debuger en utilisant `kubectl exec .... sh`
In production, think to remove shell from container

`kubectl prot-forward ...` to access pod port from local machine

## Cloud run, serverless containers in actions

lundi 12h15

Advantage of container without having to manage plateform
any stateless container : because auto kill or auto scale
scale down to zero

clound run :

- gke
- Anthos (?)
- Knative

container contract
listen on \$PORT
request time : 15m will be extend to 1h
stateless
no background activity after request

pay per use : increments by 100ms
up to 80 concurrent request

you're billed for the time of concurrent request, not by request

Compared to function aas you have to hande well concurrent requests

PubSub to exchange messages. you can use PubSub as communication layer

After talk : pubsub "transform" into http request
We can have 80 concurrent calls by deploy, up to 1000 or 10000 deployments, after we have to call comercial to unlock

## Quick about event sourcing

15min

Notion of EventLog : very simple datastore

## CI/CD for modern application

( ex- Supercharge product development with cloud best practices )
13h30
guy from AWS

lower the barrier to test new idea => increase innovation
no infrastucture => more agility, no plumbery

trunk, mainline should be buildable at every moment. If trunk failed, everybody should work on it

infrastructure as code goals

avoid snowflake servers : from far all the same but all different

methods:

- bash script : cannot update deployment, imperative way
- declarative way : cloud formation on aws

SAM : serverless application
SAM template, very simple way to define everything

AWS CDK cloud development kit
CDK : you can use your favorite langage to do loops

Bad side of CI system : you need to patch, manage, update, autoscale CI service

aws CodeBuild : fully managed, paid by minutes

CD :
Lot of small deployment => easier to rollback
Important to have good metrics. Netflix : monitor play by minutes, if it goes down => there is a problem

reduce time between "make a change" and "have a change available for customer"

canary deploy / blue green deploy

[Demo of CodeBuild/CodePipeline]

## From Zero to Hero with Kafka Connect

Streaming integration with kafka connect
Avoid to reinvent the wheel

Kafka persists data

Kafka connect : framework
Bridging to an existing system to another

suscribe to database trasaction, push to kafka topic

repush data to log4j database or elastic/kibana

lo4j browser look cool

Kafka connect is fantastic but:

- it can be a pain. High level of abstraction

kafka connect :

- connector : how to integrate with particular technologie
- -- connect record --> OR Transform(s) to transform data
- converter

You have to use schema ! Define schema in your bus message system to avoid having non formated data in your system

Transform to transform data : very ugly syntax

Confluent Hub to find shared transform hub.confluent.io

2 ways to run kafka connect:

- standalone
- distributed : best choice

kafka connect runs in JVM

Kafka connect cluster. Config stored in kafka in distributed mode, files in standalone mode

how to troubleshoot :

TODO : see what is confluent, what they do ?
confluent cloud, fully managed kafka saas

try Ksql ? KaSQL ?

## Helm your way with Kubernetes

Helm 3 : release name scoped to different namespace. With helm 2, we can't use same release name in different namespace

TO DO : use more `tree` command to show file structure

pers service aproach

master chart approach : better than umbrella ? requierment all in charts/
in helm 3, no more requirements.yaml

Hybrid approch

## Transactions in your micro-services architecture

MicroProfile LRA (Long Running Actions)

CAP Theorme
consistency - Availability - Partitionning : 2 of 3, not all

Saga pattern : ensure that each step has compensation action

Demo of microprofile framework : JAVA with annotations
... or demo of Payara ?

## Creating event-driven microservices: the why, how and what

Microservices and Kafka

TODO : try to find the conference made in the same time (Event-Driven Microservices, the Sense, the Non-sense and a Way Forward)

Data centric or event centric

presence of "event backbone"

link in the bottom of slides :
https://ibm-cloud-architecture.github.io/refarch-kc/design/readme/

microservice = use the database by service pattern
benefits : losely coupled, right database for the right job
drabacks ; transaction spanning services is hard, query spanning db hard

"compensating action" what to do when distributed actions fails

pettern named SAGA

How to update database and publish event in the same time ?

response is pattern 3 : Event sourcing

No database, everything in the event backbone

pattern 4 is CQRS Command Query Responsability Segregation

Kafka is a distributed system, one or more cluster
It has topic and partitions

Data replication
"Compaction" might help to split databases
Compaction eliminates duplicate keys, keep newest value

"Kafka Connect" to read db change log and put it in kafka.

https://debezium.io Debezium is an open source distributed platform for change data capture.

Every items with the same keys will be put in the same partition

Kafka streams : map filter function in JAVA

## Debat at the end of the day

Multi cloud :
AWS : you will limit yourself to the common fonctionnalities
Ana : avoid vendor lock. Go multi cloud because you need it, not because it is fancy

monolith to microservice : best to have more than one database
spliting complexiity

building application is easy, managing application is hard
Shifty mentallity from "reusable" to make something "good"
smaller team independant. Organisation adopting different philisophie, different mentality

Cost of micro service :
"Cloud Guru" reduced cost with cloud, "Bonduel" see massive cost reduction
container driven deployment

"try, fail fast and fail cheap"

To do microservices, to need confidence and good comunication between teams

With microservice, we stop financing for project, we finance a team to do a microservice. Organisation has to shift it's vision
