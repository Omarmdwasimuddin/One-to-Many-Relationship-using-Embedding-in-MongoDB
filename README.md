## One-to-Many Relationship using Embedding in MongoDB


#### Create module, service, controller
```bash
nest g module products
```
```bash
nest g service products
```
```bash
nest g controller products
```
---



>#### create file folder- schemas/tag.schema.ts & schemas/products.schema.ts
#### `tag.schema.ts`
```bash
import { Prop, Schema } from "@nestjs/mongoose";


@Schema()
export class Tag {
    @Prop()
    name: string;
}
```

#### `products.schema.ts`
```bash
import { Prop, Schema, SchemaFactory } from "@nestjs/mongoose";
import { Document } from "mongoose";
import { Tag } from "./tag.schema";

@Schema()
export class Product extends Document {
    @Prop()
    title: string;

    @Prop( {type: [Tag]} )
    tag: Tag[];
}

export const ProductSchema = SchemaFactory.createForClass(Product);
```
---
