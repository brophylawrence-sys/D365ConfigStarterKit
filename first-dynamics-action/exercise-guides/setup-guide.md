# Setup guide

**Time:** about 15–25 minutes. **Goal:** VS Code open, Copilot answering you, and the Dynamics 365 connection working.

Take it one step at a time. Each step tells you what you should see when it has worked. If what's on your screen looks different, that's fine and common: let your facilitator know and we'll sort it out together.

> **Before you begin:** make sure you have
> - the `first-dynamics-action` folder on your computer (unzipped, if it came as a .zip file)
> - the GitHub account details your facilitator gave you, for Copilot
> - your login for the Dynamics 365 practice environment

---

## Step 1: Install VS Code

1. Go to **https://code.visualstudio.com/download**.
2. Choose the button for your computer (Windows or Mac) and run the file that downloads.
3. Accept the default options in the installer and click through to the end.

**What you should see:** VS Code opens with a **Welcome** tab in the middle of the window. There's a narrow strip of icons down the left edge.

> Screenshot: VS Code Welcome tab on first launch.

If VS Code is already installed on your computer, just open it.

---

## Step 2: Open this course folder as your workspace

A *workspace* is simply the folder VS Code is working in. Opening our folder lets you see the course files, and it tells Copilot which rules and connections belong to this course.

1. In the top menu, click **File** → **Open Folder…** (on a Mac: **File** → **Open…**).
2. Find the `first-dynamics-action` folder, click it once, then click **Select Folder** (Mac: **Open**).

**What you should see:**

- A message may ask **"Do you trust the authors of the files in this folder?"** Click **Yes, I trust the authors**. This folder contains the course guides and the settings that connect Copilot to the practice environment.
- On the left, a panel titled **EXPLORER** shows `README.md`, `exercise-guides`, `.github`, and `.vscode`.
- The learner instructions are inside `exercise-guides`. You can leave `.github` and `.vscode` alone; they contain Copilot instructions and connection settings.

> Screenshot: Explorer panel listing the course files.

**Tip:** to read any `.md` file nicely formatted, click it in the Explorer and press `Ctrl+Shift+V` (Mac: `Cmd+Shift+V`).

---

## Step 3: Open Copilot Chat and sign in

1. Press `Ctrl+Alt+I` (Mac: `Ctrl+Cmd+I`). You can also click the small **Copilot icon** (a little face with goggles) at the top of the window, next to the search box.
2. If you're asked to sign in, choose **Sign in with GitHub** and use the account your facilitator gave you. A browser window opens; approve the sign-in there, then come back to VS Code.

**What you should see:** a chat panel on the right-hand side of the window, with a text box at the bottom that says something like *"Ask Copilot"* or *"Add context…"*.

> Screenshot: Copilot Chat panel open on the right.

**Your first check.** Type this into the chat box and press `Enter`:

```
Hi, can you reply in one sentence so I know you're working?
```

You should get a short friendly reply within a few seconds. That's your first win: the AI assistant is running.

---

## Step 4: Switch to agent mode and choose a model

*Agent mode* lets Copilot use connected tools (like Dynamics 365) to actually do things, not just talk about them.

1. Below the chat text box, find the small dropdown that shows the current mode (for example **Ask**). Click it and choose **Agent**.
2. Next to it is the model dropdown. Choose the model your facilitator recommends. Microsoft recommends a **Claude Sonnet** model for Dynamics 365.

**What you should see:** the dropdown now reads **Agent**, and a small **tools icon** (it looks like a wrench and screwdriver) appears in the chat box.

For this exercise, selecting **Agent** mode is all you need. You don't need to find or select a separately named custom agent or skill.

> Screenshot: chat box with mode set to Agent and the tools icon visible.

---

## Step 5: Start the Dynamics 365 connection

Your facilitator has already put the connection settings in this folder. You only need to start the connection and sign in.

1. In the Explorer on the left, click the arrow next to the `.vscode` folder to open it.
2. Check what's inside:
   - **If you see `mcp.json`**: click it to open it. Continue with step 3 below.
   - **If you only see `mcp.json.example`**: stop here and let your facilitator know. The connection hasn't been prepared yet, and it isn't something you need to fix yourself.
3. In the file, just above the line with `"d365-erp"`, there's a small grey link that says **Start**. Click it.
4. A message may ask whether you trust this MCP server. Choose **Trust**.
5. A message says the MCP server wants to authenticate (sign in) to Microsoft. Click **Allow**, then sign in with **your Dynamics 365 practice environment login** in the browser window that opens.

**What you should see:** the grey link above `"d365-erp"` changes to something like **Running | Stop | Restart | 25 tools**. The number of tools may differ; any number above zero means it worked.

> Screenshot: mcp.json with "Running … tools" shown above the server name.

You don't need to change anything in this file. You can close it now.

---

## Step 6: Confirm Copilot can see the Dynamics 365 tools

1. In the chat box, click the **tools icon** (wrench and screwdriver).
2. A list opens. Look for **d365-erp** (or **MCP Server: d365-erp**). Make sure its box is ticked.
3. Close the list by pressing `Esc` or clicking **OK**.

**What you should see:** `d365-erp` is in the list and ticked, with tools underneath it such as `data_find_entities` and `data_create_entities`.

> Screenshot: tools picker with d365-erp ticked.

---

## Final check: one prompt that proves it all works

Copy this prompt, paste it into Copilot Chat, and press `Enter`:

```
Please check your connection to Dynamics 365 without changing anything.
Using only the d365-erp tools, tell me: (1) whether you can see those tools, and
(2) the name of legal entity USMF, by reading it (read-only). Keep the answer to three lines.
```

VS Code may pause and ask for permission before Copilot uses a tool, with a **Continue** (or **Allow**) button. This is a read-only lookup, so it's safe to click **Continue**.

**It worked if** Copilot replies that it can see the Dynamics 365 tools and gives you a company name for USMF (in the standard practice data that's *Contoso Entertainment System USA*, but yours may differ).

**If it didn't work:** don't try to fix it yourself. Copy the message Copilot or VS Code showed you and share it with your facilitator.

When the final check passes, you're ready. In the Explorer, open `exercise-guides` and then `scenario-card.md`.
