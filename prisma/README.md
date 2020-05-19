# Database API (Database Client API)

[Prisma Client and Migrate](https://github.com/prisma/prisma/blob/master/README.md) is used as Database API

## Steps to getting started

Install dependencies

```sh
$ npm install

# setup and start PostgreSQL
```

### Setting up the database

#### production

```sh
# user .env file at prisma/ folder to configure database

# apply migrations
$ prisma migrate up --experimental
# or
$ npx prisma migrate up --experimental

# create prisma client
$ prisma generate
```

#### development

```sh
# user .env file at prisma/ folder to configure database

# apply migrations
$ prisma migrate up --experimental
# or
$ npx prisma migrate up --experimental

# create migration
$ prisma migrate save --experimental
# or
$ npx prisma migrate save --experimental

# create prisma client to use new updates in modal
$ prisma generate

```