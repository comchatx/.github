# Contributing to ComChatX packages

Thank you for helping. One rule, borrowed from pi:

**You must understand your code.** Whatever wrote it, you, a tool or a model, you can explain every line of a pull request you open, and why it is there. A pull request whose author cannot explain it is closed.

Beyond that:

- **No CLA.** Your contribution is under the repository's MIT licence, as the code around it is.
- **One package per repository.** A change goes to the repository of the package that it changes. A new package starts as an issue in [comchatx/.github](https://github.com/comchatx/.github/issues), and [How to write a package](https://github.com/comchatx/.github/blob/main/WRITING-A-PACKAGE.md) says what a package is here: its grain, how it declares what it depends on, and what a box gives it.
- **A test that fails without your change.** Run `npm ci`, `npm run typecheck` and `npm test` before you open the pull request; CI runs the same.
- **Name what you ported.** Code taken from elsewhere keeps its licence notice in `THIRD_PARTY_NOTICES.md`.
- **No secrets, no seller data.** No key, token, shop name, ASIN or person's details in code, tests or issues. Use made-up data in tests.

Security problems go to the repository's private vulnerability reporting, not to a public issue.
