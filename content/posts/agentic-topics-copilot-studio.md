> Make your bot *decide*, not just *reply*.  
> This post shows how to structure topics in Microsoft Copilot Studio so your assistant can plan, call tools, and iterate — safely.

---

## Why “agentic” structure matters
Large language models can reason about multi-step tasks—but only if the conversation design gives them **clear goals**, **affordances** (what they *can* do), and **state** (what’s known vs. unknown).  
Good topic structure turns a passive Q&A bot into a *problem-solver*.

---

## Core design principles

1. **Single purpose per topic**
   - Treat each topic as a *use-case unit*: one user outcome, end-to-end.
   - Keep triggers narrow and hand off to other topics when necessary.

2. **Goal → Options → Action**
   - Start by clarifying the user’s goal.  
   - Present explicit actions or connectors the bot can use.  
   - Let the model choose between them within rules you define.

3. **State is a first-class citizen**
   - Capture important entities early.  
   - Maintain a working “plan” object through the topic flow.

4. **Prefer tools over prose**
   - If data is needed, call a connector or skill rather than narrating what you’ll do.  
   - Summarize only *after* the action succeeds.

5. **Add guardrails**
   - Include timeouts, retries, and “handoff to human” exits to prevent loops.  
   - Log every decision so you can evaluate performance later.

---------
[Trigger] → Recognize intent (e.g. “track order”)
[Clarify] → Ask for missing info (order_id, email)
[Plan] → Build a plan object {goal, inputs, next_action}
[Choose Action] → Pick which connector to call
[Act] → Execute action
[Reflect] → Check if goal reached; if not, loop (max 2)
[Summarize] → Present result and next step
[Log + Exit] → Record outcome
-----------


---


---

## Entities & memory setup

**Entities to capture**
- `order_id` – string (pattern: `^[A-Z0-9-]{6,}$`)
- `email` – string (validated)
- `consent` – boolean (true to call external APIs)

**Conversation state**
```json
{
  "goal": "track_order",
  "inputs": { "order_id": "", "email": "" },
  "plan": { "steps": [], "next_action": "" },
  "last_result": null,
  "retries": 0
}

You are a support assistant.
Your goal: achieve the user’s request using only these actions.

Actions:
1) GetOrder(order_id)
2) FindOrdersByEmail(email)
3) OfferHumanHandoff(reason)

Rules:
- Ask for missing data with one short question.
- Use GetOrder when order_id is available, else FindOrdersByEmail.
- Stop after 2 loops. If still uncertain, OfferHumanHandoff.
- Return JSON: {next_action, inputs, user_message}

Example flow (order tracking)

Trigger intent: “Where’s my package?”

Slot-fill for order_id or email.

Call planner with {goal, inputs}.

Branch:

GetOrder → call connector → return delivery info

FindOrdersByEmail → list options → user picks

OfferHumanHandoff → transfer to support

Reflect and summarize:
“Your order #123 is out for delivery. Want text updates?”

Log the result:
{topic:"track_order", outcome:"success"}

## A minimal topic blueprint

