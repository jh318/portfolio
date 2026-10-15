# Portfolio Website

Personal portfolio for [John Hawley](https://johnhawley.io), showcasing software engineering, IT infrastructure, cloud automation, and game development projects.

**Live site:** [johnhawley.io](https://johnhawley.io)

## Overview

This repository contains the source for my personal portfolio website. The site is hosted on AWS using Amazon S3 and CloudFront, with automated deployments from GitHub Actions.

The current website is intentionally lightweight: a single HTML file with embedded CSS and no JavaScript dependencies. I plan to evolve it as I add projects, experiment with visual design, and publish playable browser games.

## Technology

- **Frontend:** HTML5 and CSS3
- **Storage:** Amazon S3
- **Content delivery:** Amazon CloudFront
- **CI/CD:** GitHub Actions
- **Authentication:** GitHub OpenID Connect (OIDC) with AWS IAM

## Deployment Architecture

```text
Push to main
    |
    v
GitHub Actions
    |  OIDC token
    v
AWS IAM / STS
    |  Temporary credentials
    v
Amazon S3 (johnhawley.io)
    |
    v
Amazon CloudFront
    |
    v
https://johnhawley.io
```

The workflow in [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) runs on pushes to `main` and can also be started manually. It:

1. Checks out the repository.
2. Assumes a restricted AWS IAM role using GitHub OIDC, without storing long-lived AWS access keys in GitHub.
3. Uploads `index.html` to the S3 bucket.
4. Creates a CloudFront cache invalidation so the updated page can be served.

The IAM role is scoped to the portfolio repository and `main` branch. Its deployment permissions are limited to the portfolio S3 bucket and CloudFront distribution.

## Local Development

Clone the repository and open `index.html` in a browser:

```bash
git clone https://github.com/jh318/portfolio.git
cd portfolio
```

No package installation or build step is currently required. You can also use a local development server or VS Code/Codespaces preview while editing.

## Repository Structure

```text
.
├── .github/
│   └── workflows/
│       └── deploy.yml    # AWS deployment workflow
├── index.html            # Website markup and styles
└── README.md
```

## Planned Improvements

- Refine the site's layout, styling, and presentation.
- Expand the featured projects section with technical write-ups.
- Add links to playable browser games and other interactive projects.
- Evolve the deployment workflow as the site grows beyond a single HTML file.
- Explore infrastructure-as-code for managing AWS resources.

These are planned enhancements, not features of the current deployment.

## Project Links

- [Live portfolio](https://johnhawley.io)
- [GitHub profile](https://github.com/jh318)
- [itch.io](https://jh318.itch.io/)
