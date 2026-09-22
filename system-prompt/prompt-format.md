# System Prompt Formatting

This page documents the system prompt formatting observed in use by saucepan when using a proxy as of August 2026. It's possible that this will change without notice or warning, as this is generally considered an internal implementation detail.

# The system prompt

The system prompt, on saucepan (and most other similar roleplaying platforms) will contain a number of things:

- The custom user prompt
- The character definition
- Lorebook or world entries (if there are any)
- The chat memories

The order of things is relevant because of something that I call the "lost middle" effect. Essentially, when presented with many instructions, especially instructions that conflict, LLMs try to prioritize the ones given first (because the model's training data contains many examples of texts where important things are said first) and the ones given last (because they are the most "recent" things fed into the model).

## The system prompt format

The format, derived from inspecting requests made by saucepan, is as follows:

```
%%CustomProviderPreContentPrompt%%

[ Critical Instructions ]
%%CharacterAdvancedPrompt%%

[ Formatting Rules ]
%%CharacterFormattingInstructions%%

[ Background ]
%%CharacterDefinition%%

[ Example Dialogue ]
%%ExampleDialogue%%

[ User Description ]
%%PersonaDefinition%%

[ Memories ]
- %%Memory2%%
- %%Memory1%%

[ Roleplay State ]
%%VariableName%%: %%VariableValue%%

[ Playbook Lore ]
[ %%LorebookEntryName%% ]
%%LorebookEntryContent%%

[ Lore ]
[ %%LorebookEntryName%% ]
%%LorebookEntryContent%%


```

Items surrounded by `%%` are used here to indicate where your custom content is substituted.

Note that lorebook entries are selected by Saucepan, and will fill out as much space as there is in the configured context. Lorebooks controlled by playbooks come before lorebooks attached directly. Variables included in playbooks are always passed through to the model regardless of whether or not the variable controls a lorebook entry.

Additionally, the last user role message will have the following appended to it, if the options are enabled:

```
[Follow these instructions as closely as possible in your response]
[style] length:short pov:third speak:both
Write 1-2 sentences, write from test bot's perspective in third person, you may speak for both test bot and UserPersonaName, but clearly label who is speaking or acting at all times so each remains distinct. --- %%CustomResponseInstructions%%
```

The POV, length, and whether to speak for character, user, or both are all configurable in Saucepan's response formatting modal.

Finally, if your custom provider has post-history instructions configured, they are appended as a system-role message following the final user-role message in the chat history.
