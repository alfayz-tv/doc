# Example config
## Example Multi Apps
```json
// touch ecosystem.config.js
module.exports = {
  apps : [
    {
      name: "registrar-api",
      cwd: '/var/www/html/registrar-api',
      script: "./main",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        APP_PORT: 3002
      }
    },
    {
      name: "registrar-admin",
      cwd: '/var/www/html/register-frontend-next/apps/admin',
      script: "node_modules/.bin/next",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        PORT: 3001
      }
    },
    {
      name: "registrar-user",
      cwd: '/var/www/html/register-frontend-next/apps/user',
      script: "node_modules/.bin/next",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        PORT: 3000
      }
    },
    {
      name: "ui-asynq",
      cwd: '/var/www/html/ui-asynq',
      script: "./main",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        PORT: 9090
      }
    },

  ]
}
```

## Example golang
```json
{
    name: "registrar-api",
    cwd: '/var/www/html/registrar-api',
    script: "./main",
    args: "start",
    exec_interpreter: "none",
    exec_mode: "fork_mode",
    log_date_format: "YYYY-MM-DD HH:mm:ss Z",
    env: {
    APP_PORT: 3002
    }
},

```

## Example turborepo
```json
{
    name: "registrar-admin",
    cwd: '/var/www/html/register-frontend-next/apps/admin',
    script: "node_modules/.bin/next",
    args: "start",
    exec_interpreter: "none",
    exec_mode: "fork_mode",
    log_date_format: "YYYY-MM-DD HH:mm:ss Z",
    env: {
    PORT: 3001
    }
},

```

## Example next-js
```json
{
    name: "bsa-frontend",
    cwd: '/home/rofai/bsa-next',
    script: "node_modules/.bin/next",
    args: "start",
    exec_interpreter: "none",
    exec_mode: "fork_mode",
    log_date_format: "YYYY-MM-DD HH:mm:ss Z",
    env: {
    PORT: 3000
    }
},

```

## Example for cluster mode
```json
{
    name: "bsa-frontend",
    cwd: '/home/rofai/bsa-next',
    script: "node_modules/.bin/next",
    args: "start",
    exec_interpreter: "none",
    instances: 2,
    exec_mode: "cluster_mode",
    log_date_format: "YYYY-MM-DD HH:mm:ss Z",
    env: {
    PORT: 3000
    }
},
```

## Example for expres.js
```json
{
    name: "backend-service",
    cwd: '/home/rofai/idchain-blockchain-core/backend-service',
    script: "node_modules/.bin/ts-node",
    args: "src/index.ts",
    exec_interpreter: "none",
    exec_mode: "fork_mode",
    log_date_format: "YYYY-MM-DD HH:mm:ss Z",
    env: {
        "NODE_ENV": "production",
        PORT: 8000
    },
    "interpreter_args": ["--loader", "ts-node/esm", "--project", "./tsconfig.json"]
},
```