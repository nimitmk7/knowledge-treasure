## What is Amazon Lex ?
It is an AWS service for building conversational interfaces for applications using voice and text. 

It provides the deep functionality and flexibility of **natural language understanding** (NLU) and **automatic speech recognition** (ASR) so you can build highly engaging user experiences with lifelike, conversational interactions, and create new categories of products.

To create a bot, you specify the basic conversation flow in the Amazon Lex V2 console. 

Amazon Lex V2 manages the dialog and dynamically adjusts the responses in the conversation. 

Using the console, you can build, test, and publish your text or voice chatbot. You can then add the conversational interfaces to bots on mobile devices, web applications, and chat platforms (for example, Facebook Messenger).

## Benefits
1. Simplicity
2. Democratizing DL technologies
3. Seamless deployment and scaling
4. Built-in integration with AWS platform.
5. Cost-effectiveness

## Core Concepts

- **Bot** – A bot performs automated tasks such as ordering a pizza, booking a hotel, ordering flowers, and so on. An Amazon Lex V2 bot is powered by automatic speech recognition (ASR) and natural language understanding (NLU) capabilities.

- **Language** – An Amazon Lex V2 bot can converse in one or more languages. Each language is independent of the others, you can configure Amazon Lex V2 to converse with a user using native words and phrases.

- **Intent** – An intent represents an ==action that the user wants to perform==. You create a bot to support one or more related intents. For example, you might create an intent that orders pizzas and drinks. For each intent, you provide the following required information:
    
    - **Intent name** – A descriptive name for the intent. 
	    - For example, `OrderPizza`.
    - **Utterances** – How a user might convey the intent(The user’s way of expressing an intent). 
	    - For example, a user might say "Can I order a pizza" or "I want to order a pizza."
    - **How to fulfill the intent** – How you want to fulfill the intent after the user provides the necessary information. We recommend that you create a Lambda function to fulfill the intent.
        
        You can optionally configure the intent so Amazon Lex V2 returns the information back to the client application for the necessary fulfillment.
        
    
    In addition to custom intents, Amazon Lex V2 provides built-in intents to quickly set up your bot. For more information, see [Built-in intents](https://docs.aws.amazon.com/lexv2/latest/dg/built-in-intents.html).
    
    Amazon Lex always includes a fallback intent for each bot. The fallback intent is used whenever Amazon Lex can't deduce the user's intent. For more information, see [AMAZON.FallbackIntent](https://docs.aws.amazon.com/lexv2/latest/dg/built-in-intent-fallback.html).
- **Slot** – An intent can require zero or more slots, or parameters. It is essentially data fields required to capture additional information such as ‘location’, ‘check-in date’ or ‘room type’.
	- You add slots as part of the intent configuration. At runtime, Amazon Lex V2 prompts the user for specific slot values. The user must provide values for all required slots before Amazon Lex V2 can fulfill the intent. 
	- For example the `OrderPizza` intent requires slots such as size, crust type, and number of pizzas. For each slot, you provide the slot type and one or more prompts that Amazon Lex V2 sends to the client to elicit values from the user. A user can reply with a slot value that contains additional words, such as "large pizza please" or "let's stick with small." Amazon Lex V2 still understands the slot value.
- **Slot type** – Each slot has a type. It represents the format/structure of the slot values.
	- You can create your own slot type, or you can use built-in slot types. For example, you might create and use the following slot types for the `OrderPizza` intent:
	- Size – With enumeration values `Small`, `Medium`, and `Large`.
	- Crust – With enumeration values `Thick` and `Thin`.
	- Amazon Lex V2 also provides built-in slot types. For example, `AMAZON.Number` is a built-in slot type that you can use for the number of pizzas ordered.
- **Version** – A version is a numbered snapshot of your work that you can publish for use in different parts of your workflow, such as development, beta deployment, and production. Once you create a version, you can use a bot as it existed when the version was made.
- **Alias** – An alias is a pointer to a specific version of a bot. With an alias, you can update the version the your client applications are using. For example, you can point an alias to version 1 of your bot. When you are ready to update the bot, you publish version 2 and change the alias to point to the new version. Because your applications use the alias instead of a specific version, all of your clients get the new functionality without needing to be updated.

## Integrating Lambda into Lex

https://docs.aws.amazon.com/lexv2/latest/dg/lambda.html

First, let us check what is the event format for LexV2, we receive 2 inputs in a Lambda function invocation. 
1. Event
2. Context.

### Event Format
Refer: https://docs.aws.amazon.com/lexv2/latest/dg/lambda-input-format.html

Let us see some important parts of the event.
### invocationSource
The code hook that called the Lambda function. The following values are possible:

`DialogCodeHook` – Amazon Lex V2 called the Lambda function after input from the user.

`FulfillmentCodeHook` – Amazon Lex V2 called the Lambda function after filling all the required slots and the intent is ready for fulfillment.

### inputMode
The mode of the user utterance. The possible values are as follows:

`DTMF` – The user input the utterance using a touch-tone keypad (Dual Tone Multi-Frequency).

`Speech` – The user spoke the utterance.

`Text` – The user typed the utterance.

### invocationLabel
A value that indicates the response that invoked the Lambda function. You can set invocation labels for the initial response, slots, and confirmation response.

### bot
Information about the bot that processed the request, consisting of the following fields: **id, name, localeId, version, aliasId, aliasName**.

### sessionState
The current state of the conversation between the user and your Amazon Lex V2 bot.

```JSON
"sessionState": {
    "activeContexts": [
        {
            "name": string,
            "contextAttributes": {
                string: string
            },
            "timeToLive": {
                "timeToLiveInSeconds": number,
                "turnsToLive": number
            }
        },
        ...
    ],
    "sessionAttributes": {
        string: string,
        ...
    },
    "runtimeHints": {
        "slotHints": {
            intent name: {
                slot name: {
                    "runtimeHintValues": [
                        {
                            "phrase": string
                        },
                        ...
                    ]
                },
                ...
            },
            ...
        }
    },
    "dialogAction": {
        "slotElicitationStyle": "Default | SpellByLetter | SpellByWord",
        "slotToElicit": string,
        "type": "Close | ConfirmIntent | Delegate | ElicitIntent | ElicitSlot"
    },
    "intent": {
        // see Intent for details about the structure
    },
    "originatingRequestId": string
}
```

#### Intent
```JSON
"intent": {
    "confirmationState": "Confirmed | Denied | None",
    "name": string,
    "slots": {
        // see Slots for details about the structure
    },
    "state": "Failed | Fulfilled | FulfillmentInProgress | InProgress | ReadyForFulfillment | Waiting",
    "kendraResponse": {
    }
}
```

### dialogAction
Determines the next action for Amazon Lex V2 to take. The format of the object is as follows:

```JSON
{
    "slotElicitationStyle": "Default | SpellByLetter | SpellByWord",
    "slotToElicit": string,
    "type": "Close | ConfirmIntent | Delegate | ElicitIntent | ElicitSlot"
}

```

## References
1. https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
2. https://docs.aws.amazon.com/lexv2/latest/dg/how-it-works.html
3. https://youtube.com/playlist?list=PLAMHV77MSKJ7s4jE7F_k_Od8qZlFGf1BY&si=IRaCLGCjuyxNdFwx 
4. 