# Basics of conversing with an LLM

When you are conversing with an LLM, either in a roleplay context or some other purpose (a chatbot assistant on a website, a dedicated app, etc.), you are interacting with an LLM using its API (API stands for Application Programming Interface, it basically just means a defined way that computer programs like your web browser can talk to other computer programs like an LLM or a web app or whatever). On saucepan, the main API that is used when using a proxy is the `/v1/chat/completions` API provided by your LLM provider.

Requests to the `/v1/chat/completions` API (provided by OpenAI and most AI inference providers) typically look something like this:

```
{
   "messages": [
     {
       "content": "system prompt goes here",
       "role": "system"
     },
     {
       "content": "user's first question goes here",
       "role": "user"
     },
     {
       "content": "model's first response goes here",
       "role": "assistant"
     },
     {
       "content": "user's second question goes here",
       "role": "user"
     }
   ],
   "model": "model-name",
   "stream": true,
   "temperature": 1.0
 }
```

This is a simple example, there are many more things that could be in here, I'm focusing primarily on the use in text-based roleplaying rather than other applications that include things like tools, multimedia input/output, etc. I'm also not discussing the response sent back by the model, although it looks similar, since the parsing of that into something that's displayed to you is handled entirely by the app and is not something we need to concern ourselves with for our purposes here.

To reiterate, this is what the *request made from the user to the model* looks like. It includes the entire history of the conversation (contrary to popular belief, LLMs are computer software and are reinitialized from scratch every time they run, so it's necessary to include the history in order to create the illusion of memory—LLMs do not "remember" anything), the model name being requested (some providers will serve multiple models from the same endpoint, for example `deepseek-chat` and `deepseek-reasoner`), model settings, and whether or not the response should be streamed or delivered all at once.

There are three 'roles' in the conversation: `system`, `user`, and `assistant`. Assistant indicates that a message is a previous response from the model. User indicates that the message is a request or prompt from the end-user. System, the focus of this document, is the instructions meant for the LLM, intended to define the scope and purpose of the conversation.

There are a couple things to note here:

- LLMs have a limited context window, and in text-based roleplaying it's not uncommon for the conversation to go significantly longer than the context window in use. In most platforms, only the most recent chunk of the conversation (that fits within the defined context limit) is included, causing the LLM to de-facto "forget" earlier messages. This is a bad experience for the end user, and different platforms have different ways of dealing with it, to varying degrees of sophistication. On saucepan, this is the ostensible purpose of the "memories" feature, which must be manually created and updated by the user (saucepan+ users have an automated option, but I am not a saucepan+ user). Other platforms have similar ways of doing this, to varying degrees of success.
- Nothing stops a requester from lying about who is what role, or otherwise doing things strangely. Generally though, LLMs will expect one `system` message at the beginning, then alternating `user` and `assistant` messages. On Saucepan, when you start a new chat, the first message is an `assistant`-role message with the introductory scenario.

## Rerolling

I've noticed some persistent myths spread about the "rerolling" or "swipe" function (whereby you can generate a new response to the same message, and then choose between which two responses you prefer). I've seen a lot of people saying that rerolls send the old response *along with* the instruction to generate a new one. My testing shows that this is not true—a reroll request is *identical* to the request that generated the original message.

What this means is that there's no need to prefer a delete/re-send workflow over just rerolling.
