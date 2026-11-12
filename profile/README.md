<a href="https://comchatx.com">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/comchatx/.github/main/profile/banner-dark.png">
    <img alt="Hand your Amazon operations to cx" src="https://raw.githubusercontent.com/comchatx/.github/main/profile/banner-light.png" width="846">
  </picture>
</a>

ComChatX is where Amazon sellers put cx to work. cx looks up the data, makes the plan and carries it out within what you allow.

**It does the work.** cx changes your listings, prices and ads through the plugins you install, then tells you what it did. Ask, and it gives its reasons.

**It gets its own data.** Other AI tools ask you to find the sources and wire up the APIs. Here there is one balance: cx buys what it needs from the data sources you install, reads your store once you connect it, and every charge shows in Usage.

**It is pi, and everything else is a plugin.** cx is [pi](https://pi.dev) with nothing added and nothing taken away, on a cloud computer used by your cx alone. Each plugin does one job and can build on another, so they fit together like parts.

Sellers start at [comchatx.com](https://comchatx.com). Developers start with [How to write a package for ComChatX](https://github.com/comchatx/.github/blob/main/WRITING-A-PACKAGE.md) and [Contributing](https://github.com/comchatx/.github/blob/main/CONTRIBUTING.md).

## How a plugin reaches a seller

A plugin is a pi package published on npm. When a seller presses Install on the Plugins page, pi's own `pi install` puts the package on cx's computer, with the plugins it needs, and cx can use it from the seller's next message. The Plugins page lists ComChatX's own plugins, and the packages that npm's search finds by the keyword `pi-package`.

## Where the plugins are

ComChatX's own plugins are in the repositories listed under this page, each with a line that says what it does, and each under the MIT licence. A seller finds them on the Plugins page in the product.
