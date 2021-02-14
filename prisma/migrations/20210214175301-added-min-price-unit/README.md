# Migration `20210214175301-added-min-price-unit`

This migration has been generated by rahuldamodar94 at 2/14/2021, 5:53:01 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."orders" ADD COLUMN "min_price_per_unit" text   ;
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20210214164751-order-token-type..20210214175301-added-min-price-unit
--- datamodel.dml
+++ datamodel.dml
@@ -3,9 +3,9 @@
 }
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url      = env("DATABASE_URL")
 }
 model bids {
   active    Boolean? @default(true)
@@ -95,9 +95,10 @@
   id             Int             @default(autoincrement()) @id
   maker_address  Int?
   min_price      String
   price          String
-  price_per_unit String?         
+  price_per_unit String? 
+  min_price_per_unit String?        
   quantity       String?         
   token_type     String?         @default("ERC721")
   usd_price      Float?
   signature      String?
```

