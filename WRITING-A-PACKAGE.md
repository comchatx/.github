# How to write a package for ComChatX

A ComChatX plugin is an unchanged pi package plus, where it needs one, a half outside the box: the gate's half for a data source, the proxy's half for a model, the page's half for what the Plugins page draws. The half inside the box is never changed. A model plugin has only the outside half.

Most plugins are only the half inside the box, and you can write one today with nothing but pi. This page covers that half.

## What a package is

A package is a pi package, exactly as pi's [packages documentation](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/packages.md) describes it: an npm package with a `pi` key in its `package.json` naming the extensions or skills it brings, and the `pi-package` keyword.

```json
{
  "name": "cx-example",
  "version": "0.1.0",
  "license": "MIT",
  "keywords": ["pi-package"],
  "pi": { "extensions": ["./dist/extensions"] },
  "peerDependencies": { "@earendil-works/pi-coding-agent": "*" }
}
```

A ComChatX box runs pi unchanged, so a package that works in your own pi works in a box. Test it there first:

```bash
pi install -l ./path/to/your/package
```

What a box adds around pi:

- **The seller's project.** The session's working directory is the account's own tree, and `.pi/settings.json` in it lists the packages that the seller installed. Pi's own install and remove write that list; a package never edits it.
- **The network.** A box reaches the outside only through its loopback proxy, which holds the keys. A package holds no key and asks for none; a call that needs one goes to a route that the box's proxy serves. `cx-jev-tidy` is the worked example. Outside a box nothing listens at that address, so a package of that kind must do nothing harmful when its call fails.
- **The model.** The model is whichever one the seller chose. Write for pi's interfaces, not for one model.

## What a plugin is, and is not

1. **One job, not a set of features.** A plugin does one job for a seller, one you can tell a seller in a sentence, such as "tracks the prices of your products and your competitors'". Code that does no job a seller sees on its own is a part for other plugins: it does not go on the Plugins page by itself, and is bundled into the plugin that uses it. A bigger job is several small plugins put together: the board is a base, and its to-dos with their option cards are a plugin a seller can install alone.
2. **A data source connects; it does not interpret.** A data source plugin hands cx everything its supplier can give, as the supplier gives it: nothing renamed, converted or left out. Its skill says how to read it. How to analyse it and what to do about it belong to the plugins built on it: a listing playbook, an ads playbook, and later ones for stock or money.
3. **Tools, not a workflow.** Give the most basic abilities, and let the seller and cx combine them; do not fix for the seller when a report goes out or what shape it takes. A ready path goes in as one of the plugin's Try it examples on the Plugins page. Do not build a tool for what the model can do by writing a few lines of code itself, such as counting the characters in a title.
4. **Skills state facts, not rules.** Write what a tool does, how the supplier's API answers and where it trips people up. Do not write when to ask the seller, which step comes first, or how long a report is kept: the model judges those itself.
5. **All of the grant, never less.** A plugin uses as much as the seller's grant allows: what can be read can be read, and what can be changed can be changed. Neither the page nor the gate adds a switch to narrow it; the risk is the seller's own.
6. **For every seller, not for one.** One seller's way of working is a reference: write it as a fact about that seller, never as a rule.
7. **Borrow first, then write.** First look for what pi's ecosystem and the supplier already give: pi packages, the supplier's MCP server and command line. If one falls short, wrap it in a cx plugin that adds what is missing; if a wrapper would add nothing, do not wrap it, and use it as it is. Write your own only when nothing can be wrapped.
8. **Name, don't copy.** When a plugin needs another, name it in your package, and the two install together; one it works without is marked optional, for the seller to add if they want. Code several plugins share is one package each of them depends on, never a copy. How to name one is under [Dependencies](#dependencies).
9. **A plugin owns everything of its own; the platform knows no plugin.** A plugin's words, its settings fields and its Try it examples live in its own package, and ComChatX draws them as the package says. A change to a plugin is a new version of that plugin only; the platform does not ship with it. The platform draws each kind of thing one way, for every plugin.

How a plugin words its card line and the first screen of its Overview is ComChatX's copy page, `.claude/skills/ui-words/plugin-words.md` in the product's repository.

## Dependencies

Both kinds of dependency are declared, and neither is copied.

- **A library** is a `dependencies` entry in your `package.json`, or bundled into your build. Pi runs `npm install` when it installs a package from npm.
- **The packages pi supplies** (`@earendil-works/pi-ai`, `@earendil-works/pi-agent-core`, `@earendil-works/pi-coding-agent`, `@earendil-works/pi-tui`, `typebox`) are `peerDependencies` with the range `"*"`, and are never bundled.
- **Another plugin.** A plugin that needs another names it, and the two install together; its code is never copied. A plugin among your `dependencies` is named there: every data source depends on `cx-sources`, and data sources are written by ComChatX for now.
- **A plugin your plugin needs beside it** goes in the `comchatx` field of your `package.json`, by its npm name, when it is no `dependencies` entry: `@comchatx/cx-schedule` declares `"comchatx": { "builds": ["@amaster.ai/pi-task-scheduler"] }`. The Plugins page installs it with your plugin. One your plugin works without is marked optional, as `cx-listing-playbook` names Keepa: `{ "on": "cx-keepa", "need": "optional" }`. The page lists it under the plugins yours uses, for the seller to install.

## Extensions and skills

An **extension** is TypeScript that pi loads and that hooks pi's events: it adds a section to the prompt, registers a tool, or acts at a turn's boundary. `cx-amazon-seller` is the smallest example, at one handler and one sentence. A **skill** is a `SKILL.md` that tells the model what a tool does and how the supplier answers; pi reads its body when the model judges it relevant.

Today a ComChatX package carries extensions or skills, one kind per package. Pi itself allows both kinds in one package, but the Plugins page does not promise that pair yet.

## Shipping it

- Use one repository per package, with the MIT licence. Name third-party code that you ported in a `THIRD_PARTY_NOTICES.md`, and ship that file with the package.
- Build to a directory that the `pi` key names, and list it in `files`. Keep tests out of any directory that pi loads, since pi loads every module in an extensions directory.
- Write a README that says what the package does, what it needs (a box, a key, nothing), and how to install it with `pi install npm:<name>`. On the Plugins page the README is the plugin's Overview, and npm's `description` its card line.
- What the Plugins page reads from a package today: npm's `description`, the README, the skills and prompt templates it ships (the templates are its Try it examples), the plugins among its dependencies, and from its `comchatx` field its name, mark, shelf (`section`) and place (`order`), the Chinese of its lines, `builds`, and a data source's `source`. A data source's settings and connection form is not read from its package yet: ComChatX draws it by the source's name.
- Publish to npm. The packages in this organization publish from a tag through npm trusted publishing, with provenance.

A package appears on the Plugins page when its npm entry carries the keyword `comchatx` and its `package.json` the `comchatx` field: the page reads where it stands, its Chinese line and its mark from that field, and marks it as a third party's. With the keyword and no field it stands under Extensions in npm's words. To ask a question or propose a change, open an issue in this repository.
