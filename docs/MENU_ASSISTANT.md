# Menu assistant (optional)

n8n nodes: Chat Trigger, AI Agent, a chat model, and a Google Sheets tool (Get Row(s)) on the `Menu` tab.

System message (AI Agent, Options, System Message):

```
You are the Blue Dove menu assistant at a busy counter. Answer ONLY using the Menu tool.
Never invent items, prices or ingredients. Keep replies under 3 sentences and suggest at
most 2 items. If asked about allergies, say you cannot confirm and ask them to check with
the counter. If the question is not about the menu or orders, say you can only help with
the menu.
```

Test cases:

| Input | Expected |
|---|---|
| What is quick and vegetarian? | Suggests items from the menu only |
| Do you have pizza? | Says it is not on the menu, invents nothing |
| Is the paneer wrap nut-free? | Declines to confirm, defers to the counter |
| What is the capital of France? | Says it can only help with the menu |
