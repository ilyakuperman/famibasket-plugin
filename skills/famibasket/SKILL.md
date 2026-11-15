---
name: famibasket
description: Helps the user keep their Famibasket family shopping list up to date through the Famibasket tools — turning a recipe, a meal plan or a spoken list into items on the list, adding what the user asks for, ticking off bought items and tidying the list. Use when the user mentions their Famibasket list, shopping list or family basket, or asks to put ingredients or groceries on it.
---

# Famibasket shopping list

Famibasket is a shared shopping list for a family. Everyone in a list sees the same items on their phones within seconds.
The Famibasket tools work only with the signed-in user's own lists and only change them when the user asks.

## Tools at a glance

| Tool | What it does |
| --- | --- |
| `add_items` | Adds one or many items in one call. Duplicates are resolved by the service: an item the family has had before comes back to the list; an item already on the list is not duplicated |
| `get_shopping_list` | Shows what is on a list: items to buy by store department, then recently bought items |
| `mark_bought` | Ticks items as bought, or (`bought: false`) returns them to the to-buy part |
| `update_items` | Changes quantity, stores, comment or the urgent flag of items on the list |
| `remove_items` | Takes items off the list; they stay in the family's saved items and can be added again |
| `list_shopping_lists` | Lists the user's lists with their language and which one is the default |

Without `list_id`, every tool uses the user's default list (chosen when the user connected Famibasket).

## Adding items

1. Add only what the user asked to add. When the user shares a recipe or a meal plan, first show the shopping list you derived and add it after the user agrees, unless the user already asked to add it.
2. Put all items of one request into a single `add_items` call. Reading the list first is not needed — the result already says what was added, what came back from saved items and what was already on the list.
3. Write each item the way a person types it in the app:
   - one product per entry, in its basic (dictionary) form: "Milk", not "some milk" or "milk for the pancakes";
   - in the list's language (returned as `list.language`); if it is not known yet, use the language the user writes in;
   - a count goes in front: "2 Bread", "6 Eggs"; a weight or volume with a unit goes after the name and is stored as the comment: "Flour 500 g".
4. Use the object form when the user gives details: `{"name": "Coffee", "quantity": 2, "stores": ["Lidl"], "comment": "decaf", "urgent": true}`.
   For an item that is already on the list these details replace the current ones, so pass only what the user actually said.
5. From a recipe, merge repeated ingredients, skip staples the user says they have (water, salt, oil — ask if unsure), and round to buyable amounts.
6. After the call, tell the user in one or two sentences what was added, what was already there and anything that failed with its reason.

## Ticking off, changing and removing

- "I bought the bread" → `mark_bought` with the item name or id. "Not bought after all" → `mark_bought` with `bought: false`.
- "We need three, not two" → `update_items` with the new quantity. Pass only the fields the user mentioned.
- "Take the chips off the list" → `remove_items`. This does not delete anything; the item can be added again later.
- When a name is ambiguous, call `get_shopping_list` and use item ids.

## What the tools cannot do

The Famibasket tools cannot delete items or lists, rename items, change an item's department, add photos, or manage list members and invitations.
For those, point the user to the Famibasket app (famibasket.com). Lists other than the default one are available too: name the list or pass its `list_id`
from `list_shopping_lists`.

## Errors

Error messages from the tools are written for the user — pass them on and follow the suggested step (for example, choosing a new default list at
mcp.famibasket.com/connections, or waiting a minute after a rate limit). Do not retry a failed write more than once without asking the user.
