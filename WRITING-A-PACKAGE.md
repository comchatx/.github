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

## The grain

A plugin is the smallest package that does one thing for a seller on its own. If you could explain to a seller in one sentence what installing it changes, it is a plugin. Code that does nothing on its own is a library: bundle it into the packages that use it with npm, as an ordinary dependency. A library never appears on the Plugins page.

## Dependencies

Every kind of dependency is declared. Only a plugin carried inside yours is copied, and then by npm alone.

- **A library** is a `dependencies` entry in your `package.json`, or bundled into your build. Pi runs `npm install` when it installs a package from npm.
- **The packages pi supplies** (`@earendil-works/pi-ai`, `@earendil-works/pi-agent-core`, `@earendil-works/pi-coding-agent`, `@earendil-works/pi-tui`, `typebox`) are `peerDependencies` with the range `"*"`, and are never bundled.
- **Another plugin.** A plugin that needs another names it, and the two install together. A package of ComChatX names it in `builds` in its `comchatx` field (below), and the Plugins page installs the named package first, at npm's latest: a data source names `cx-sources`, the package that every data source needs, and `cx-feishu` names `@amaster.ai/pi-channels`. Data sources are written by ComChatX for now. If your plugin needs another one, open an issue rather than copying its code.
- **A plugin carried inside yours.** Where the two must load as one, as `@comchatx/cx-schedule` loads `@amaster.ai/pi-task-scheduler` with its task directory set, yours depends on it at one exact version, lists it in `bundleDependencies` so it ships in your tarball, as pi's packages documentation asks of a pi package used as a dependency, and loads it through its own extension entry. `builds` names it with `"carried": true`: the Plugins page draws it among the plugins yours uses and installs nothing more, since installed beside yours it would load twice. It moves only with a release of yours.

## Extensions and skills

An **extension** is TypeScript that pi loads and that hooks pi's events: it adds a section to the prompt, registers a tool, or acts at a turn's boundary. `cx-amazon-seller` is the smallest example, at one handler and one sentence. A **skill** is a `SKILL.md` that teaches the model when and how to do something; pi reads its body when the model judges it relevant.

A package may carry both, as `@comchatx/cx-schedule` does: one extension and one skill.

## Shipping it

- Use one repository per package, with the MIT licence. Name third-party code that you ported in a `THIRD_PARTY_NOTICES.md`, and ship that file with the package.
- Build to a directory that the `pi` key names, and list it in `files`. Keep tests out of any directory that pi loads, since pi loads every module in an extensions directory.
- Write a README that says what the package does, what it needs (a box, a key, nothing), and how to install it with `pi install npm:<name>`. npm shows it, and so does the Plugins page: for a package of this organization, the README is its description, so it speaks to a seller, and the steps for whoever works on the package go in a `DEVELOPING.md`, which is not shipped.
- Give `package.json` a `description`, the package's one line in English, and the `author`.
- Publish to npm. The packages in this organization publish from a tag through npm trusted publishing, with provenance.

Every pi package on npm is on the Plugins page: Discover lists what npm's search finds for the `pi-package` keyword under Third-party plugins, and a seller installs it with one click. A package this organization publishes is drawn as ComChatX's own, from its npm record and its `comchatx` field. A seller installs it at npm's latest, and the box never moves it to a newer version on its own: the seller does, with Update on the Plugins page. To propose one, open an issue in this repository.

## The `comchatx` field

What npm does not say of a package of ours, the page reads from the `comchatx` key of its `package.json`; where the page cannot read it, the whole field is set aside and the package is drawn from npm's words alone.

```json
"comchatx": {
  "about": "让 cx 在你设定的时间替你做事。",
  "description": "The README's words in Chinese.",
  "mark": { "line": "clock" },
  "section": "extensions",
  "order": 30,
  "builds": [{ "on": "@amaster.ai/pi-task-scheduler", "carried": true }]
}
```

- `about` is the one line in Chinese, beside npm's `description` in English.
- `description` is the README in Chinese; the page draws the README only where this is given.
- `mark` is the card's drawing, `section` the shelf (`extensions` or `data`), and `name` the plugin's name where it is not the package's.
- `order` is where it stands among ours on Discover, lowest first, in steps of ten so a new package takes a place between two without the others being published again; one without it comes after them.
- `builds` names what installs with it: an npm name or a plugin of the page's own (`jev`), or `{ "on": <name>, "why": { "en", "zh" } }` to say why, with `"carried": true` for a plugin your tarball carries (above).
- A data source adds `source`, and a package with skills may give each skill's words under `skills`; `cx-keepa` is the worked example.
