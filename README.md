# AI Code Review Bot

An automated GitHub bot that reviews Pull Requests using GPT-4, providing inline code feedback, error detection, and optimization suggestions directly within CI/CD pipelines.

## Features

- **Automated PR Review**: Automatically reviews pull requests when they are opened or updated
- **Inline Comments**: Adds specific, actionable review comments directly on the changed code lines
- **PR Summary**: Generates a concise summary of the changes in each pull request
- **Release Notes**: Auto-generates release notes based on PR descriptions and code changes
- **Conversational**: Can respond to developer replies on review comments for follow-up discussions
- **Configurable**: Supports custom prompts, file filters, and review sensitivity settings

## Tech Stack

- **Language**: TypeScript
- **AI**: OpenAI GPT-4 / GPT-3.5 Turbo
- **Platform**: GitHub Actions
- **API**: GitHub REST API + Octokit
- **CI/CD**: GitHub Actions workflow

## How It Works

```
Developer opens PR
       │
       ▼
GitHub Action triggers
       │
       ▼
Bot fetches PR diff + metadata
       │
       ▼
GPT-4 analyzes code changes
       │
       ▼
Bot posts inline review comments
       │
       ▼
Developer can reply → Bot responds
```

## Getting Started

### Prerequisites

- A GitHub repository
- OpenAI API key with GPT-4 access

### Installation

1. Clone this repository:

```bash
git clone https://github.com/Rishikesan05/AI-Code-Review-Bot.git
```

2. Add the following workflow file to your target repository at `.github/workflows/ai-review.yml`:

```yaml
name: AI Code Review
on:
  pull_request:
    types: [opened, synchronize, reopened]
  pull_request_review_comment:
    types: [created]

permissions:
  contents: read
  pull-requests: write

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: Rishikesan05/AI-Code-Review-Bot@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        with:
          debug: false
          review_comment_lgtm: false
```

3. Add your OpenAI API key as a repository secret named `OPENAI_API_KEY`

### Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `debug` | Enable debug logging | `false` |
| `review_comment_lgtm` | Post comments when code looks good | `false` |
| `path_filters` | Glob patterns to include/exclude files | none |
| `system_message` | Custom system prompt for GPT | built-in |

## Project Structure

```
├── src/
│   ├── main.ts          # Entry point - GitHub Action handler
│   ├── bot.ts           # Core review logic
│   ├── review.ts        # PR review orchestration
│   ├── commenter.ts     # GitHub comment management
│   ├── options.ts       # Configuration parsing
│   ├── tokenizer.ts     # Token counting for GPT
│   └── utils.ts         # Utility functions
├── __tests__/
│   └── main.test.ts     # Unit tests
├── action.yml           # GitHub Action definition
├── dist/                # Compiled output
└── package.json
```

## Development

```bash
# Install dependencies
npm install

# Run tests
npm test

# Build
npm run build
```

## License

MIT
