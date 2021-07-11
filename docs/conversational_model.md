---
id: conversational_model
title: Conversational Model
---

### Chatbots as agents

>OpenDialog takes an agent-based view of a chatbots.

Multi-agent systems development is a subfield of AI that studies the possible architecture of individual agents as well as the architecture of systems that allow multiple agents to coordinate towards both individual and joint goals.

OpenDialog models the interaction between the user and the chatbot as that between agents in a multi-agent system. In particular, we draw from work around around [electronic institutions](https://nms.kcl.ac.uk/michael.luck/resources/aij12.pdf) to describe the overall context within which users and bots operate in. In addition, we looked at [enhanced Dooley graphs](https://pdfs.semanticscholar.org/71e5/f7aee39470470c1bed39bf11790d8df4cccd.pdf) for the added flexibility in  representing the exchange of messages between participants in a conversation. 



## Modelling conversational applications as Electronic Instututions

In OpenDialog conversational applications are modelled as _electronic institutions_ (EIs). 

An EI is "an organisational structure for coordinating the activities of multiple interacting agents". 

It mimics the concept of a conventional institution, as a space for coordinating the activities of individuals and transposes and evolves those concepts to fit the needs of a digital setting.


  The abstraction of an EI, and the semantics it offers, provides the conceptual framework for capturing the activities of interacting agents in a conversational setting and describe them in a consistent and coherent way. 

Almost anything we do in day to day life is a type of social institution. The way we behave in a restaurant (walk in, get seated, orders taken, etc), the way we behave in a shop, the way we behave at the office and so on. All these institutions define some norms and regulations about how everyone within an institution is supposed to behave so that the purpose the istitution was designed or has evolved for is achieved. For example, if you walk in a restaurant and start throwing things around or swearing at other guests you will be asked to leave. It is not acceptable behaviour. If you walk into a restaurant and ask for something of the menu before you were seated at a table you might be asked to wait to be seated, that is the norm of how the 

> In OpenDialog _every_ participant, from bots to human users, is modelled as an agent participating in an EI. We use the same concepts and abstractions for both throughout giving us a *coherent* model of the system through a common set of concepts. 

Before we go on to the specifics of EIs and how we use them, there are four core characteristics that are worth highlighting as they underpin the overall design and approach of EIs. 

### An EI models social activities

The agents in our EI need to cooperate and coordinate to achieve their goals. The bot needs to ask the right questions and the user needs to provide relevant information in order to further the overall goals. In designing the system we need to cater for how to make both the bot (or bots) and the human (or humans) aware of what role they are playing in a given situation and what the expectations are of their role in that situation in order to further the overall goals. 

### An EI caters for decomposable activities

Activities are decomposable. A larger goal (e.g. order a pizza to be delivered to a specific location) can be decomposed into smaller activities (choose restaurant from which to order, define order, define location, deliver, pay, rate experience, etc). These smaller activities each require an exchange of _utterances_ in different stages with each stage achieving a specific goal. We call the different stages _scenes_. _Scenes_ can be composed into larger _conversations_.

_Conversations_ have a specific purpose and a well defined communication protocol. Networks of scenes represent the progress of a conversation through different phases with potentially different constraints and relevant context. As the conversation progress through scenes different _activities_ are also completed, each taking us closer to the final goal. 

### An EI is an open system

Openess from an EI perspective means that agents can enter or leave the system at any point. The design of conversations and the management of knowledge needs to cater for constant and unpredictable change. Crucially for dialogical systems with humans involved, handling the unexpected gracefully is key. 

### An EI is a dialogical system

Activities within an EI are achieved through the exchange of utterances and the performance of activities. Activities can change or reveal states of the world the EI is in, either as a direct result of an utterance (e.g. the utterance "cook a pizza" will lead to the activity of cooking a pizza) or because an activity is required to be able to provide a reply to an utterance (e.g. the utterance "are all doors closed?" may lead to the activity of collecting information from all doors about their state).

## Representing conversations

An EI is made up by the sum of possible conversations that participants in the EI can have. A conversation captures some higher order goal. 

Conversations have a specific structure that we can reason about within OpenDialog. Below is simple conversation with just two scenes and a connection between the scenes. 

![alt-text](assets/example_conversation.png)

There are a few things going on so let's break it down. 

### A conversation is represented as a graph.

The graph allows us to capture the different scenes, the participants in each scene and the exchanges between participants as well as any other information such as actions. The graph structure within a graph database enables us to explore the conversational space flexibly and efficiently. 

### A conversation is divided in scenes with participants exchanging intents

Participants in scenes intent to _say_ intents and _listen_ to intents. 

A conversation can develop within a single scene or it can branch out to different scenes. Scenes are connected through intents that one participant in one scene says and a participant in an other scene listens for.

Each scene has a specific sub-goal, the sum of goals giving us the overall objective. 

### Intents can cause actions

When OpenDialog has determined that a participant said a specific intent and that intent is associated with an action that action is performed. 

## Reasoning about conversations

The task of OpenDialog is to facilitate the definition of conversation, starting from the core concepts defined here, and manage the processes of _running_ this conversations through a conversational interface. 

As you dive into other aspects of the document you will see how these basic concepts find practical implementation and are able to support a variety of different conversational patterns - from very simple ones to increasingly more sophisticated ones. 



