+++
title = "Designing Topics in Copilot Studio for Agentic Bots"
date = 2025-11-11T15:30:00Z
draft = false
+++

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
```

**Reusable topic flow:**
```
[Trigger] → Detect intent  
[Clarify] → Ask for missing info  
[Plan] → Build a mini-plan {goal, next_action}  
[Act] → Run a connector  
[Reflect] → Check if goal complete  
[Summarize] → Reply + propose next step  
[Exit] → Log and finish
```

---

### ⚙️ 4. Use tools, not talk
When your bot needs information, **call connectors** — don’t narrate what it “might” do.  
Do the action, then explain it to the user afterward.

### 🛡️ 5. Add guardrails
Loops, retries, timeouts, human handoff — all of these keep your assistant safe, predictable, and trustworthy.

---

## Example: order tracking made agentic

Here’s what a simple planner prompt might look like:

```text
You are a support assistant.
Your job: complete the user’s goal using only the actions below.

Actions:
1) GetOrder(order_id)
2) FindOrdersByEmail(email)
3) OfferHumanHandoff(reason)

Rules:
- Ask for missing data with one short question.
- Use GetOrder when order_id is available; otherwise, FindOrdersByEmail.
- Stop after 2 loops. If still unsure, OfferHumanHandoff.
- Return JSON: {next_action, inputs, user_message}
```

---

## Making it safe and measurable

**Keep a scorecard:**
- ✅ Success rate per topic  
- 🔁 Average turns to completion  
- 🚪 Handoff frequency  
- 💬 Sentiment after completion  

Each run can log something simple like:
```json
{
  "topic": "track_order",
  "actions": ["FindOrdersByEmail","GetOrder"],
  "outcome": "success",
  "turns": 4
}
```

That’s enough to spot patterns and improve behaviour over time.

---

## Snippets you’ll reuse a lot

**Disambiguation**
```text
I found several matching orders — which one should I check?
- {{order_id}} from {{date}} ({{status}})
```

**Consent**
```text
I’ll look up your order using your email.  
Do you consent to share it with our order system?
```

**Handoff**
```text
I couldn’t complete this automatically.  
Would you like me to transfer to a specialist?
```

---

## Quick design checklist

- [x] One clear goal per topic  
- [x] Validated entities & consent  
- [x] Limited, well-defined actions  
- [x] Guardrails: max loops, fallback  
- [x] Clean JSON responses  
- [x] Clear summary and telemetry  

---

## Final thought

Agentic behaviour isn’t magic — it’s **structure**.  
When you give Copilot Studio the right scaffolding — goals, choices, state, and exits —  
your bot stops “guessing” and starts **deciding**.

---

> 💡 *Next experiment:* Try converting one of your old, linear topics into this pattern. You’ll see the difference immediately.


