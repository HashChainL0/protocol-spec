# Contributing

Thanks for reading the spec closely. Hash Chain is at the spec stage, so careful criticism is the most useful contribution.

## Ways to contribute

- **Ask a question** about a design point that is unclear. Use the "Spec question" issue template.
- **Propose a change** to a document. Use the "Spec change" issue template first for anything larger than a typo, so the idea can be discussed before text is written.
- **Fix small things** such as typos, broken links and unclear wording directly with a pull request.
- **Report a security problem** privately. Do not open a public issue. See [SECURITY.md](SECURITY.md).

## Pull request process

1. Fork the repository and create a branch.
2. Change one topic per pull request.
3. Explain in the description what changed and why, and link the related issue.
4. If your change alters a trust assumption, a data structure or a connection, update every document that mentions it, including the glossary and FAQ.
5. A maintainer reviews. Substantive changes may need discussion in the issue before merging.

## Spec style guide

- **Plain first, technical second.** Every document opens with an "In short" block of two or three plain sentences.
- **Fixed structure.** In short, body sections, Technical details, Open questions.
- **Honest status.** Mark designs as designs. Never describe a draft as audited, deployed or proven.
- **No invented numbers.** No throughput, fee, price, user or date figures unless they come from a cited, reproducible source. Use symbols such as T0 and T1 for timing parameters.
- **Unspecified means unspecified.** If something is undecided, write "to be specified" and add it to Open questions.
- **Name things consistently.** Use the terms in the [glossary](docs/glossary.md). The project name is written "Hash Chain".
- **Punctuation.** Use commas, colons and periods. Do not use em dashes.
- **Diagrams.** Mermaid for inline diagrams. Quote any label that contains parentheses or punctuation. Static diagrams go in `diagrams/` as SVG.
- **Interfaces.** Pseudocode and TypeScript-style interfaces are marked "draft" and are descriptive, not normative, until the spec says otherwise.
- **Keywords.** Where normative language is needed, use MUST, SHOULD and MAY in capitals with their usual meaning.

## Code of Conduct

By taking part you agree to follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## License

By contributing you agree that your contribution is licensed under CC BY 4.0, the same license as the rest of the repository.
