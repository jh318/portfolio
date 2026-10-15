# Portfolio Website

Personal portfolio for [John Hawley](https://johnhawley.io), showcasing software engineering, IT infrastructure, cloud automation, and game development projects.

**Live site:** [johnhawley.io](https://johnhawley.io)

## Overview

This repository contains the source code for my personal portfolio website.

The site is built with HTML, CSS, and JavaScript and hosted on AWS using Amazon S3 and CloudFront. GitHub Actions automatically deploys updates when changes are merged into `main`.

The portfolio highlights my professional experience, technical projects, and game development work. It also serves as a place to experiment with web development, cloud infrastructure, deployment automation, and AI-assisted development.

## Technology

- **Frontend:** HTML5, CSS3, and vanilla JavaScript
- **Hosting:** Amazon S3
- **Content delivery:** Amazon CloudFront
- **CI/CD:** GitHub Actions
- **AWS authentication:** GitHub OpenID Connect (OIDC) with AWS IAM
- **Development environment:** GitHub Codespaces and VS Code
- **AI-assisted development:** OpenAI Codex

The website is intentionally lightweight and does not require a frontend framework or build system.

## Deployment Architecture

```text
Push / Merge to main
        |
        v
   GitHub Actions
        |
        | GitHub OIDC token
        v
     AWS IAM / STS
        |
        | Temporary AWS credentials
        v
    Amazon S3
   (johnhawley.io)
        |
        v
  Amazon CloudFront
        |
        v
 https://johnhawley.io
```

The workflow in [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) runs on pushes to `main` and supports manual execution.

It performs the following steps:

1. Checks out the repository.
2. Assumes a restricted AWS IAM role through GitHub OIDC, avoiding long-lived AWS access keys.
3. Synchronizes the contents of `site/` to the S3 bucket.
4. Creates a CloudFront cache invalidation so visitors receive updated content.

The IAM role is scoped to this repository and its `main` branch, with deployment permissions limited to the portfolio's AWS resources.

The S3 synchronization intentionally does not use `--delete`, allowing separately deployed project content to coexist in the bucket as the portfolio grows.

## Local Development

Clone the repository:

```bash
git clone https://github.com/jh318/portfolio.git
cd portfolio
```

Start a local development server:

```bash
python3 -m http.server 8000 --directory site
```

Then open:

`http://localhost:8000`

No dependency installation or build step is required.

Development can also be performed using GitHub Codespaces and its forwarded-port preview.

## Repository Structure

```text
.
├── .github/
│   └── workflows/
│       └── deploy.yml
├── site/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   ├── js/
│   ├── images/
│   └── games/
└── README.md
```

- `site/` contains the publicly deployed website.
- `css/` contains the website's styles.
- `js/` is reserved for client-side functionality.
- `images/` contains website and project assets.
- `games/` provides a location for playable browser game content.

## Development Workflow

Changes are generally developed on feature branches, reviewed through GitHub pull requests, and merged into `main`.

Merging triggers the automated AWS deployment workflow.

I also use AI-assisted development tools, including OpenAI Codex, to help prototype, implement, troubleshoot, and refine projects. I remain responsible for the design decisions, reviewing changes, testing functionality, and maintaining the resulting code.

## Planned Improvements

- Expand project pages with screenshots, technical details, and development notes.
- Publish playable browser games.
- Explore independently deploying games and other projects alongside the main portfolio.
- Introduce Terraform for infrastructure-as-code.
- Continue improving accessibility, responsiveness, and presentation.

These are planned enhancements rather than features of the current deployment.

## Project Links

- [Live portfolio](https://johnhawley.io)
- [GitHub profile](https://github.com/jh318)
- [itch.io](https://jh318.itch.io/)