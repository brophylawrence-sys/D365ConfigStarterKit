# Exercise: create one purchase order with Copilot

**Time:** about 10–15 minutes. **Goal:** one purchase order in Dynamics 365, created from a plain-English request, after you've checked and confirmed a preview.

Before starting, make sure the **final check** in `setup-guide.md` worked, and keep `scenario-card.md` open so you can glance at it.

## The one habit this exercise is about

When an AI assistant only *reads* data, the risk is low: at worst you get a wrong answer. When it *writes* data (creates or changes records), the risk is higher, so a person checks first. That's why we always work in this order:

1. Copilot reads the request and picks out the key details.
2. Copilot shows you a **preview** of what it plans to create.
3. **You** read that preview and compare it with the scenario card.
4. Only when it's right do you confirm, and only then does anything get created.

It takes a minute longer than letting the AI "just do it," and that minute is the point.

---

## Part 1: Ask for a preview

Read Sam's email on `scenario-card.md`, then fill in the five blanks below yourself using the email and the order-at-a-glance table. If you can't find a value, stop and ask your facilitator; don't guess or ask Copilot to fill it in. Paste the completed prompt into Copilot Chat (still in **Agent** mode) and press `Enter`:

```
I'd like to create one purchase order in Dynamics 365.

Legal entity: [fill in]
Vendor (number and name): [fill in]
Item (number and name): [fill in]
Quantity and unit price (including currency): [fill in]
Delivery site and warehouse: [fill in]

Using the d365-erp tools, look up the vendor and item in the legal entity above to
check that they exist. Read only; don't create anything yet.

Show me a preview table with the legal entity, vendor, item, quantity, unit price,
currency, site, and warehouse. Then stop and wait. Do not create anything until I
reply with the word "Confirmed".
```

**What you'll see:**

- Copilot says it's going to look things up. VS Code may ask permission before each lookup, with a **Continue** button and the tool name (for example `data_find_entities`). These are read-only lookups, so it's fine to click **Continue**. Please don't choose "Always allow"; we want you to see every step.
- After a short while, Copilot shows a small table (the preview) and asks you to confirm.

> Screenshot: Copilot's preview table in the chat.

## Part 2: Check the preview (the important part)

Put the preview next to the table in `scenario-card.md` and check each line:

- Is the **legal entity** USMF?
- Is the **vendor** US-104, Fabrikam Supplier?
- Is the **item** M0001, and is the **quantity** 10?
- Is the **unit price** 25.00 and the currency USD?
- Are the **site** and **warehouse** 1 and 11?

**If something is wrong or missing**, don't confirm. Tell Copilot what to fix in your own words, for example:

```
The quantity should be 10, not 100. Please show me the corrected preview. Don't create anything yet.
```

Then check the new preview in the same way.

**If Copilot says the vendor or item doesn't exist**, stop and ask your facilitator. The practice data might be set up slightly differently.

## Part 3: Confirm

Only when every line matches, type:

```
Confirmed
```

**What you'll see:**

- VS Code asks permission for a tool called something like `data_create_entities`. This is the step that writes to Dynamics 365. Because you've just checked the preview, click **Continue**.
- Copilot may ask to use this tool twice: once for the order itself (the "header") and once for the line with the item. That's normal: Dynamics 365 stores them as two linked records.
- Copilot tells you the order was created and gives you a **purchase order number**, usually in a format like `00000123`. Write it down.

> Screenshot: Copilot's confirmation message with the purchase order number.

---

## What just happened

Copilot doesn't "know" Dynamics 365. On its own, it can only read and write text. What it *can* do is use tools, and the Dynamics 365 ERP MCP server offered it a set of them: "find records," "create records," and so on.

So when you confirmed, Copilot chose the "create records" tool and filled in the details from your preview. The MCP server then passed that request to Dynamics 365 through its own official APIs, signed in as **you**. That means Dynamics 365 applied the same checks and permissions it would if you'd typed the order in by hand.

In one sentence: **MCP gave Copilot a safe, standard way to use Dynamics 365's own tools on your behalf, and you stayed in charge of the one moment that mattered.**

## Checkpoint: see it for yourself

Paste this into Copilot Chat, putting your purchase order number where it says `PO-NUMBER`:

```
Read-only please: look up purchase order PO-NUMBER in legal entity USMF and show me
its vendor, item, quantity, unit price, and status.
```

You should see the same details you confirmed, with a status such as *Open order* or *Draft*.

**If you have access to Dynamics 365 in your browser**, you can also look there:

1. Open the practice environment link from your facilitator and sign in.
2. In the top-right corner, check that the company shown is **USMF**. If not, click it and choose USMF.
3. Go to **Procurement and sourcing** → **Purchase orders** → **All purchase orders**.
4. In the **Filter** box, type your purchase order number.

There it is: an order in a real Dynamics 365 system, created from a sentence you typed.

## You're done

That's the whole exercise. Please leave the order where it is; your facilitator will tidy up the practice environment afterward. There's no need to ask Copilot to delete or change it.

If you'd like to see a second example of how MCP works, from the reading side, have a look at `mcp-in-dynamics-example.md`. It's optional.
