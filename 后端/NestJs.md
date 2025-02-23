# `NestJs`

## 基本概念

`NestJS` 是一个用于构建高效、可扩展的 `Node.js `服务器端应用程序的框架（后端开发框架）。采用` TypeScript` 编写，并支持面向对象编程、函数式编程和函数式响应编程



## 环境安装

### 项目初始化

对于`NestJS`的安装，先需要进行`node`的安装，安装稳定版本的`node`之后，可以采用以下的方式进行全局安装：

- `npm install -g @nestjs/cli nodemon ts-node`
- `pnpm add -g @nestjs/cli nodemon ts-node`
- `yarn add -g @nestjs/cli nodemon ts-node`

采取上述任意一种方式即可

安装好`nestjs`后，我们可以通过`nest new 项目名`，来创建一个后端项目，依次选择使用哪个包来管理工具，用于下载一些软件，推荐使用`pnpm`（速度更快，包的管理体积更小）

进入文件，运行项目：`pnpm run start`

***

### 推荐安装插件

- `Prisma`：用于和后端数据库的交互，会进行一些代码提示和语法校验

- `Project Manager`：项目管理插件，可以帮助我们快速的切换项目，后端开发的时候，需要与其他项目进行来回切换，通过打开关闭文件的方式进行切换会比较麻烦

  安装好软件后，在左侧工具栏中会出现一个文件夹的图标，我们可以打开一个项目，点击管理图标，点击上方的保存图标，在顶部任务栏中输入回车，就可以将这个项目放到项目管理文件中进行管理

  我们可以点击编辑项目对其进行分组编辑：

  ```json
  [
  	{
  		"name": "Project-Doc",
  		"rootPath": "d:\\MyOpenSource\\Project-Doc",
  		"paths": [],
  		"tags": ["个人开源项目"],
  		"enabled": true
  	}
  ]
  ```

  > - `name`：表示项目的名称
  > - `rootPath`：表示项目文件的路径
  > - `tags`表示分组标签

  将项目放入管理后，我们在切换项目的时候，可以在`VSCode`中直接进行切换，可以点击左侧工具栏的项目管理软件，再切换项目，也可以在`VSCode`中按下`CTRL+SHIFT+P`，输入`project`，找打这个软件的编辑项目



## 概念介绍和文件结构

### 概念介绍

后端中，常用的概念有：模块、控制器、路由和服务

后端开发过程中，实际的开发业务需要分成不同的模块进行，在互联网应用中，客户通过路由，路由将内容带到控制器中，控制器通过需求，去请求其他模块，其他模块来提供具体的服务

模块是一个个独立的功能，每一个模块会向外面暴露出一些服务（有些服务是模块自己独享的，有些服务是可以暴露给第三方的，即暴露给其他模块）。一个模块中，有可能有路由、有可能有控制器、有可能有服务，但是不一定全有。我们在编写网站时，不会自己编写每一个模块，一般使用一些社区完善的框架，社区中会有一些第三方的包，我们直接拿过来使用即可

总之，我们就是借用模块，来配置路由、配置控制器来完善我们的应用

***

### 文件结构介绍

初始化项目后的文件结构如下所示：

![image-20250222132301563](..\images\image-20250222132301563.png)

- `.prettierrc`文件：代码格式化文件，本身提供了一个格式化操作，但是远远不够，我们需要进行修改，可以参考下面内容进行修改：

  ```json
  {
    "arrowParens": "always",
    "bracketSameLine": true,
    "bracketSpacing": true,
    "embeddedLanguageFormatting": "auto",
    "htmlWhitespaceSensitivity": "css",
    "insertPragma": false,
    "jsxSingleQuote": false,
    "printWidth": 120,
    "proseWrap": "never",
    "quoteProps": "as-needed",
    "requirePragma": false,
    "semi": false,
    "singleQuote": true,
    "tabWidth": 2,
    "trailingComma": "all",
    "useTabs": false,
    "vueIndentScriptAndStyle": false,
    "singleAttributePerLine": false
  }
  ```

- `src`文件夹存放的是源文件，是项目的主要文件，我们在这里面进行逻辑的编写，其主要文件有：

  - `app.controller.spec.ts`文件是做测试的文件，一般不使用，可以进行删除

  - `main.ts`是项目的入口文件，也就是主文件

    ```ts
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';
    
    // 挂载一个根模块
    async function bootstrap() {
      const app = await NestFactory.create(AppModule);
      // 后端服务，需要向用户提供服务，因此需要做一个端口监听，监听的端口设置为3000
      await app.listen(process.env.PORT ?? 3000);
    }
    bootstrap();
    ```

    > 监听的端口是3000，启动项目后，可以通过`localhost:3000`进行访问，就会出现`Hello World!`系统配置的默认路由所对应的服务事件

  - `app.module.ts`：模块定义文件，在根上定义模块`Module`，让这个模块注册到`Nest`中，使其可以识别到，之后在模块内部定义控制器，控制器中要声明装饰器，使系统路由表中可以知道对于具体的路由，需要使用哪个控制器中的哪个方法进行匹配，这个就是控制器和模块路由的关系

    ```ts
    import { Module } from '@nestjs/common';
    import { AppController } from './app.controller';
    import { AppService } from './app.service';
    
    // 定义一个装饰器
    @Module({
      imports: [],
      controllers: [AppController],    // 定义控制器，定义完后，nest就会分析这个控制器
      providers: [AppService],
    })
    export class AppModule {}
    ```

    > 装饰器可以分为类装饰、属性装饰器、方法装饰器和参数装饰器
    >
    > 使用装饰器后，可以改变后续类的原型对象，为其他类的原型对象上增加一些功能。将一个干净的类，定义为`Nest`的模块，用户通过路由访问的时候，我们就会遍历全局模块`modules`列表，查找哪些模块中设置了这个路由，我们就调用这个控制器，具体可以理解为：
    >
    > ```ts
    > // 对于新创建的类，就会继承模块的属性，增强了这个类的功能
    > function a(){}
    > a.prototype._module={
    >     {
    >       imports: [],
    >       controllers: [AppController],
    >       providers: [AppService],
    >     }
    > }
    > 
    > const modules = []
    > modules.push(a)
    > ```

    总之，根上需要有一个模块`Module`，让模块注册到`Nest`中，使其可以被识别，之后再定义控制器，控制器中要声明装饰器，这样系统路由表中就会知道哪个控制器中的哪个方法用来匹配不同的路由

  - `app.controller.ts`：控制器定义文件，提供路由访问功能

    ```ts
    import { Controller, Get } from '@nestjs/common';
    import { AppService } from './app.service';
    
    // 也加了装饰器，下面定义的类就可以被NestJs进行作用，会识别出内部定义的路由
    @Controller()
    export class AppController {
      constructor(private readonly appService: AppService) {}
      // 定义路由：定义一个装饰器，在方法上进行装饰器的声明
      @Get()
      getHello(): string {
        return this.appService.getHello();
      }
    }
    ```

    > @符号修饰的是装饰器，是一个函数，装饰器可以作用在类、属性、方法和参数上
    >
    > 路由定义：
    >
    > - 我们对路由进行定义，默认情况是不进行任何路由的添加，直接主页访问：`localhost:3000`
    >
    >   ```ts
    >   @Get()
    >   getHello(): string {
    >     return this.appService.getHello();
    >   }
    >   ```
    >
    >   在页面中就可以看到，`appService`服务的`getHello()`方法返回的内容
    >
    > - 如果将路由定义为a，如下所示：
    >
    >   ```ts
    >   @Get('a')
    >   getHello(): string {
    >     return 'a';
    >   }
    >   ```
    >
    >   使用域名访问：`http://localhost:3000/a`进行访问，就会显示对应的内容a
    >
    > - 如果在控制器中进行路由的填写：实际的路由会进行继承
    >
    >   ```ts
    >   @Controller('a')
    >   export class AppController {
    >     constructor(private readonly appService: AppService) {}
    >     @Get()
    >     getHello(): string {
    >       return this.appService.getHello();
    >     }
    >   }
    >   ```
    >
    >   使用域名访问：`http://localhost:3000/a`进行访问，网页中会显示`appService`服务的`getHello()`方法返回的内容
    >
    > - 如果在控制器和实际路由中都进行路由的配置：
    >
    >   ```ts
    >   @Controller('a')
    >   export class AppController {
    >     constructor(private readonly appService: AppService) {}
    >     @Get('b')
    >     getHello(): string {
    >       return this.appService.getHello();
    >     }
    >   }
    >   ```
    >
    >   使用域名访问：`http://localhost:3000/a/b`进行访问，网页中会显示`appService`服务的`getHello()`方法返回的内容

  - `app.service.ts`：服务内容文件，存放一些服务的响应事件，提供业务的逻辑代码

    ```ts
    import { Injectable } from '@nestjs/common';
    
    @Injectable()
    export class AppService {
      getHello(): string {
        return 'Hello World!';
      }
    }
    ```

`NestJs`后端也可以进行非前后端分离，可以进行页面的渲染，但是目前推荐使用前后端分离的开发思路，所以，主要用它来进行接口的编写



## 依赖注入

依赖关系的注入，如果不使用后端框架，那我们就需要进行手动的注入（C为B提供服务，B为A提供服务，我们需要手动注入依赖，才能使C为A提供服务），但是有了框架，框架就会自动的为我们进行注入依赖

在控制器代码中，就体现了依赖注入的过程：

```ts
constructor(private readonly appService: AppService) {}
```

> 上述代码是使用`ts`的语法，如果不使用`ts`语法需要在构造函数中声明，编写方式为：
>
> ```js
> this.appService = new AppService() {}
> ```

> 具体含义表示为：
>
> - `private readonly`：变成一个只读不能修改的私有属性
>
> - `appService: AppService)`：声明类型（实例：服务方法），声明完之后，`NestJs`会自动实例化出一个对象，我们就可以直接使用这个实例：`this.appService`
>
>   `AppService`为服务的方法，是从服务文件导入过来的，在服务文件中是一个方法类，用于处理业务逻辑，我们当然可以在控制器中进行处理业务逻辑，但是不推荐，因为这样不能进行复用，而且控制器是提供路由访问功能的，只是接收路由的，有必要与业务逻辑进行区分，因此，将业务逻辑进行区分出去，放到服务文件中，这样这个服务不仅可以提供给这个控制器，还可以提供给其他的控制器



## 服务提供者

`NestJs`的设计是划分不同的模块，每一块业务可以放到一个模块里面

每一个模块有它的独立作用域，模块之间是封闭的，这个模块里面的控制器和服务都只是属于这个模块的，但是模块的某些东西要提供出去使用，这时模块就要往外提供接口（总之，模块默认是独立的，但是也要暴露出一些接口给其他模块来使用（`@Injectable()`装饰器将类变成提供者））

在模块定义文件`app.module.ts`中，这一块就是服务提供者，模块提供装饰器注册后，会有一个容器（`providers`）来管理服务提供者，我们要把服务提供者（`AppService`）放到我们的容器中：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],       // 将服务放到容器中
})
export class AppModule {}
```

注册服务的完整语法应该如下所示：

```ts
providers: [
    {
        provide: AppService,    // 注册服务的名字，可以进行修改，如改为'newName'
        useClass: AppService,
    },
],
```

> 服务我们可以将其想象成一个对象，控制器在使用这个服务的时候，就是拿`key`：`AppService`去容器中找对应的方法，`provide`就是对象中的`key`，我们可以对其名字进行修改，但是在控制器中构造函数调用的时候依赖注入部分就要做相应的修改，使用装饰器进行依赖注入：
>
> ```ts
> constructor(
>     @Inject('newName')
>     private readonly appService: AppService
> ) {}
> ```
>
> `appService: AppService`这两个名字不能一样，前面的是对象属性，后面的是类型修饰
>
> 但是这里`AppService`没有了`key`值匹配的作用，但是还有提供类型支持的作用
>
> 但是我们一般都不会写完整的注册语法，会进行简写

在控制器文件中进行依赖注入后，回去模块的容器中查找，如果没有这个服务提供者，就会报错，提示找不到这个服务，所以我们一定要将服务放到容器中

### 新服务的创建

创建一个新服务，使用命令行进行创建：`nest g s new --no-spec -d`

> - `g s`：创建服务
> - `-d`表示只是返回执行的结果，不会实现真正的服务创建，想要真正的创建服务，要将`-d`去掉

执行结果：在`src/new/new.service.ts`中创建一个新的服务，并更新到根模块的容器中，如果我们不想要子目录，我们可以加上`--flat`，具体修改为`nest g s new --no-spec --flat -d`

这样的执行结果为：在`src/new.service.ts`中创建一个新的服务，并更新到根模块的容器中：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { NewService } from './new.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService, NewService],       // 将服务放到容器中
})
export class AppModule {}
```

之后我们就可以在新的服务中编写我们的业务方法：

```ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class NewService {
    newFunction(){
      return 'Hello!';
    }
}
```

我们通常在`app.service.ts`（该文件可以理解为汇聚服务业务的主文件）文件中自定义服务依赖的注入

```ts
import { Injectable } from '@nestjs/common';
import { NewService } from './new.service';

@Injectable()
export class AppService {
  constructor(private readonly newF: NewService) {}
  getHello(){
    return this.newF.newFunction();
  }
}
```

> `@Injectable()`装饰器将类变成服务提供者，会自动的分析类中的构造函数`constructor`，将依赖实现出来

控制器文件不做修改，还是引用`AppService`服务：

```ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

使用域名访问：`http://localhost:3000`进行访问，网页中会显示`Hello!`

***

### 基本类型提供者注册

普通值类型也可以进行提供者的注册，也就是说，在模块容器中，我们可以自定义提供者，在模块文件中自定义提供者：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService, 
              {   // 自定义基本类型的服务提供者
                  provide: 'info',
                  useValue: {
                      name: 'jlc',
                      age: '24',
                  },
              },
             ],
})
export class AppModule {}
```

可以将这个服务写在外面，使逻辑更加清晰：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

const infoService = {
  provide: 'info',
  useValue: {
      name: 'jlc',
      age: '24',
  },
}

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService, infoService],
})
export class AppModule {}
```

> - `useClass` 期望的是一个类，而不是一个对象字面量。 即用于提供类类型的依赖项
> - `useValue`用于提供具体值（如对象字面量、字符串、数字等）
> - `useFactory`用于提供通过工厂函数创建的依赖项 
> - `useExisting`用于提供现有提供者的别名

如果我们想要进行使用，在服务文件/控制器文件（不推荐，业务逻辑一般放服务文件中）中进行依赖注入即可：

```ts
import { Inject, Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('info') private info: any) {}
  getHello(): string {
    return 'Hello World!' + this.info.name;
  }
}
```

> `@Inject('info')`表示去容器中找到这个`key`值的服务提供者

业务逻辑编写完成后，提供的业务在网站中显示为：`Hello World!jlc`

***

### 动态注册服务提供者

服务是可编程的，我们可以根据不同的场景来注册不同的服务（如：开发的时候使用一个服务，上线的时候使用另一个服务）

在模块文件中动态注册服务提供者，我们需要根据环境变量进行动态的设置，在根目录下新建`.env`文件，定义目前是一个开发环境：

```txt
NODE_ENV=development
```

在模块文件中尝试进行读取环境变量，并实现动态注册服务提供者

读取环境变量时，我们需要安装一个插件`npm install dotenv`，后续环境变量一般使用第三方的包会比较简单

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { DevService } from './dev.service';
import { config } from 'dotenv';
import { join } from 'path';

config({ path: join(__dirname, ',,/.env') })

const configService = {
  provide: 'configService',
  // 动态注册：根据环境变量选择哪个服务
  useClass: process.env.NODE_ENV == 'development' ? DevService : AppService,
}

@Module({
  imports: [],
  controllers: [AppController],
  providers: [configService],
})
export class AppModule {}
```

> `import { config } from 'dotenv'`只是从`dotenv`包中导出了`config`一个内容，如果我们想要包中的所有内容进行导出，统一使用一个变量来接收，我们需要修改为`import dotenv from 'dotenv'`，但是实际上`ts`是不支持的，我们要去`tsconfig.json`中进行配置，添加一个配置项：`"esModuleInterop": true`
>
> 配置完后，就可以使导出的内容进行一块接收，使用的时候`dotenv.`即可
>
> 这样读取环境变量的代码就可以修改为：
>
> ```ts
> import dotenv from 'dotenv';
> import path from 'path';
> 
> dotenv.config({ path: path.join(__dirname, ',,/.env') })
> ```
>
> 后续发展：`import * as dotenv from 'dotenv';` 的方式引入库也可以直接全部导入，不用额外配置`ts`

> 在开发环境下使用`DevService`，在上线环境下使用`AppService`
>
> 具体的两个服务为：
>
> ```ts
> // app.service.ts
> import { Injectable } from '@nestjs/common';
> 
> @Injectable()
> export class AppService {
>   get() {
>     return 'AppService get';
>   }
> }
> ```
>
> ```ts
> // dev.service.ts
> import { Injectable } from '@nestjs/common';
> 
> @Injectable()
> export class DevService {
>   get() {
>     return 'DevService get';
>   }
> }
> ```

在控制器文件中进行注册：

```ts
import { Controller, Get, Inject } from '@nestjs/common';

@Controller()
export class AppController {
  constructor(@Inject('configService') private readonly configService) {}

  @Get()
  getHello(): string {
    return this.configService.get();
  }
}
```

在网页中，会显示`DevService get`，如果`.env`配置文件`NODE_ENV`不为`development`，那么网页中显示`AppService get`

#### 根据环境动态的选择配置项

对于开发环境和生产环境，可能有不同的配置项，我们可以将这些配置项全部放在`src/config`文件夹中，创建用于开发环境的配置项`development.config.ts`：

```ts
export const developmentConfig = {
    url: 'localhost',
    port: 5000
}
```

线上环境的配置项`production.config.ts`：

```ts
export const productionConfig = {
    url: '6.6.6.6',
    port: 3000
}
```

在根目录下新建`.env`文件，定义目前是一个开发环境：

```txt
NODE_ENV=development
```

在模块文件中进行环境变量的加载和配置项的加载：配置项加载的时候是使用它的值而不是类，要用到`useValue`

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { developmentConfig } from './config/development.config';
import { productionConfig } from './config/production.config';
import { AppService } from './app.service';
import dotenv from 'dotenv';
import path from 'path';

dotenv.config({ path: path.join(__dirname, ',,/.env') })

const ConfigService = {
  provide: 'ConfigService',
  useValue: process.env.NODE_ENV == 'development' ? developmentConfig : productionConfig,
}

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService, ConfigService],
})
export class AppModule {}
```

其中`app.service.ts`的内容为：在服务中动态读取配置

```ts
import { Inject, Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(
    @Inject('ConfigService') private readonly ConfigService: Record<string, any>
  ) {}
  getHello(): string {
    return 'Hello World!' + `[${this.ConfigService.url}]`;
  }
}
```

> `Record<string, any>`表示使用类型工具进行定义，表示定义的是一个对象，其值是任意的类型

在控制器中，还是使用`AppService`进行服务的调用：

```ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

> 网站中显示的结果为：`Hello World![localhost]`
>
> 如果环境变量发生了改变，不是`development`了，网站中的内容就变成了：`Hello World![6.6.6.6]`

这样，就实现了根据环境，动态的选择我们的配置项

***

### 工厂函数注册提供者

如果一个服务需要依赖其他的服务，如数据库服务调用配置项服务，这时我们可以把它定义成工厂函数

创建一个数据库服务：`nest g s db --no-spec --flat`

`db.service.ts`服务用于连接我们的数据库，具体代码如下：

```ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class DbService {
    constructor(private readonly options: Record<string, any>) {}
    public connect(){
      return `<h1 style="background:red">连接数据库 - 主机：${this.options.url}</h1>`;
    }
}
```

模块文件的具体代码：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { developmentConfig } from './config/development.config';
import { productionConfig } from './config/production.config';
import { AppService } from './app.service';
import { DbService } from './db.service';
import dotenv from 'dotenv';
import path from 'path';

dotenv.config({ path: path.join(__dirname, ',,/.env') })

const ConfigService = {
  provide: 'ConfigService',
  useValue: process.env.NODE_ENV == 'development' ? developmentConfig : productionConfig,
}

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService, ConfigService, {
      provide: 'DbService',
      inject: ['ConfigService'],
      useFactory(configService) {
        return new DbService(configService)    // 返回一个DbService的实例
      },
  }],
})
export class AppModule {}
```

> 我们要在数据库中使用配置项的信息，因此`DbService`服务要依赖`ConfigService`服务，我们要使用工厂函数来实现，数据库连接的参数在`ConfigService`服务中，使用`inject: ['ConfigService']`会将`ConfigService`服务作为工厂函数的参数传递过来
>
> `inject `属性可以注入多个依赖项。在这种情况下，工厂函数的参数将是一个包含这些依赖项的数组，依赖项的顺序与` inject `数组中的顺序相同。 多个依赖项的示例 假设我们有一个 `AppModule`，其中 `DbService` 依赖于 `ConfigService` 和另一个服务 `OtherService`。可以如下设置` inject` 属性和工厂函数：
>
> ```ts
> import { Module } from '@nestjs/common';
> import { AppController } from './app.controller';
> import { ConfigService } from './config.service';
> import { DbService } from './db.service';
> import { OtherService } from './other.service';
> 
> @Module({
>   imports: [],
>   controllers: [AppController],
>   providers: [ConfigService, OtherService,
>     {
>       provide: 'DbService',
>       inject: [ConfigService, OtherService], // 注入 ConfigService 和 OtherService
>       useFactory: (configService: ConfigService, otherService: OtherService) => {
>         // 使用 ConfigService 和 OtherService 创建 DbService 实例
>         return new DbService(configService, otherService);
>       },
>     },
>   ],
> })
> export class AppModule {}
> ```
>
> 但是在实际的开发中，我们会将其变成模块来开发

在控制器中，引入放在容器中的服务：

```ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(
    private readonly appService: AppService,
    @Inject('DbService')
    private readonly dbService: DbService
  ) {}

  @Get()
  getHello(): string {
    return this.dbService.connect();
  }
}
```

> 网页中显示的内容：`连接数据库 - 主机：localhost`，如果环境变量的值不是`development`，网页中显示：`连接数据库 - 主机：6.6.6.6`

