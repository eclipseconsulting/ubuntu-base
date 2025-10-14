# Ubuntu Base Images

[![Build & Push](https://github.com/eclipseconsulting/ubuntu-base/actions/workflows/build.yml/badge.svg)](https://github.com/eclipseconsulting/ubuntu-base/actions/workflows/build.yml)

Lean Ubuntu LTS base images for runtime containers, published to GitHub Container Registry.

## Available Images

- `ghcr.io/eclipseconsulting/ubuntu-base:22.04` - Ubuntu 22.04 LTS
- `ghcr.io/eclipseconsulting/ubuntu-base:24.04` - Ubuntu 24.04 LTS

**Architectures:** `linux/amd64`, `linux/arm64`

## Features

- Weekly automated security updates
- Pre-configured locale (en_US.UTF-8)
- Build tools (build-essential, git, openssl)
- Oh My Zsh pre-installed
- Unprivileged `appuser` (UID 10001) for secure runtime
- `/apps` working directory (owned by appuser)01) for secure runtime

## Usage

### Basic Usage

```dockerfile
FROM ghcr.io/eclipseconsulting/linux-base:24.04

# Install your runtime (Python, Node, etc.)
# Switch to USER appuser when ready
``` 

## Included Packages

- ca-certificates, curl, wget
- git
- build-essential (gcc, g++, make, etc.)
- openssl
- zsh with Oh My Zsh
- locales

## Build Schedule

Images are automatically rebuilt:
- Weekly on Mondays at 3:17 AM UTC
- On changes to Dockerfiles

## Build the Image 

```shell
# Test Ubuntu 24.04
cd ubuntu-24.04
docker build -t ubuntu-base:24.04-test .
```

```shell
# Test Ubuntu 22.04
cd ubuntu-22.04
docker build -t ubuntu-base:22.04-test .
```

## Testing the Image

### Quick Smoke Test

```
# Run a shell in the container
docker run -it --rm ubuntu-base:24.04-test zsh

# Inside the container, verify:
gcc --version
g++ --version
git --version
whoami                    # Should be root
echo $LANG                # Should be en_US.UTF-8
ls -la /apps              # Should exist and be owned by appuser
```

### Test the appuser

```
# Run as the appuser
docker run -it --rm -u appuser ubuntu-base:24.04-test zsh

# Inside container:
whoami                    # Should be appuser
pwd                       # Should be /apps
touch test.txt            # Should succeed (you own /apps)
```

### Clean Up After Testing

```

# Remove test images
docker rmi ubuntu-base:24.04-test
```

This project was inspired by the book [Talk Python in Production](https://talkpython.fm/books/python-in-production).

