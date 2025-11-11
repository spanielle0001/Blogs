> 🧠 Ever wonder why some Copilot Studio bots feel “smarter” — like they can actually *plan* instead of just reply?  
> The secret isn’t the model… it’s how you **structure your topics**.

---

## The shift from reactive to agentic

Most Copilot Studio bots answer questions.  
Agentic bots, on the other hand, **pursue goals**.

The difference is all about **how you design topics** — giving the model space to decide, act, and reflect instead of locking it into a single dialog path.  
When you do this well, your bot stops feeling scripted and starts feeling intentional.

---

## Core ideas to build agentic topics

### 🎯 1. One purpose per topic
Every topic should have a single, clear goal — like *track an order* or *book a meeting*.  
Keep it short, and let other topics handle side paths instead of branching endlessly.

### 🧩 2. Goal → Options → Action
Start with the user’s intent.  
Show the assistant what tools it can use.  
Let the model pick the right one, within rules you define.

### 🧠 3. Treat state like data
Don’t hide memory behind the scenes — track it openly:
```json
{
  "goal": "track_order",
  "inputs": { "order_id": "", "email": "" },
  "next_action": "",
  "last_result": null
}

