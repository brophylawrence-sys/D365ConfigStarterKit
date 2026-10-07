# Glossary

Every term used in this folder, in plain English. Nothing here needs to be memorized. Come back whenever a word trips you up.

## The tools on your computer

- **Visual Studio Code (VS Code):** a free program from Microsoft for working with files. We use it because it has an AI assistant built in.
- **Folder / workspace:** when you "open a folder" in VS Code, that folder becomes your workspace: the set of files VS Code shows on the left and that the AI assistant is allowed to read. This whole course lives in one folder called `first-dynamics-action`.
- **Markdown file (.md):** a plain text file with light formatting, like the one you're reading. VS Code can show it nicely formatted (press `Ctrl+Shift+V`, or `Cmd+Shift+V` on a Mac).
- **Sign in / authenticate:** proving who you are to a system, usually through a Microsoft or GitHub login window.

## The AI pieces

- **GitHub Copilot:** the AI assistant built into VS Code. It reads what you type and writes back, like a chat.
- **Copilot Chat:** the chat panel in VS Code where you talk to Copilot.
- **Model:** the AI "brain" Copilot uses to understand and answer you. You can choose between a few; your facilitator will tell you which one to pick.
- **Agent mode:** a Copilot Chat mode that lets Copilot use connected tools, instead of only answering in text. It is not the same as choosing a separately named custom agent.
- **Custom agent:** a specialist Copilot setup with its own name, role, and instructions. This course workspace does not include one.
- **Skill:** reusable instructions for completing a kind of task. A skill may be made available to Copilot by the surrounding setup; this course does not require one.
- **Prompt:** the message you type to Copilot.

## The connection to Dynamics 365

- **MCP (Model Context Protocol):** a shared standard that lets an AI assistant connect to other systems in a safe, predictable way. Think of it as a universal plug.
- **MCP server:** the thing on the other end of that plug. The Dynamics 365 ERP MCP server offers Copilot a list of things it's allowed to do in Dynamics 365.
- **Tool:** one specific thing an MCP server lets Copilot do, such as "look up records" or "create a record."
- **Tool call:** the moment Copilot actually uses one of those tools. VS Code shows each tool call in the chat and may ask you to approve it.
- **API:** the official "service entrance" a system offers for other programs to talk to it. The MCP server uses Dynamics 365's own APIs, so it follows the same rules and permissions as you do.
- **Environment / sandbox:** one copy of Dynamics 365. A sandbox is a practice copy, separate from the real business system.

## The business terms

- **Dynamics 365:** Microsoft's business software, which companies use to run finance, purchasing, stock, production, and more. In this course we use the Finance and Supply Chain Management part.
- **Legal entity:** one company inside Dynamics 365. A single environment can hold several; every record belongs to one of them. Ours is called `USMF`.
- **Vendor:** a supplier: a company you buy things from.
- **Item:** a product the company buys, stores, or sells. Each has an item number.
- **Purchase order (PO):** an official request to a vendor to deliver certain items, in a certain quantity, at an agreed price.
- **Site and warehouse:** where the delivered items should go: the site is the location, the warehouse is the specific storage place there.
- **Preview:** in this course, the summary Copilot shows you *before* it creates anything, so you can check it.
