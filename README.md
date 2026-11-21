<p align="center">
  <a href="https://i2v.ai">
    <img src="https://i2v.ai/brand/folded-rabbit-light.png" alt="i2v.ai" width="96">
  </a>
</p>

<h1 align="center">Generate media with i2v.ai</h1>

<p align="center">
  A composite GitHub Action that quotes a media request, refuses to exceed your USD ceiling, submits it, waits for the result and downloads the file.
</p>

<p align="center">
  <a href="https://i2v.ai/cli"><img alt="Docs" src="https://img.shields.io/badge/docs-i2v.ai%2Fcli-6366f1?style=flat-square"></a>
  <a href="https://www.npmjs.com/package/@i2v-ai/sdk"><img alt="SDK" src="https://img.shields.io/badge/SDK-%40i2v--ai%2Fsdk-0ea5e9?style=flat-square"></a>
  <a href="./LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MIT-22c55e?style=flat-square"></a>
</p>

<br>

## Usage

```yaml
name: Product image
on: workflow_dispatch
jobs:
  image:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - id: generate
        uses: i2v-ai/generate-action@main
        with:
          api-key: ${{ secrets.I2V_API_KEY }}
          model: gpt-image-2
          input: '{"prompt":"A ceramic mug on an oak table, soft daylight"}'
          max-cost-usd: "0.20"
          output-file: out/mug
      - uses: actions/upload-artifact@v4
        with:
          name: mug
          path: ${{ steps.generate.outputs.output-path }}
```

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| `api-key` | yes | i2v.ai API key, usually `${{ secrets.I2V_API_KEY }}`. |
| `model` | yes | Model ID such as `gpt-image-2` or `seedance-2-0`. |
| `input` | yes | Model inputs as a JSON object string. |
| `max-cost-usd` | yes | Approved ceiling. The action stops before submitting when the quote is higher. |
| `output-file` | no | Download path, default `i2v-output`. The media extension is added when missing. |
| `idempotency-key` | no | Stable key, default `gh-<run>-<attempt>`. Reuse it to recover instead of re-submitting. |
| `wait-seconds` | no | Polling deadline, default `900`. |

## Outputs

| Output | Description |
| --- | --- |
| `task-id` | Accepted task ID. |
| `output-path` | Absolute path of the downloaded media file. |
| `quote-amount` / `quote-currency` | Quoted maximum reservation. |

The action also writes `<output-path>.task.json` with the quote, receipt and final task for auditing. It never logs the API key.

## Why the ceiling matters

Media generation is a paid action with a variable price. The action quotes first, compares the quote with `max-cost-usd`, and only then submits. If the quote is higher, the workflow fails without creating a task.

## For AI agents

- Agent index: <https://i2v.ai/llms.txt>
- API documentation: <https://i2v.ai/llms-full.txt>

## Related repositories

| Repository | Purpose |
| --- | --- |
| [i2v-cli](https://github.com/i2v-ai/i2v-cli) | Command-line tools |
| [i2v-sdk-js](https://github.com/i2v-ai/i2v-sdk-js) | TypeScript SDK |
| [i2v-sdk-python](https://github.com/i2v-ai/i2v-sdk-python) | Python SDK |
| [i2v-skills](https://github.com/i2v-ai/i2v-skills) | Agent Skills |
| [i2v-mcp](https://github.com/i2v-ai/i2v-mcp) | Remote MCP metadata |
| [i2v-api-examples](https://github.com/i2v-ai/i2v-api-examples) | Runnable examples |
| [i2v-agent-tools](https://github.com/i2v-ai/i2v-agent-tools) | Ecosystem hub |

## License

MIT. See [LICENSE](./LICENSE).
