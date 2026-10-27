# ComChatX

ComChatX is a hosted assistant for people who run a business selling on Amazon. Each account has its own box running one unchanged [pi](https://pi.dev) session, and the seller works in a single continuous chat. Everything beyond pi itself — who the assistant is, the data it can reach, how a long conversation is kept small — is a package that the seller installs or removes on the Plugins page.

## How a package reaches a seller's box

A package is an ordinary pi package published on npm. When a seller clicks Install on the Plugins page, the box runs pi's own `pi install -l npm:<name>` in that account's project, and pi loads the package from the next prompt. Remove runs `pi remove`. The box carries pi and nothing else, so a package published here reaches every seller by that same click.

## Packages

| Package | What it does |
|---|---|
| [cx-assistant](https://github.com/comchatx/cx-assistant) | Tells the model who it is: cx, a seller's assistant on ComChatX. A new account starts with it. |
| [cx-topics](https://github.com/comchatx/cx-topics) | Names the seller's conversations and retrieves the messages they quote. A new account starts with it. |
| [cx-amazon-seller](https://github.com/comchatx/cx-amazon-seller) | Tells the model that the person it talks with runs a business selling on Amazon. |
| [cx-jev-tidy](https://github.com/comchatx/cx-jev-tidy) | When pi's context fills, cuts stale tool output with Jev before pi summarises, keeping every word. |
| [cx-listing-playbook](https://github.com/comchatx/cx-listing-playbook) | The work on an Amazon listing: how it performs and why, a log of each change, and how to judge one. |
| [cx-subagents](https://github.com/comchatx/cx-subagents) | Lets cx hand a long task to a helper in the background and keep answering; the result comes back by itself. |
| [cx-sources](https://github.com/comchatx/cx-sources) | How cx uses a connected data source and what a call costs; every data source needs it. |
| [cx-keepa](https://github.com/comchatx/cx-keepa) | Amazon price and BSR history from Keepa. |
| [cx-amazon-sp-api](https://github.com/comchatx/cx-amazon-sp-api) | Reads and updates the seller's own Amazon store through Amazon's Selling Partner API. |

The last three are the half of a data source inside the box: skills only. Their other half, which holds the keys and the prices, is ComChatX's and runs outside the box.

Every repository here holds one package under the MIT licence.

## Write one

[How to write a package for ComChatX](https://github.com/comchatx/.github/blob/main/WRITING-A-PACKAGE.md) · [Contributing](https://github.com/comchatx/.github/blob/main/CONTRIBUTING.md)
