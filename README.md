# Famibasket

Keep your [Famibasket](https://famibasket.com) family shopping list up to date from a conversation. Ask the assistant to add groceries —
one item, a whole list, or everything a recipe or a weekly meal plan needs — and they appear on the phones of everyone in your list within
seconds. You can also see what is on the list, tick items off as bought, change quantities, stores and comments, and take items off the list.

## What is inside

- **Connector** (`.mcp.json`): the remote Famibasket server at `https://mcp.famibasket.com/mcp` (Streamable HTTP, OAuth sign-in).
- **Skill** `famibasket` (`skills/famibasket/SKILL.md`): how to turn a request, a recipe or a meal plan into list items, write item names
  the way the app expects, and use each tool.

The plugin contains no local code, scripts or hooks. It runs nothing on your computer.

## Tools

| Tool | Reads or changes | What it does |
| --- | --- | --- |
| `add_items` | changes | Adds one or many items; items the family had before come back instead of being duplicated; for an item already on the list the amounts are added (1 kg + 200 g = 1 kg 200 g) |
| `get_shopping_list` | reads | Items to buy by store department, then recently bought items |
| `mark_bought` | changes | Ticks items as bought or back to buy |
| `update_items` | changes | Sets quantity, stores, comment or the urgent flag to an exact value |
| `remove_items` | changes | Takes items off the list (they stay in the family's saved items) |
| `list_shopping_lists` | reads | Your lists, their language and which one is the default |

The tools cannot delete items or lists, rename items, add photos or manage list members.

## Setup

1. You need a Famibasket account (sign up in the app at [famibasket.com](https://famibasket.com) with Google, Apple or email) and at least one list.
2. Install the plugin and connect Famibasket when asked. A Famibasket page opens: sign in the same way as in the app, choose which list
   the assistant should add to by default, and select **Allow**.
3. Try: "Add milk, 2 bread and eggs to my Famibasket list." Another list can be named in the same request: "Add charcoal to the Country house list."

Full guide with examples: <https://famibasket.com/assistants/>.

## Data and privacy

The plugin sends data only to the Famibasket server (`mcp.famibasket.com`), and only when a tool is used. The server reads and changes your
Famibasket lists on your behalf, with the same access rules as the app: you see only lists you are a member of. It stores an encrypted sign-in
token and hashed access tokens until you disconnect. It does not read your conversations, memory or files. Privacy policy:
<https://famibasket.com/privacy.html>. To disconnect, remove the connector in your assistant or open <https://mcp.famibasket.com/connections>.

## Support

support@famibasket.com

## License

MIT — see `LICENSE`.
