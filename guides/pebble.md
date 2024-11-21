# Implementing Pebble (baby Let's Encrypt)

[Pebble](https://github.com/letsencrypt/pebble) is a testing-only cut down version of [Boulder](https://github.com/letsencrypt/boulder), which is useful for testing ACME clients as it has no rate limits. Pebble is RFC8555 compliant.

> **Note**: Pebble is not suitable for production use due to randomisation of `/directory` endpoints and certificates and private keys.

This guide will install Pebble from source on Debian Linux (AMD64).

## Installation:

1. Install prerequisite packages:

```bash
sudo apt install curl git -y
```

2. Download Go:

```bash
# Download the latest version of Go for AMD64:
curl -L https://go.dev/dl/go1.23.3.linux-amd64.tar.gz --output go1.23.3.linux-amd64.tar.gz
```

3. Extract Go to `/usr/local`:

```bash
sudo tar -C /usr/local -zxvf go1.23.3.linux-amd64.tar.gz
```

4. Add `/usr/local/go/bin` and `~/go/bin` to the PATH environment variable by adding the below to `~/.profile`:

```bash
echo export PATH=$PATH:/usr/local/go/bin:~/go/bin >> ~/.profile
```

5. Install Pebble:

```bash
# Clone the repository and check in to the directory:
git clone https://github.com/letsencrypt/pebble/ && cd pebble
# Install Pebble:
go install ./cmd/pebble
```