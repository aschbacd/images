# Simple Docker Images Collection

This repository contains a collection of minimal, custom Docker images. Each image is built and
published automatically to the GitHub Container Registry (GHCR) when its folder is updated and
changes are merged into the `main` branch.

## Features

- 📁 One subfolder per image inside `images/` (e.g., `images/restic/`, `images/tini/`)
- ⚙️ Automatic builds with GitHub Actions
- 🔖 Semantic version tags based on installed software and base image versions
- 🐋 Published to [GHCR](https://ghcr.io) under `ghcr.io/aschbacd/<image-name>`

## Repository Structure

```
.
├── images/
│   ├── restic/
│   │   └── Dockerfile
│   ├── tini/
│   │   └── Dockerfile
├── .github/
│   └── workflows/
│       └── build.yml
└── README.md
```

Each folder contains:
- A `Dockerfile` using a base image (e.g. `restic/restic:0.18.0`, `alpine:3.21.3`)
- An ARG/ENV variable named `VERSION` defining the installed software version
- (Optional) A `# VERSION=...` comment to help the workflow extract the tag

## Image Tags

Images are published with two tags:

1. **Versioned tag**: Based on the `VERSION` variable and base image
   Example: `0.19.0-alpine-3.21.3`
2. **`latest` tag**: Always updated on changes to the image

### Custom Version Increment

You can specify a custom version increment in the Dockerfile by adding a `VERSION_INCREMENT` variable:

```dockerfile
# VERSION=0.18.0
# VERSION_INCREMENT=1
```

- If no `VERSION_INCREMENT` is specified or it is set to `0`, the tag will be based on the base image version (e.g., `restic:0.18.0`).
- If a `VERSION_INCREMENT` is provided (e.g., `1`), the tag will be `restic:0.18.0-1`.

## Pulling Images

```bash
docker pull ghcr.io/aschbacd/restic:0.18.0
docker pull ghcr.io/aschbacd/tini:0.19.0-alpine-3.21.3
```

## Adding a New Image

1. Create a new folder under `images/` (e.g. `images/htop/`)
2. Add a `Dockerfile` using this pattern:

```Dockerfile
FROM alpine:3.21.3
ARG VERSION=3.2.1
ENV VERSION=$VERSION

RUN apk add --no-cache htop=$VERSION
```

3. Open a PR. Once merged to `main`, the image will be built and published automatically.

## CI/CD

The GitHub Actions workflow:
- Detects changes in folders under `images/`
- Builds Docker images
- Extracts `VERSION` and base image tag
- Tags and pushes images to GHCR

## Contributing

Feel free to open issues or submit pull requests for new images or improvements.

## License

[MIT](./LICENSE)
