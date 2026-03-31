# qemu-builder

Prebuilt QEMU binaries for Ubuntu 24.04, distributed as a Docker image for `amd64` and `arm64`.

The image compiles `qemu-system-aarch64` from source against the Ubuntu 24.04 system libraries, so the resulting binary is native to the host platform and doesn't require any additional runtime dependencies beyond what's already in the image.

## Usage

Pull the image from the GitHub Container Registry:

```bash
docker pull ghcr.io/dymk/qemu-builder:latest
```

Run `qemu-system-aarch64` directly:

```bash
docker run --rm ghcr.io/dymk/qemu-builder:latest qemu-system-aarch64 --version
```

Or copy the binary out of the image into your own build:

```dockerfile
COPY --from=ghcr.io/dymk/qemu-builder:latest /usr/local/bin/qemu-system-aarch64 /usr/local/bin/
```

## Build

The image is built automatically via GitHub Actions on every push to `main` and published to `ghcr.io/dymk/qemu-builder`. Builds target both `linux/amd64` and `linux/arm64`.

To build locally:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t qemu-builder .
```

## Configuration

The QEMU source and version can be overridden at build time via build args:

| Arg           | Default                                      | Description              |
|---------------|----------------------------------------------|--------------------------|
| `QEMU_URL`    | `https://gitlab.com/qemu-project/qemu.git`   | Git repository to clone  |
| `QEMU_BRANCH` | `v10.0.0`                                    | Branch or tag to build   |

