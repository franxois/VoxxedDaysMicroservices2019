## split monolith

... at the end we had only several service in a monolith

1 monolith in multi repositories

There is people talking about going back to monolith

When you extract a service, it still need to call the monolith
How do you choose comunication way ? grpc, rest, kafka ?
Internal cross-services comm ? consul, istio, traefik, nginx, haproxy, envoy ??

autonomous aligned teams : all the team sgould have some agreements about comunications between services

Going to microservice is having a lot of new problems, lots of new tools to choose

## micro frontend, micro services puzzle extended to frontend

Audrey from Saagie

it's not only technical, it's organisational

Apparition of front-end monolith

Next step : micro front-end
split the frontend application and split frontend team by service

Each teams provide components

You have to use design system : saagie-ui

First try : Angularjs encapsulated in a web component and encapsulate web components
Second try : use of react fragments
third try : Layout-as-lib

tip & tricks
Think user case : aggregation of SPA or component ? Hard or soft refresh
Take care of design : design system, optimistic UI, use skeletons (placeholder when loading)
Share ressources, not runtime

... At the end, everything was in React

## Event sourcing, you are doing it wrong

... it depends context

Event driven business : reality is event sourced

As architect, everybody ask for microservices

The golden source of truth (= kafka)

An event is a fact, imutable, non changeable thing

Stream should contains low number of event < 20

To have all the user, use "query projections"

handlers are mostly trivial, prefer small aggregation

Use OCC Optimistic concurrency control : verify the data you read before did not change
... this not exists in Kafka

Don't use event version, use new event

Use weak schema, like JSON Schema, Schema-on-Read

Domain service : aggregate fields from different events, projection

Haw to deal with errors ? use "compensation event"

GDPR compliance
right to be forgotten
Fisrt solution, encrypt and forget the key => issue with key administration, what to forget, store, code complexity, dashboard

cascading delete with tombstone : delete stream then emit tombstone to other services

Use 2 streams, public and private data, only delete private data

microservice + DDD + ES = love

To try : eventmodeling.org

He recommends eventstore.org

## Breacking monolith using Pact

How to test microservice ? Not the monolithic way

Split out deployment

mock services : less ressource intensive but very hard to put in place

Solution : contract
Pact https://docs.pact.io/ : consumer driven contract

Consumer and provider
Check compatibility before deploy "should I deploy"

It is consumer driven

You should use Pact if you own producer and consumer
consumer only publish contract, producer only consume pact

Begin litle Pact test

Discution with the speaker : you replace your integration tests with pact test. It does http request and verify the response

https://github.com/pact-foundation
"The cross-language distributed contract testing framework."

## Kafka presentation

publish/subscribe : consumer can see everything

How to start : do the quick start

event:

- key (otpional)
- value

At first, don't use key

chose ack level
0 fire and forget, 1 wait for 1 broken, ALL wait for all

consumer group to avoid having to receive all events

kafka is a distributed system, with 1 or more cluster

ordering gardering into 1 partition

There is a cluster leader, but other cluster have copy of the partition and can be elected in case of crash with the first leader

## Monitoring in the Microservices world - Kubernetes Vs. Serverless based applications

Why monitor:

- know when something is wrong
- understand business impact
- find the root cause fast

Wich alert :
there is always some problems somewhere

https://spotinst.com/blog/kubernetes-ecosystem/

We can ask speaker for the slides

[Demo of lumingo]

lumigo-cli : tool to understand AWS Lambda cost

AWS Xray : tool to visualize architecture

"Open tracing" is a standard

## Looking back on migrating 30 microservices to a monorepository

6 microservices, 1 front end, different repository
more project, more repo.
1 repo for shared logic code
... 11 repository
... 31 code repository

2 branches :

- prod
- preprod

feature branch from prod, pull request to preprod, then another pull request to prod

problem :

- a dev on 3 projects => 6 pull requests. If we forgot to merge one of them => bug
- enforce coding style. `pip install flake8` on every 30 projects
- another tool has to be innstalled on all projetcs

Big problems with submodules:

- how to fix bug in shared logic ?

multi-repository code organisation causes too many issues

Large compagnies use monorepo

Google :

- simplify dependency mgmt
- code sharing
- ...
- ...

Preserve git history:

- change forlder structure in all repo
- use `git merge --allow-unrelated-histories ...`

run test fast :
existing tools for monorepo management : lerna, bazel ...
=> use of homemade tool, keep things simple

- determine wich microservice were modified
- lauch routing test for this project

Use tag for each microservice "A: v1" "B: v2"

for shared code, still have to test everything : use parallelisation

Best practice

- automated deploy process
- have rollback strategy

At the end : full success

Q/A : issue with many pull requests open ?

https://github.com/korfuri/awesome-monorepo
TODO : look for tool to manage builds or think of simple home made solution

## riff

serverless on promise

take a function, build an image
It use "kpack"

"dive" tool to inspect docker image

## Responsible Microservices

for many cloud === microservices
lot of DDD.
there is no silver bullet
"Death star architecture"

Are we getting benefits for adding complexity
When should you use microservices ?

https://content.pivotal.io/blog/should-that-be-a-microservice-keep-these-six-factors-in-mind

multiple rates of change, different parts evolve at different speed

use git to have a heat map of code changes

`CodeScene`tool to search what file changes the much, this mean code to refactor. Who's working on what

The Strangler Pattern

1 technique : run legacy and microservice in //, compare result of each request. Failback to legacy

Now we need to deliver in days, not months. Speed maters today.

smaller scope === faster to get in

A/B test

Expertise grows with repetition : deploy early, deploy often

microservice need monitoring : book "site reliability enginering"

Aggregate the data, it takes time to monitor

We need to understand business drivers

Be ralistic !

chaos enginering

polyglot stack : pick up the right tool
focus on "paved roads", you build it, you own it

microservice, keep it small : anything that can be writen in 2 weeks or less

## Reacting to the future of application architecture

Bees and reacting

moving to microservice : still have issues

look to beens for inspiration

"Reactive manifesto"

- responsive
- elastic
- resilient
- message-driven

reactive programming vs reactive systems

soccer players not necessary play together

reactive systems:

- DDD Domain driven design
- asynchronous communication
- event sourcing/CQRS
- High Availability / Eventual consistency
- database sharding
- back pressure : limit publisher
- bulheading : limit subscriber
- circuit breaking

Reactive Framework akka, lagom, play, vert.x, microprofile, rxJava

Verizon switched to reactive micro service architecture. They had issue with massive loading. Lots of advantages.

How to migrate from microservice to reactive ?
