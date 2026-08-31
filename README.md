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
---
