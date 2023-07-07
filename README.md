# My Workspace Docker Environment

This repository contains a Docker-based development environment setup for various programming languages and tools.

## Directory Structure
```
.
├── .devcontainer
│   ├── .env.example
│   ├── devcontainer.json
│   ├── docker
│   │   └── workspace
│   │       └── Dockerfile
│   └── docker-compose.yml
├── .githooks
│   └── pre-commit
├── .gitignore
├── .vscode
│   └── settings.json
├── README.md
└── memo
    ├── docker-memo.md
    └── git-memo.md

```

## Prerequisites

- Docker
- Docker Compose
- Visual Studio Code
- [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension for Visual Studio Code

## Setup

1. Clone this repository.
2. Copy `.env.example` to `.env` and modify the environment variables as needed.
3. Open this repository in Visual Studio Code.
4. Press `Ctrl+Shift+P` to open the command palette.
5. Type `Reopen in Container` and press `Enter`. This will reopen the workspace inside the Docker container.

## Services

The Docker environment includes the following services:

- Workspace: A development environment with various programming languages and tools installed.
- PostgreSQL: A PostgreSQL database server.
- PgAdmin: A web-based PostgreSQL database client.
- MySQL: A MySQL database server.
- MongoDB: A MongoDB database server.
- Mongo Express: A web-based MongoDB database client.

## Development Environment

The workspace service includes the following programming languages and tools:

- Java (via SDKMAN)
- Gradle (via SDKMAN)
- Python (via pyenv)
- Node.js (via nvm)
- AWS CDK (via npm)
- Go (via gvm)
- GCC and G++ (latest versions)
- AWS CLI
- GitHub CLI
- AWS SAM CLI

