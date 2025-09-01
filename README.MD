# Docker for Nuxt + Mongo
--- -

Easy to use, prod and dev ready, docker template for nuxt with mongo integration.

Just create your project in `/nuxt-app` directory or update [compose override](compose.override.yaml) to existing, and you are good to go 😉.

Tested on Windows 10 / 11 (WSL2)

## Step-by-step tutorial
--- -

### Create Nuxt App (select override content)
```shell
  pnpm create nuxt@latest nuxt-app
```

### Build web container:
```shell
  docker compose build --pull --no-cache
```
### Run containers:
```shell
  docker compose up --wait
```

### Build to production:
```shell
  docker compose -f compose.yaml -f compose.prod.yaml build --no-cache
```
### Deploy to production:
```shell
  docker compose -f compose.yaml -f compose.prod.yaml up --wait
```
