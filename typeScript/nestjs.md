# NestJS

*Source:* learn.nestjs.com/courses

* Node must be already installed

## Install NestJS CLI -globally
`npm i -g @nestjs/cli`

## Checkout your Nest version (once installed)
`nest --version`

## Output all Nest commands
`nest --help`

## Create new Nest application
`nest new <app-name>`

## Start Nest application
`npm run start`
The project should be available at PORT 3000
http://localhost:3000

## Running NestJS in Development Mode
`npm run start:dev`

To run after cloning from github make sure to run first
`npm install`

## Generate a Controller
`nest generate controller`
it will prompt you to add a name for the controller.

### Short hand for generating a controller
`nest g co`

## Generate a Service with Nest CLI

`nest generate service`
or
`nest g s`
 - It will prompt you to enter a name for the service

## Generate a module
`nest g module <module name>`

  Modules contain four things:
    - Controllers: Optional listing of controllers defined in the module which have to be instantiated.
    - Exports: Optional list of the subset of providers that are provided by the module and should be available in other module which import the module
    - Imports: Optional list of imported modules that export the providers which are required in the module
    - Providers: Optional list of providers that will be instantiated by the Nest injector and that may be shared at least across the module.

## Generate a DTO Class

D.T.O = Data Transfer Object

`nest g class coffees/dto/create-coffee.dto --no-spec`

## Using route parameters example

``` ts
/* CoffeesController FINAL CODE */
import { Controller, Get, Param } from '@nestjs/common';

@Controller('coffees')
export class CoffeesController {
  @Get()
  findAll() {
    return 'This action returns all coffees';
  }

  @Get(':id')

  // findOne(@Param() params) {
  //  return 'this action returns #${params.id} coffee';
  //  }

  findOne(@Param('id') id: string) {
    return `This action returns #${id} coffee`;
  }
}
```

## Handling Request Body/Payload

``` typescript
import { Body, Controller, Get, Param, Post } from '@nestjs/common';

@Controller('coffees')
export class CoffeesController {
  @Get()
  findAll() {
    return 'This action returns all coffees';
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return `This action returns #${id} coffee`;
  }

  @Post()
  create(@Body() body) {
    return body;
    // return `This action creates a coffee`;
  }
}
```
## Install Dependencies
`npm i class-validator class-transformer`

## Install @nestjs/mapped-types
`npm i @nestjs/mapped-types`


---------

## Docker-compose

inside the project's root directory
`$ touch docker-compose.yml`

Add
``` yml

version: "3"
services:
  db:
    image:  postgres
    restart: always
    ports:
      - "5432:5432"
    environment:
       POSTGRES_PASSWORD: pass123
```

  * Start containers in detached / background mode
    `$ docker-compose up -d`

  * Stop containers
    `$ docker-compose down`

## TypeORM

Install dependencies

`$ npm install @nestjs/typeorm typeorm pg`



Update app.module.ts

``` typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { CoffeesModule } from './coffees/coffees.module';

@Module({
  imports: [
    CoffeesModule,
    TypeOrmModule.forRoot({
      type: 'postgres', // type of our database
      host: 'localhost', // database host
      port: 5432, // database host
      username: 'postgres', // username
      password: 'pass123', // user password
      database: 'postgres', // name of our database,
      autoLoadEntities: true, // models will be loaded automatically
      synchronize: true, // your entities will be synced with the database(recommended: disable in prod)
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
