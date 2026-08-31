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

##

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


#### `products.module.ts`
```bash
import { Module } from '@nestjs/common';
import { ProductsService } from './products.service';
import { ProductsController } from './products.controller';
import { MongooseModule } from '@nestjs/mongoose';
import { Product, ProductSchema } from './schemas/products.schema';


@Module({
  imports: [
    MongooseModule.forFeature([
      { name: Product.name, schema: ProductSchema },
    ])
  ],
  providers: [ProductsService],
  controllers: [ProductsController]
})
export class ProductsModule {}
```

##

#### `products.service.ts`
```bash
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Product } from './schemas/products.schema';
import { Model } from 'mongoose';

@Injectable()
export class ProductsService {
    constructor(
        @InjectModel(Product.name) private productModel: Model<Product>,
    ) {}

    async createProducts(): Promise<Product> {
        const product = new this.productModel({
            title: 'Gaming Laptop',
            tag: [
                { name: 'Electronics' },
                { name: 'Laptop' },
                { name: 'Gaming' },
            ],
        });
        return product.save();
    }

    async getProducts(): Promise<Product[]> {
        return this.productModel.find().exec();
    }
}
```

##

#### `products.controller.ts`
```bash
import { Controller, Get, Post } from '@nestjs/common';
import { ProductsService } from './products.service';

@Controller('products')
export class ProductsController {
    constructor(private productsService: ProductsService) {}

    @Post()
    createProducts() {
        return this.productsService.createProducts();
    }

    @Get()
    getProducts() {
        return this.productsService.getProducts();
    }
}
```
---
