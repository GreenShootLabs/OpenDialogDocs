---
id: conversation-markup
title: Conversation Markup Basics
---

## Conversation Markup Basics

Conversations can be defined using a YAML file. This makes them easy to version and share and easy to understand. 

Each YAML file holds a single conversation. Each conversation has an identifier which needs to be unique within a group of conversations that an agent should be dealing with. 

### Conversation ID

```yaml
conversation:
  id: remaining_tasks_conversation
```

### Conditions

A conversation can have conditions associated with it. Conditions are a list of statements that need to evaluate to true for the conversation to be considered. For example:

```yaml
conditions:
  - last_message_posted_time: 10000
    operation: ge
```

The above condition statement will ask the conversation engine to verify that the value of the attribute `last_message_posted_time` is `greater than or equal` to 10000. 


The allowed operations are

```
le - less than or equal to
lt - less than
ge - greater than or equal to
gt - greater than
```

If the conditional check is for a boolean no operation is required. 

```yaml
conditions:
  - user_is_logged_in: true
```

All the conditions associated with a conversation must evaluate to true for the conversation to be considered.


### Scenes

A scene represents an exchange of intents between participants in a scene.

Scenes can have conditions defined that need to evaluate to true (just like conversations).

Each scene has an identifier which is also the key of the scene object in YAML. Exchanges of intents are defined by the identifier for the participant expressing that intent and an identifier for the intent. 

```yaml
scenes:
  opening_scene:
    intents:
     - u: remaining_tasks
     - b: task_report
```

The `welcome_scene` above captures the exchange between the user (`u`) and the bot (`b`). The user starts the conversation with the opening intent `remaining_tasks`. Th `u` and `b` are default identifiers.

In real terms this means that the user asked something like `tell me what other tasks I have to do` or `give me my remaining tasks` or `fetch unfinished work`. All these _utterances_ can be mapped to a _single_ intent.

The bot replies with a message that has the intent of presenting the user a task report. 

#### Interpreters

In order to interpret utterances from users to specific intents we use interpreters. OpenDialog supports both an application-wide default interpreter that can be used for all utterances, but conversations, scenes and intents within scenes can define their own interpreters.

```yaml
scenes:
  opening_scene:
    intents:
     - u: remaining_tasks
       interpreter: remaining_tasks_interpreter
     - b: task_report
```

When the conversation engine is attempting to match an utterance to an intent and considers the `welcome_scene` of the `task_management` conversation it will ask that the utterance be given to the `remaining_tasks_interpreter` for interpretation. 

#### Actions

After a user intent is matched or a bot intent is selected to generate a response utterance we can define an action to be performed. In the background, the results of that action will be available for the agent to use to render the response or do any other type of reasoning required. 

```yaml
scenes:
  opening_scene:
    intents:
     - u: 
         i: remaining_tasks
         interpreter: remaining_tasks_interpreter
         action: retrieve_remaining_tasks
     - b: task_report
```

#### Completing conversations

A conversation is considered complete when the last utterance of the last scene is reached or an explicit `completes:true` statement is reached.

```yaml
scenes:
  opening_scene:
    intents:
     - u: 
        i: remaining_tasks
        interpeter: remaining_tasks_interpreter
        action: retrieve_remaining_tasks
     - b: 
        i: task_report
        completes: true
```

### Multiple scenes

As conversations move between different states they can change scenes. For example, consider how we would represent the following possible exchange:

_\-User:_ Create a new project

_\-Bot:_ Would you like to start from a template or from scratch?

_\-User:_ From scratch

_\-Bot:_ What is the project called?

_\-User:_ OpenDialog

an alternative exchange would be:

_\-User:_ Create a new project

_\-Bot:_ Would you like to start from a template or from scratch?

_\-User:_ From a template

_\-Bot:_ What template would you like to use?

_\-User:_ Conversational project template

_\-Bot:_ What is the project called?

_\-User:_ OpenDialog

In the above exchanges the goal is the same, the use wants to create a new project. In the first instance, however, the user is starting from scratch while in the second they are starting from a template. Our task is to represent the full range of exchanges in a single conversation. 

There are three key scenes. The opening scene, where the user asks to create a project and the bot asks what type of project and then two potential follow-on scenes, one leading to choosing a template to start the project from and the other creating a "blank" project.

```yaml
conversation:
  id: create_a_new_project
  scenes:
    opening_scene:
      intents:
        - u: 
            i: create_new_project
            interpreter: create_new_project_interpreter
        - b: type_of_project
        - u: 
            i: project_from_template
            scene: project_from_template
        - u: 
            i: new_project
            scene: new_project
    project_from_template:
      intents:
        - b: select_a_template
        - u: template_type
        - b:
            i: confirm_template
            transition:
              scene: new_project
    new_project:
      intents:
        - b: request_project_name
        - u: 
            i: project_name
            no_match: 
              b: clarify_project_name_request
              attempts: 2 
              transition: handle_failure     
        - b: 
            i: confirm_project_name
            completes: true
```

#### Opening Scene 

The `opening_scene` would match if the `create_new_project_interpreter` matched the utterance to the intent `create_new_project`. 

The bot would then respond with a message that carries the intent `type_of_project`. 

If the user replies with `project_from_template` this would move the conversation to the `project_from_template` scene, while if the user replied with an utterance that matches `new_project` we would move the conversation to the `new_project` scene. 

#### Project from template scene

In the `project_from_template` scene the bot askes the user to select a template type, the user replies with their selection and then the bot confirms the select. The conversation is the transited to the `new_project` scene. 

#### New Project scene

In this scene the bot requests the project name, the user provides it and confirms. If the user replies in a way that cannot be matched to a `project_name` intent (e.g. they use characters in the project name that are not valid or use a name that is already taken) the bot will attempt twice to clarify the issue before transitioning the conversation to a `handle_failure` scene (not represented here).


### Conditional branching based on missing information

Another need for multiple scenes is so that we can collect information that is required to fulfull a request. 

_\-User:_ Create and assign a new task to me

_\-Bot:_ What project should the task be under?

_\-User:_ OpenDialog project

_\-Bot:_ What's the task?

_\-User:_ Update the OpenDialog documentation


From a human perspective this is a really simple conversation. From a conversation design perspective, however it can get quite complicated. Simple consider these some potential requests from the user:

_\-User:_ Create and assign a new task

_\-User:_ Create and assign a new task to me

_\-User:_ Create and assign a new task called "Update documentation" to me

_\-User:_ Create and assign a new task called "Update documentation" to me under the "Open Dialog" project

How do we capture all the possibilities in a way that would allow for variation to the conversation, for different pieces of information to be made available at different points in time or in a different sequence, etc. 

Here is how you could represent this using OpenDialog's markup language.

```yaml
conversation:
  id: create_a_new_task
  scenes:
    opening_scene:
      intents:
        - u: create_a_new_task
          attributes:
            - id: task.name
              if-not-present:
                transition:
                  scene: request_task_name
            - id: project.name
              if-not-present:
                transition:
                  scene: request_project_name
            - id: asignee.name
              if-not-present:
                transition:
                  scene: request_assignee_name
          actions:
            action: create_task
        - b: task_creation_report
          completes: true
    request_task_name:
      intents:
      - b: request_task_name
      - u: task_name
        no_match: clarify_task_name_request
        attempts: 2      
      - b: confirm_task_name
      - u_virtual: create_a_new_task
        scene: opening_scene  
    request_project_name:
      intents:
      - b: request_project_name
      - u: project_name
        no_match: clarify_project_name_request
        attempts: 2      
      - b: confirm_project_name
      - u_virtual: create_a_new_task
        scene: opening_scene
    request_project_name:
      intents:
      - b: request_assignee_name
      - u: assignee_name
        no_match: clarify_assignee_name_request
        attempts: 2      
      - b: confirm_assignee_name
      - u_virtual: create_a_new_task
        scene: opening_scene      
```

The key is in the opening utterance from the user. We are expecting an utterance that matches the intent `create_a_new_task` with 3 potential attributes contained within the utterance. In general, utterances can always contain useful attributes that we can extract and in conversation design we can choose to explicitly deal with the presence or absence of an attribute or not. 

In this case we are explicitly dealing with the situation where the attribute has not been detected in the utterance. If an attribute is missing we move to a scene whose goal is to collect the required utterance. These scenes complete with a special type of transition `u_virtual` - this will transition the conversation to the opening_scene and force the conversation engine to reconsider the situation _as if_ the `create_a_new_task` intent was said. 

With each pass through the opening scene we should have additional information until we have all the required attributes and can perform the action of creating the task.  

