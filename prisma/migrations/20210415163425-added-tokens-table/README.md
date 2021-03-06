# Migration `20210415163425-added-tokens-table`

This migration has been generated by rahuldamodar94 at 4/15/2021, 4:34:25 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "mainnet"."orders" ADD COLUMN "tokensCategories_id" integer   ,
ADD COLUMN "tokensToken_id" text   ;

ALTER TABLE "mainnet"."orders" ADD FOREIGN KEY ("tokensToken_id", "tokensCategories_id")REFERENCES "mainnet"."tokens"("token_id","categories_id") ON DELETE SET NULL  ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20210414073104-added-composite-key-category..20210415163425-added-tokens-table
--- datamodel.dml
+++ datamodel.dml
@@ -3,15 +3,15 @@
 }
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url      = env("DATABASE_URL")
 }
 model bids {
   active    Boolean? @default(true)
   created   DateTime @default(now())
-  id        Int      @default(autoincrement()) @id
+  id        Int      @id @default(autoincrement())
   orders_id Int
   price     String
   signature String
   status    Int?     @default(0)
@@ -24,9 +24,9 @@
 model categories {
   active              Boolean?              @default(true)
   created             DateTime              @default(now())
   description         String?
-  id                  Int                   @default(autoincrement()) @id
+  id                  Int                   @id @default(autoincrement())
   img_url             String?
   name                String                @unique
   updated             DateTime              @default(now())
   url                 String?
@@ -36,29 +36,48 @@
   isMetaTx            Boolean?              @default(false)
   categoriesaddresses categoriesaddresses[]
   orders              orders[]
   assetmigrate        assetmigrate[]
-  disabled            Boolean?               @default(false)
+  disabled            Boolean?              @default(false)
+  tokens              tokens[]
 }
 model categoriesaddresses {
   active           Boolean?   @default(true)
-  address          String     
+  address          String
   ethereum_address String
   categories_id    Int
   chain_id         String
   created          DateTime   @default(now())
   id               Int        @default(autoincrement())
   updated          DateTime   @default(now())
   categories       categories @relation(fields: [categories_id], references: [id])
+
   @@id([address, chain_id])
 }
+model tokens {
+  token_id      String
+  categories_id Int
+  image_url     String?
+  description   String?
+  external_url  String?
+  name          String?
+  attributes    String?
+  created       DateTime   @default(now())
+  id            Int        @default(autoincrement())
+  updated       DateTime   @default(now())
+  categories    categories @relation(fields: [categories_id], references: [id])
+  orders        orders[]
+
+  @@id([token_id, categories_id])
+}
+
 model erc20tokens {
   active               Boolean?               @default(true)
   created              DateTime               @default(now())
   decimal              Int
-  id                   Int                    @default(autoincrement()) @id
+  id                   Int                    @id @default(autoincrement())
   name                 String
   symbol               String                 @unique
   isMetaTx             Boolean                @default(true)
   updated              DateTime               @default(now())
@@ -71,67 +90,67 @@
   active      Boolean?    @default(true)
   address     String      @unique
   chain_id    String
   created     DateTime    @default(now())
-  id          Int         @default(autoincrement()) @id
+  id          Int         @id @default(autoincrement())
   token_id    Int
   updated     DateTime    @default(now())
   erc20tokens erc20tokens @relation(fields: [token_id], references: [id])
 }
 model favourites {
   created  DateTime @default(now())
-  id       Int      @default(autoincrement()) @id
+  id       Int      @id @default(autoincrement())
   order_id Int
   updated  DateTime @default(now())
   users_id Int
   orders   orders   @relation(fields: [order_id], references: [id])
   users    users    @relation(fields: [users_id], references: [id])
 }
 model orders {
-  active         Boolean?        @default(true)
-  chain_id       String
-  categories_id  Int
-  created        DateTime        @default(now())
-  erc20tokens_id Int
-  expiry_date    DateTime?
-  id             Int             @default(autoincrement()) @id
-  maker_address  Int?
-  min_price      String
-  price          String
-  price_per_unit String? 
-  min_price_per_unit String?        
-  quantity       String?         
-  token_type     String?         @default("ERC721")
-  usd_price      Float?
-  signature      String?
-  status         Int?            @default(0)
-  taker_address  Int?
-  taker_amount   String?
-  maker_amount   String?
-  tokens_id      String
-  txhash         String?
-  name           String?         @default("")
-  type           String
-  buyer          Int?
-  seller         Int?
-  updated        DateTime        @default(now())
-  views          Int?            @default(0)
-  categories     categories      @relation(fields: [categories_id], references: [id])
-  erc20tokens    erc20tokens     @relation(fields: [erc20tokens_id], references: [id])
-  buyer_users    users?          @relation("buyer_orders", fields: [buyer], references: [id])
-  seller_users   users?          @relation("seller_orders", fields: [seller], references: [id])
-  bids           bids[]
-  notifications  notifications[]
-  favourites     favourites[]
+  active             Boolean?        @default(true)
+  chain_id           String
+  categories_id      Int
+  created            DateTime        @default(now())
+  erc20tokens_id     Int
+  expiry_date        DateTime?
+  id                 Int             @id @default(autoincrement())
+  maker_address      Int?
+  min_price          String
+  price              String
+  price_per_unit     String?
+  min_price_per_unit String?
+  quantity           String?
+  token_type         String?         @default("ERC721")
+  usd_price          Float?
+  signature          String?
+  status             Int?            @default(0)
+  taker_address      Int?
+  taker_amount       String?
+  maker_amount       String?
+  tokens_id          String
+  txhash             String?
+  name               String?         @default("")
+  type               String
+  buyer              Int?
+  seller             Int?
+  updated            DateTime        @default(now())
+  views              Int?            @default(0)
+  categories         categories      @relation(fields: [categories_id], references: [id])
+  erc20tokens        erc20tokens     @relation(fields: [erc20tokens_id], references: [id])
+  buyer_users        users?          @relation("buyer_orders", fields: [buyer], references: [id])
+  seller_users       users?          @relation("seller_orders", fields: [seller], references: [id])
+  bids               bids[]
+  notifications      notifications[]
+  favourites         favourites[]
 }
 model users {
   active        Boolean?        @default(true)
   address       String          @unique
   created       DateTime        @default(now())
-  id            Int             @default(autoincrement()) @id
+  id            Int             @id @default(autoincrement())
   updated       DateTime        @default(now())
   bids          bids[]
   favourites    favourites[]
   buyer_orders  orders[]        @relation("buyer_orders")
@@ -144,17 +163,17 @@
   active   Boolean? @default(true)
   username String   @unique
   password String
   created  DateTime @default(now())
-  id       Int      @default(autoincrement()) @id
+  id       Int      @id @default(autoincrement())
   updated  DateTime @default(now())
 }
 model notifications {
   read     Boolean  @default(false)
   active   Boolean  @default(true)
   created  DateTime @default(now())
-  id       Int      @default(autoincrement()) @id
+  id       Int      @id @default(autoincrement())
   message  String
   type     String   @default("ORDER")
   updated  DateTime @default(now())
   order_id Int
@@ -164,9 +183,9 @@
 }
 model assetmigrate {
   created       DateTime   @default(now())
-  id            Int        @default(autoincrement()) @id
+  id            Int        @id @default(autoincrement())
   updated       DateTime   @default(now())
   type          String
   txhash        String     @unique
   exit_txhash   String?    @unique
```


