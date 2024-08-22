# Hockeypuck OpenPGP Key Server

## Inroduction:

This guide is for installation and basic configuration of the Hockeypuck OpenPGP Key Server. Hockeypuck is an OpenPGP key server that supports the SKS protocol for syncronization using a PostgreSQL backend database.

## Installation:

We will cover only the recommended installation which is using Docker via the Git [repository](https://github.com/hockeypuck/hockeypuck).

1. Clone the repository:

```bash
# Ensure core.autocrlf is false.
git clone --config core.autocrlf=false https://github.com/hockeypuck/hockeypuck
```

2. From the `/contrib/docker-compose/devel` directory build the containers:

```bash
docker-compose build
```

3. Start the containers:

```bash
docker-compose up -d
```