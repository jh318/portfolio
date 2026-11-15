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
│   ├── games.html
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

## Games gallery

Open `http://localhost:8000/games.html` during local development. Portfolio and Games share `css/style.css` and use ordinary HTML navigation; no JavaScript router or build step is needed.

To add a game, copy an `<article class="project-card game-card">` in `site/games.html`. Give the heading a unique ID and update `aria-labelledby`, the description, tags, status, and Play link. Keep unavailable games as disabled buttons with an explanatory note rather than linking to a missing build. Pizza Break's engine is not yet listed because it has not been specified.

The gallery deliberately uses labeled placeholders rather than invented gameplay images. When a real screenshot is available, put it in `site/images/` and replace the contents of its `.game-media` element with an image, for example:

```html
<div class="game-media">
  <img src="images/railstation.webp"
       alt="Describe the actual gameplay visible in the screenshot"
       width="1280" height="720" loading="lazy" />
</div>
```

Use the actual image dimensions and meaningful alt text. The gallery reserves a 16:9 area and crops images to fit.

### Routing and browser game deployment

The gallery is served at `/games.html`, an explicit S3 object path. The existing workflow already uploads it and the shared CSS; no deployment changes are required. This repository does not define the CloudFront origin or routing configuration, so the implementation avoids assuming directory-index support. CloudFront's default root object does **not** automatically resolve subdirectories such as `/games/` to their `index.html`. S3 website origins can behave differently; see [AWS default root object documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DefaultRootObject.html).

For Pizza Break, place a future browser export and its assets under `site/games/pizza-break/`, then replace the disabled button with a link to `games/pizza-break/index.html`. Use relative asset URLs within the export so it works below that prefix. Do not link there until a build exists. Other games can each have their own subdirectory. Clean URLs such as `/games/pizza-break/` would require confirming website-origin index handling or adding a CloudFront rewrite separately.

Builds in `site/` will be uploaded by the current workflow. Separately uploaded builds can also coexist because sync omits `--delete`; reserve distinct prefixes to avoid overwriting them. Removed or renamed files remain in S3 until deliberately cleaned up. The current `/*` invalidation covers both portfolio and game updates made by this workflow; independent game deployments need their own cache strategy.

Before publishing a browser export, check its engine's hosting requirements and asset response headers (especially WebAssembly MIME types and any precompressed files). Threaded WebAssembly builds may need cross-origin isolation headers; that would be a separate CloudFront configuration decision, not part of this design experiment. A normal static export should be tested at its final nested URL before enabling Play.

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
