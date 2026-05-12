<p align="center">
  <img src="docs/logo.svg" alt="OpenBioMedClaw" width="320">
</p>

<p align="center">
  <strong>OpenBioMedClaw</strong> — 生物医药领域的 AI Agent
</p>

<p align="center">
  <em>A biomedical AI agent · Fork of <a href="https://github.com/anomalyco/opencode">OpenCode</a> · MIT License</em>
</p>

<p align="center">
  <a href="README.md">English</a> |
  <a href="README.zh.md">简体中文</a>
</p>

---

## Status

**Early development.** Current milestone: Web Demo (target: 2 weeks).

## What is this

OpenBioMedClaw is a task-oriented AI agent for biomedical research and drug discovery.
It is built as a fork of [OpenCode](https://github.com/anomalyco/opencode), extended with
domain-specific tools (PubMed, RDKit, ClinicalTrials.gov, etc.) and a biomedical expert prompt,
designed to assist researchers, computational chemists, bioinformaticians, and project managers
working at pharma, biotech, CRO, and academic medical institutions.

## Quickstart (development)

```bash
git clone git@github.com:youngking0727/opencode_dev.git
cd opencode_dev
bun install

# Start the web server
bun run --cwd packages/opencode --conditions=browser src/index.ts web --port 4196

# Open http://localhost:4196 in your browser
```

## Agent modes

Inherited from OpenCode. Switch with the `Tab` key:

- **build** — Full-access agent for development work
- **plan** — Read-only agent for analysis and code exploration; denies file edits by default

## Documentation

- [Fork maintenance guide](./MAINTAINING.md)
- More documentation will be added as the project matures.

## Acknowledgments

Built on top of [anomalyco/opencode](https://github.com/anomalyco/opencode) (MIT License).
Sincere thanks to the OpenCode team for their open-source work.

## License

MIT — see [LICENSE](./LICENSE)
