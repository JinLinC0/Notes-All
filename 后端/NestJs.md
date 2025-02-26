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
> - `--no-spec`：不创建测试文件
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

***

### 异步服务提供者

服务在容器注册的时候，可以支持异步操作，`app.module.ts`的代码如下所示：

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
  providers: [
      AppService, 
      ConfigService, 
      // 服务进行异步注册
      {
          provide: 'DbService',
          inject: ['ConfigService'],
          useFactory: async (configService) => {
            return new new Promise((r) => {
                setTimeout(() => {
                    r('abc')
                }, 3000)
            })
          },
      }
  ],
})
export class AppModule {}
```

控制器文件在调用时进行修改，具体文件`app.controller.ts`修改为：

```ts
import { Controller, Get, Inject } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService,
              @Inject('DbService')
              private readonly dbService: string) {}

  @Get()
  getHello(): string {
    return this.dbService;
  }
}
```

项目在启动的时候，服务在进行注册的时候是有3秒的异步等待的，注册完后，在网页中还是正常显示`abc`

服务实例在进行返回的时候，不是进行同步返回的，如查询数据库操作，就是一个典型的异步操作



## 模块

模块是单一的，通常一个模块对应一个具体的业务

### 新模块的创建

创建模块：`nest g mo new -d`

> `-d`表示虚拟创建，返回创建的结果，实际不创建，如果要实际创建，将`-d`去掉即可
>
> 就会在根路径下创建新模块：`src/new/new.module.ts`
>
> `new.module.ts`文件的具体内容为：
>
> ```ts
> import { Module } from '@nestjs/common';
> 
> @Module({})
> export class NewModule {}
> ```
>
> 并在根模块中进行导入更新了这个新模块，`app.module.ts`的内容为：
>
> ```ts
> import { Module } from '@nestjs/common';
> import { AppController } from './app.controller';
> import { NewModule } from './new/new.module';
> 
> @Module({
>   imports: [NewModule],
>   controllers: [AppController],
>   providers: [AppService],
> })
> export class AppModule {}
> ```

模块本身是干不了活的，需要搭配控制器中的路由和业务服务才能实现效果

创建一个服务：`nest g s new --no-spec -d`    生成`src/new/new.service.ts`服务提供者文件

在`new.module.ts`文件中，系统会自动的将这个新建的服务放到同目录下的模块中：

```ts
import { Module } from '@nestjs/common';
import { NewService } from './new.service';

@Module({
    providers: [NewService], // 新建的服务已经注册到这个模块中，作用域是这个模块，在其他模块中不能使用
})
export class NewModule {}
```

我们可以按照这样的方式在创建一个`test`模块，这样系统中就有一个根模块和两个其他模块

之后，我们创建控制器：`nest g co new -- no-spec -d`  生成`src/new/new.controller.ts`控制器文件：

```ts
import { Controller } from '@nestjs/common';

@Controller('new')
export class NewController {}
```

同时控制器会被添加到对应的同文件夹中的模块中：

```ts
import { Module } from '@nestjs/common';
import { NewService } from './new.service';
import { NewController } from './new.controller';

@Module({
    providers: [NewService],
    controllers: [NewController],
})
export class NewModule {}
```

控制器中可以添加方法：

```ts
import { Controller, Get } from '@nestjs/common';

@Controller('new')
export class NewController {
    @Get()
    show() {
        return 'new show method';
    }
}
```

这样我们就可以通过网页地址栏进行访问：`localhost:3000/new`，网页中显示的结果是`new show method`

但是我们一般不将业务的方法放到控制器中，我们一般在控制器中存放路由即可，使用路由去服务提供者中查找具体的方法，从而进行调用

***

### 模块间的调用

服务是有具体的模块作用域的，但是不同的模块间是可以互相依赖的，如果一个模块想要使用其他模块的服务，我们将其他模块导入即可：

```ts
import { Module } from '@nestjs/common';
import { NewService } from './new.service';
import { NewController } from './new.controller';
import { TestModule } from './../test/test.module';

@Module({
    imports: [TestModule],
    providers: [NewService],
    controllers: [NewController],
})
export class NewModule {}
```

`Test`模块有其作用域内的服务，`test.service.ts`的内容为：

```ts
import { Inject, Injectable } from '@nestjs/common';

@Injectable()
export class TestService {
  get(): string {
    return 'test show method';
  }
}
```

我们需要将提供服务的模块进行接口的开发，这样其他模块才可以使用该模块中的所有服务，为`TestModule`模块开放接口，`test.module.ts`修改为：

```ts
import { Module } from '@nestjs/common';
import { TestService } from './test.service';

@Module({
    providers: [
       TestService,
       {
           provide: 'test',
           useValue: '测试服务值',
       }
    ],
    exports: [TestService, 'test']    // 开放服务，将两个服务都暴露出去
})
export class NewModule {}
```

这样在`New`模块控制器中就可以使用`Test`模块中的服务了：

```ts
import { Controller, Get, Inject } from '@nestjs/common';
import { TestService } from './../test/test.service';

@Controller('new')
export class NewController {
    constructor(private readonly test: TestService,
                @Inject('test') private testValue: string) {}
    @Get()
    show() {
        return this.test.get() + this.testValue;
    }
}
```

通过网页地址栏进行访问：`localhost:3000/new`，网页中显示的结果是`test show method测试服务值`

`New`模块成功的使用了`Test`模块中的服务和基本类型数据

#### 全局模块的注册

我们可以将模块注册成全局模块，需要使用`@Global()`装饰器进行全局的声明，在要进行全局注册的模块中进行声明，对`Test`模块进行全局注册：

```ts
import { Global, Module } from '@nestjs/common';
import { TestService } from './test.service';

@Global()
@Module({
    providers: [
       TestService,
       {
           provide: 'test',
           useValue: '测试服务值',
       }
    ],
    exports: [TestService, 'test']    // 开放服务，将两个服务都暴露出去
})
export class NewModule {}
```

> 不管是不是全局的模块，都需要在模块中将接口暴露出来

声明全局之后，其他模块在调用该模块服务时，就不用进行`imports`导入了：

```ts
import { Module } from '@nestjs/common';
import { NewService } from './new.service';
import { NewController } from './new.controller';
import { TestModule } from './../test/test.module';

@Module({
    providers: [NewService],
    controllers: [NewController],
})
export class NewModule {}
```

在`New`模块控制器中还是可以使用`Test`模块中的服务：

```ts
import { Controller, Get, Inject } from '@nestjs/common';
import { TestService } from './../test/test.service';

@Controller('new')
export class NewController {
    constructor(private readonly test: TestService,
                @Inject('test') private testValue: string) {}
    @Get()
    show() {
        return this.test.get() + this.testValue;
    }
}
```

通过网页地址栏进行访问：`localhost:3000/new`，网页中显示的结果是`test show method测试服务值`

`New`模块成功的使用了`Test`模块中的服务和基本类型数据

***

### 管理配置模块

我们一般会编写一个模块来管理我们的基本配置

我们一般在`src`根目录中创建一个文件夹`configure`，这个文件夹专门用来管理我们的常用配置项，具体的配置项`database.ts`所示：

```ts
export default () => ({
    database: {
        host: 'localhost'
    }
})
```

这些配置项来供我们的服务进行读取，我们创建一个配置项模块和服务：

- 创建`config`模块：`nest g mo config -d`
- 创建一个配置项服务：`nest g s config --no-spec -d`

服务处理模块一般不写控制器，我们使用根控制器进行控制即可

配置项服务的任务要求是要能够读取配置项文件夹`configure`中的全部配置文件，`config.service.ts`内容为：

```ts
import { Injectable } from '@nestjs/common';
import { readdirSync } from 'fs';
import path from 'path';

@Injectable()
export class ConfigService {
    // 将数据存储到属性
    config = {} as any;
    // 构造函数中写配置项，先进行读取，在把值进行填入到config中
    constructor() {
        // 定义路径
        const options = { path: path.resolve(__dirname, '../configure') }
        // 读取文件夹下的所有配置文件，readdirSync表示同步读取
        readdirSync(options.path).map(async (file) => {
            if (file.slice(-2) === 'js') {
                // 加载模块，路径连接上文件名称
                const module = await import(path.resolve(options.path, file))
                // 使用点语法进行合并，不同配置文件内容的追加
                // module.default()执行默认导出函数，得到数据
                this.config = { ...this.config, ...module.default() }
            }
        })
    }
    get() {
        return this.config.database.host;
    }
}
```

> `NestJs`后端在编译运行的时候，会生成`dist`文件目录，加载的是该目录中的内容
>
> ![image-20250224182744509](..\images\image-20250224182744509.png)
>
> 如果，修改代码之后，刷新后网页没有效果，我们可以把`dist`文件删除掉，重新运行后端

根模块控制器中调用配置项模块服务的代码，如`app.controller.ts`所示：

```ts
import { Controller, Get } from '@nestjs/common';
import { ConfigService } from './config/config.service';

@Controller()
export class AppController {
  constructor(private readonly config: ConfigService) {}

  @Get()
  getHello() {
    return this.config.get();
  }
}
```

> 使用这个服务，在网页中地址栏中输入：`localhost:3000`，我们就可以在网页中读到对应配置文件中的值：`localhost`

服务开启时，会将我们定义的模块全部进行实例化出来，包括模块中的服务，实例化过程中，其内部的构造函数都会被执行，如果构造函数中有打印信息，会在后台进行打印

静态的配置项模块：模块是不可定制的，是静态的，如果要使用这个模块，直接`imports: [ConfigModule]`倒入进来即可，配置项文件只能放到`configure`里面，不能放到其他位置

#### 优化管理配置项模块

我们需要对管理配置项模块进行进一步的优化，在请求的时候传入`database.host`，就可以得到其配置项中的内容，对于`config.service.ts`文件做如下的修改：

```ts
import { Injectable, Optional } from '@nestjs/common';
import { readdirSync } from 'fs';
import path from 'path';

@Injectable()
export class ConfigService {
    constructor(@Optional() private config = {}) {
        const options = { path: path.resolve(__dirname, '../configure') }
        readdirSync(options.path).map(async (file) => {
            if (file.slice(-2) === 'js') {
                const module = await import(path.resolve(options.path, file))
                this.config = { ...this.config, ...module.default() }
            }
        })
    }
    get(path: string) {
        return path.split('.').reduce((config, name) => {
            return config[name]
        }, this.config)
    }
}
```

> - 我们定义的配置项：`config = {} as any;`，但是，一般情况下参数的声明都在`constructor`构造函数中进行编写：`constructor(private config = {})`，但是这样写是不行的，因为顶部有装饰器`@Injectable()`，系统会将`constructor`构造函数中的内容当作服务来看待，系统会尝试的进行依赖注入，为了解决这个情况，需要使用装饰器`@Optional()`来告诉系统，这里需要使用默认值
>
> - `get`函数可以简化为：
>
>   ```ts
>   get(path: string) {
>       return path.split('.').reduce((config, name) => config[name], this.config)
>   }
>   ```
>
>   这个简化是箭头函数的简化

根模块控制器中调用配置项模块服务的代码，如`app.controller.ts`所示：

```ts
import { Controller, Get } from '@nestjs/common';
import { ConfigService } from './config/config.service';

@Controller()
export class AppController {
  constructor(private readonly config: ConfigService) {}

  @Get()
  getHello() {
    return this.config.get('database.host');
  }
}
```

> 使用这个服务，在网页中地址栏中输入：`localhost:3000`，我们就可以在网页中读到对应配置文件中的值：`localhost`

***

### 动态模块

动态模块可以使模块可以进行动态的配置，可以修改配置项文件的目录，还是可以进行配置项内容的加载

动态模块的思路就是可以进行传参，我们需要设计方法进行传参，在`config.module.ts`模块中定义静态方法：

```ts
import { DynamicModule, Module } from '@nestjs/common';
import { ConfigService } from './config.service';

@Module({
  providers: [ConfigService],
  exports: [ConfigService],
})

export class ConfigModule {
    // 定义静态方法来调用一个动态模块
    static register(options: { name: string }): DynamicModule {
        return {
            module: ConfigModule,    // 模块必须加上
            providers: [{ provide: 'my', useValue: options.name }],  // 定义一个服务
            exports: ['my']    // 暴露服务
        }
    }
}
```

> 动态服务的写法与在`@Module`装饰器中的写法完全一样，需要额外的加上`module: ConfigModule`模块指定；动态服务写法的好处是我们可以为其加上参数

在根模块`app.module.ts`中导入的时候，需要动态的调用这个静态方法：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from './config/config.module';

@Module({
  imports: [ConfigModule.register( { name: 'jlc' } )],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

> 我们可以为这个静态方法传递参数

我们可以在根控制器`app.controller.ts`中注册服务：

```ts
import { Controller, Get, Inject } from '@nestjs/common';

@Controller()
export class AppController {
  constructor(@Inject('my') private my: string) {}

  @Get()
  getHello() {
    return this.my;
  }
}
```

> 在网页中输入`localhost:3000`，结果显示：`jlc`，成功接收到了静态函数传递的值

#### 动态的加载配置项模块

我们可以修改存放配置项文件文件夹的名称，当传入修改后的文件名即可重新加载配置项内容，实现动态的加载配置项模块，核心就是传入和接收具体的配置项文件夹的路径

在根模块`app.module.ts`文件下，进行如下的修改：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from './config/config.module';
import path from 'path';

// 将配置项文件夹的路径抽离出来，configure为当前配置项文件的名称
const configPath = path.resolve(__dirname, './configure')

@Module({
  imports: [ConfigModule.forRoot( { path: configPath } )],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

> 如果配置项文件名称发生了修改，我们只需对`configPath`中的路径名称进行修改即可，实现配置项模块的动态加载

配置项模块`config.module.ts`文件，设计静态方法来动态的接收传入的路径参数

```ts
import { DynamicModule, Module } from '@nestjs/common';
import { ConfigService } from './config.service';

@Module({
  providers: [ConfigService],
  exports: [ConfigService],
})

export class ConfigModule {
    static forRoot(options: { path: string }): DynamicModule {
        return {
            global: true,
            module: ConfigModule,
            providers: [{ provide: 'CONFIG_OPTIONS', useValue: options }],
        }
    }
}
```

> 全局模块的注册，也可以使用上述的方式进行注册，不单单只是使用装饰器进行注册

配置项服务`config.service.ts`，我们要根据传入的参数，进行配置项内容的读取：

```ts
import { Inject, Injectable, Optional } from '@nestjs/common';
import { readdirSync } from 'fs';
import path from 'path';

@Injectable()
export class ConfigService {
    constructor(@Inject('CONFIG_OPTIONS') options: { path: string }, @Optional() private config = {}) {
        readdirSync(options.path).map(async (file) => {
            if (file.slice(-2) === 'js') {
                const module = await import(path.resolve(options.path, file))
                this.config = { ...this.config, ...module.default() }
            }
        })
    }
    get(path: string) {
        return path.split('.').reduce((config, name) => {
            return config[name]
        }, this.config)
    }
}
```



## 配置管理

在模块中，手写过配置项模块，但是为了加快开发的速度，提高开发的效率，通过使用安装`ConfigModule`包来进行配置项的管理

### 安装依赖

在开发`NestJs`后端的时候，一般需要使用到以下的包：

安装命令：`pnpm add -D prisma-binding @prisma/client mockjs @nestjs/config class-validator class-transformer argon2 @nestjs/passport passport passport-local @nestjs/jwt passport-jwt lodash multer dayjs express`

或者：`npm install -D prisma-binding @prisma/client mockjs @nestjs/config class-validator class-transformer argon2 @nestjs/passport passport passport-local @nestjs/jwt passport-jwt lodash multer dayjs express`

> 命令中的一系列开发依赖项：
>
> - `prisma-binding`：`Prisma Binding` 是 `Prisma` 的一个` JavaScript `客户端库，它提供了一组用于构建 `GraphQL` 解析器的工具。可以轻松地将` Prisma Client `集成到` GraphQL `服务器中
>
> - `@prisma/client`：`Prisma Client `是` Prisma` 自动生成的用于与数据库进行交互的 `JavaScript/TypeScript `客户端库。它提供了一组 `API `来执行 `CRUD `操作以及其他与数据库相关的操作
>
> - `mockjs`：`Mock.js` 是一个用于生成随机数据的` JavaScript` 库，它可以帮助你模拟开发和测试环境中的数据
>
> - `@nestjs/config`：`NestJS` 的配置模块，用于管理应用程序的配置信息。可以帮助我们从不同的源（如环境变量、配置文件等）加载配置，并在整个应用程序中使用
>
> - `class-validator`：于验证类和对象的 `TypeScript/JavaScript `库，帮助我们轻松地定义和应用验证规则，以确保数据的有效性和完整性
>
> - `class-transformer`：用于转换类和对象的` TypeScript/JavaScript` 库，可以帮助我们轻松地在不同的数据结构之间进行转换和映射
>
> - `argon2`：一种密码哈希算法，密码安全性更高的替代方案，可以帮助你安全地存储用户密码
> - `@nestjs/passport`：`NestJS `的` Passport `模块，用于在 `NestJS `应用程序中实现身份验证和授权功能
>
> - `passport`：是` Node.js `中一个流行的身份验证中间件，提供了一组易于使用的策略来实现身份验证功能
>
> - `passport-local`：`Passport `中用于处理本地身份验证的策略，它允许用户使用用户名和密码进行身份验证
>
> - `@nestjs/jwt`：`NestJS` 的 `JWT `模块，用于在 `NestJS `应用程序中实现` JWT`（`JSON Web Token`）身份验证和授权功能
>
> - `passport-jwt`：`Passport` 中用于处理 `JWT `身份验证的策略，它允许我们使用 `JWT` 来实现无状态的身份验证
>
> - `lodash`：`Lodash` 是` JavaScript `中一个实用工具库，提供了许多常用的工具函数，可以帮助你简化 `JavaScript `编程
>
> - `multer`：`Multer` 是一个 `Node.js` 中用于处理文件上传的中间件，它可以帮助我们处理表单数据中的文件上传功能
>
> - `dayjs`：`Day.js `是一个轻量级的` JavaScript `日期处理库，它提供了简单易用的 `API `来处理日期和时间

`@nestjs/config`这个包是用来管理我们配置项的，使用方式和我们手写的配置项模块类似

***

### 加载配置项

#### 根配置文件`.env`中配置项加载

我们先要到`app.module.ts`文件中导入并定义这个模块：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [ConfigModule.forRoot({
      isGlobal: true,
  })],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

在使用的时候，我们去我们创建的`.env`（`.env`文件在根目录中创建）配置文件中读取我们添加的配置项内容：

```ts
BLOG_NAME = JBLOG
```

> 在本地开发的时候，会有一些其他的配置项，如阿里云的密钥等，这些密钥一般是隐私的，我们不能将其提交到代码仓库的，所以`.env`这个文件一般要在`git`的忽略文件中进行忽略，如果代码部署到服务器上的时候，我们在拉取代码后，将`.env`这个文件复制过去即可

`ConfigModule`模块会自动的读取这个配置文件

在控制器文件`app.controller.ts`中，将读取配置项服务引入：

```ts
import { Controller, Get } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService,
              private readonly config: ConfigService) {}

  @Get()
  getHello() {
    return this.config.get('BLOG_NAME');
  }
}
```

> 在网页地址中输入：`localhost:3000`，可以看到读取的配置项内容：`JBLOG`

对于配置项操作，一般简单的项目，我们将配置项内容放在`.env`文件中即可

#### 其他配置文件的配置项加载

我们可以在`src`文件夹中自定义创建一些模块的配置文件，比如，为根模块创建一个配置文件：`app.config.ts`，具体内容为：

```ts
export default () => ({
    app: {
        name: 'JBLOG'
    }
})
```

`ConfigModule`模块是不认这个配置文件的，我们需要在`app.module.ts`文件中为`ConfigModule`模块添加一个选项，将这个配置文件加载过来：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from '@nestjs/config';
import { appConfig } from './app.config'

@Module({
  imports: [ConfigModule.forRoot({
      isGlobal: true,
      load: [appConfig]
  })],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

> 加载之后，自定义的配置文件就会和`.env`根配置文件进行合并，可以一起被`ConfigModule`模块读取到

在控制器文件`app.controller.ts`中，将读取配置项服务引入：

```ts
import { Controller, Get } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService,
              private readonly config: ConfigService) {}

  @Get()
  getHello() {
    return this.config.get('app.name');
  }
}
```

> 在网页地址中输入：`localhost:3000`，可以看到读取的配置项内容：`JBLOG`

***

### `process.env`环境变量

`process.env`可以读取到当前的环境变量，包括当前的工作目录、操作系统等等，其中项目中的`.env`配置也会加载到环境变量中

对于`.env`文件的内容：

```ts
BLOG_NAME = JBLOG
```

我们也可以使用`process.env.BLOG_NAME`的方式进行配置内容的读取，不单单只是通过`ConfigService`服务进行配置项内容的读取

#### 根据当前环境读取不同配置项内容

`.env`文件，加上`NODE_ENV`项进行区分不同的开发环境：

```ts
NODE_ENV = development
BLOG_NAME = JBLOG
```

在`app.config.ts`文件中进行当前环境的判断，具体内容为：

```ts
export default () => ({
    app: {
        name: 'JBLOG',
        isDev: process.env.NODE_ENV === 'development'
    }
})
```

在`app.module.ts`文件中为`ConfigModule`模块添加一个选项，将这个配置文件加载过来：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from '@nestjs/config';
import { appConfig } from './app.config'

@Module({
  imports: [ConfigModule.forRoot({
      isGlobal: true,
      load: [appConfig]
  })],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

在控制器文件`app.controller.ts`中，将读取配置项服务引入：

```ts
import { Controller, Get } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService,
              private readonly config: ConfigService) {}

  @Get()
  getHello() {
      if (this.config.get('app.isDev')){
          return this.config.get('app.name');
      }
  }
}
```

> 当前为开发环境的时候，进行对应内容的打印
>
> 在网页地址中输入：`localhost:3000`，可以看到读取的配置项内容：`JBLOG`

这样对于不同的开发环境选择不同的策略，如对于生产环境，是不是需要进行运行日志的打印

***

### 多配置文件定义

多配置文件就是我们采用多个文件来定义不同模块的配置项，最后将其都放到一个`config`文件夹中进行统一管理，一般在这个文件夹中写一个入口文件`index.ts`，进行所有配置项的统一导出管理

在`config`文件夹下编写两个模块的配置项文件，如：`app.config.ts`和`db.config.ts`：

```ts
export default () => ({
    app: {
        name: 'JBLOG'
    }
})
```

```ts
export default () => ({
    database: {
        host: 'localhost'
    }
})
```

编写入口文件`index.ts`：

```ts
import appConfig from './app.config';
import dbConfig from './db.config';

export default [appConfig, dbConfig];
```

在根模块中，配置模块调用加载，使用`ConfigModule`模块可以找到这个自定义的配置项内容：

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from '@nestjs/config';
import { config } from './config'

@Module({
  imports: [ConfigModule.forRoot({
      isGlobal: true,
      load: [...config]    // 是一个数组，需要进行展开
  })],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

但是这种情况在调用的时候`this.config.get('app.name')`，输入`app`，系统是不会给出类型提示的

#### 命名空间配置项

为了解决上述没有代码提示的情况，我们需要进行基于命名空间的配置，在配置文件中进行修改，以`db.config.ts`修改为例：

```ts
import { registerAs } from '@nestjs/config';

export default registerAs('database', () => ({
	host: 'localhost'
}))
```

在控制器服务调用的`app.controller.ts`文件中进行依赖的注入和类型的添加：

```ts
import { Controller, Get } from '@nestjs/common';
import { ConfigService, ConfigType } from '@nestjs/config';
import { AppService } from './app.service';
import dbConfig from './config/db.config'

@Controller()
export class AppController {
  constructor(private readonly appService: AppService,
              private readonly config: ConfigService
              // 将配置内容当作模块文件中providers的对象内容进行使用，调用时需进行注入
              @Inject(dbConfig.KEY) private database: ConfigType<typeof dbConfig>) {}

  @Get()
  getHello() {
      return this.database.host    // 这时再输入database后，就会有代码提示，告知对象下的所有KEY
  }
}
```

> `@nestjs/config`包中为我们提供了类型提示工具：`ConfigType`，将所需要的配置内容`typeof dbConfig`传入即可，后续在使用的时候，就会有代码提示



## 数据库相关

`NestJs`后端在开发的过程中，需要与数据库进行交互，不断的进行存储和读取数据库中的数据，有许多的第三方包可以进行使用，推荐使用`prisma`包进行数据库的管理

### `prisma`的安装

安装命令：`pnpm add -D prisma @types/mockjs @nestjs/mapped-types @types/passport-local @types/passport-jwt @types/express @types/lodash @types/multer`

也可以使用`npm`进行安装：`npm install -D prisma @types/mockjs @nestjs/mapped-types @types/passport-local @types/passport-jwt @types/express @types/lodash @types/multer`

***

### 配置数据库连接

使用`prisma`包来执行数据库操作的相关指令可以查看：`npx prisma`

- 初始化数据库：`npx prisma init`

  初始化完成后，就会在根目录下生成一个`prisma`，内部有一个数据库文件：`schema.prisma`：

  ```ts
  generator client {
    provider = "prisma-client-js"
  }
  
  datasource db {
    provider = "postgresql"
    url      = env("DATABASE_URL")
  }
  ```

  > 我们可以在这个文件中定义我们的数据库结构

  同时还在根目录下生成一个`.env`文件，用于存放我们数据库的配置：

  ```ts
  DATABASE_URL="postgresql://johndoe:randompassword@localhost:5432/mydb?schema=public"
  ```

- 连接配置：我们现在使用的是`MySQL`数据库，因此我们要在`.env`配置文件中进行如下的修改：

  ```ts
  DATABASE_URL="mysql://root:552259@localhost:3306/nest-blog"
  ```

  > 同时加入数据库的连接信息
  >
  > 账号:密码@主机地址:端口/连接的库

- 建表：使用数据库表迁移文件文件：`schema.prisma`中进行表结构的声明，数据表在开发使用的过程中会不断的进行变化，在迁移文件中声明表结构，每次修改文件，就会生成一个新的版本

  修改`schema.prisma`文件，并新增一个新的表模型：

  ```ts
  generator client {
    provider = "prisma-client-js"
  }
  
  datasource db {
    provider = "mysql"
    url      = env("DATABASE_URL")
  }
  
  model User {
    id        BigInt   @id @default(autoincrement()) @db.UnsignedBigInt
    email     String   @db.Char(50)
    password  String
    name      String?
    github    String?
    createdAt DateTime @default(now())
    updatedAt DateTime @updatedAt
  }
  ```

  > `@id`表示声明为主键；`@default(autoincrement())`定义默认值是自增的；`@db.UnsignedBigInt`设置值是非负的
  >
  > `String?`表示类型是字符串，且不是必填项
  >
  > `@default(now())`：获取该条数据创建时的时间戳
  >
  > `@updatedAt`：获取数据内容有更新时的时间戳

- 数据库迁移：在定义完表结构后，我们要执行数据库的迁移（形成一个版本，后续如果表结构发生变化，会会生成新的版本）

  迁移命令：`npx prisma migrate dev`

  依次输入`Yes`；本次迁移的描述（对这次迁移操作做了什么）第一个可以写`init`

  迁移完后就会看到本次迁移的执行结果和内部包含的本次迁移的`sql`文件：

  ![image-20250225105633758](..\images\image-20250225105633758.png)

  > `sql`文件的内容只有对本次修改的表结构内容进行设计，之前的表结构本次没有修改的不会体现

  同时，对应的数据库中就会生成配置的数据库和表结构：

  ![image-20250225110132905](..\images\image-20250225110132905.png)

  最后将项目部署到服务器上的时候，我们只需要将迁移文件执行一下，系统就会按照迁移文件的日期一个一个的进行跑，将数据库迁移过去

  命令：`npx prisma migrate reset`，将数据库重置一下，按照迁移文件依次的进行执行

***

### 多表关联的模型定义

博客项目的数据库构建，初步思考：

- 用户注册信息表：`User`

  ```ts
  model User {
    id        BigInt   @id @default(autoincrement()) @db.UnsignedBigInt
    email     String   @db.Char(50)
    password  String
    name      String?
    avatar    String?   // 头像
    github    String?
    createdAt DateTime @default(now())
    updatedAt DateTime @updatedAt
  }
  ```

- 博客栏目表：`category`

  ```ts
  model category {
    id       BigInt    @id @default(autoincrement()) @db.UnsignedBigInt
    title    String    @db.Char(50)
    articles article[]
  }
  ```

  > 一个主键，一个分类的标题
  >
  > 文章表和栏目表的关系：一个栏目可以包含多个文章（一对多的关系）：`articles`字段与文章模型`article`关联，要与多个文章关联，要加`[]`

- 具体文章表：`article`

  ```ts
  model article {
    id         BigInt    @id @default(autoincrement()) @db.UnsignedBigInt
    title      String    @db.Char(50)
    content    String    @db.Text
    thumb      String
    createdAt  DateTime  @default(now())
    updatedAt  DateTime  @updatedAt
    category   category @relation(fields: [categoryId], references: [id], onDelete: Cascade)
    categoryId BigInt    @db.UnsignedBigInt
  }
  ```

  > 主键`id`；文章的标题`title`；正文内容`content`；图片描述`thumb`
  >
  > 栏目的编号：`categoryId`（多的一方要记录少的一方，每篇文章要记录对应栏目的编号）；`category`表示定义关联方式：`categoryId`与栏目表的`id`进行关联，并保持两者的类型一致，`onDelete: Cascade`表示对关联关系进行约束，如果某个栏目删除后，对应文章表中该栏目下的具体文章也会删除
  >
  > 一般情况下，在栏目表中进行了关联，具体文章表的`category`和`categoryId`行会自动的出来，如果没有出来，我们可以在终端执行：`npx prisma format`，这行指令会自动分析文件结构，并补全格式化

  在数据库的关联中，我们就可以看到这个外键关联将关联关系定义了：

  ![image-20250225131827857](..\images\image-20250225131827857.png)

  表中的作者与用户信息表中的作者进行关联

***

### 数据填充

我们使用自动填充来生成测试数据，而不是通过手动的方式进行录入

需要在`package.json`中配置一条命令：

```json
"prisma": {
	"seed": "ts-node prisma/seed.ts"
},
```

> 这个命令用于读取`prisma`文夹中的`seed.ts`文件，并执行这个文件，因此，我们在在这个路径下新建一个`seed.ts`文件，我们要在这个文件下执行数据的填充

添加一条数据的代码示例：代码用于操作`mysql`中的对象

```ts
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();   // 生成一个操作mysql的客户端

async function main() {
  await prisma.user.create({
    data: {
      email: "2794810071@qq.com",
      password: "552259",
      name: "JinLinC0",
      github: "https://github.com/JinLinC0"
    },
  });
}

main()
```

> 除了创建`create`外，还有更新`update`、统计`count`等等操作命令

在终端中运行：`npx prisma db seed`，执行成功后，就会将数据填充到数据库对应的表中，或者使用`npx prisma migrate reset`命令，使用这条命令后，会重新执行一次完整的数据迁移，并执行数据填充操作

![image-20250225150005830](..\images\image-20250225150005830.png)

#### 使用`mock.js`进行随机大量数据的填充

安装`mock.js`：`npm install mockjs`

引入`mock.js`包随机生成数据进行数据的填充：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";

const prisma = new PrismaClient();

async function main() {
    for (let i = 0; i < 20; i++) {
        await prisma.user.create({
            data: {
                email: Random.email(),
                password: Random.string(),
                name: Random.string(),
                avatar: Random.image("200x200"),
                github: Random.url()
            },
        });
    }
}

main()
```

> 终端中运行：`npx prisma db seed`或者`npx prisma migrate reset`，就可以在数据库的对应表中看到随机生成的20条数据：
>
> ![image-20250225151603535](..\images\image-20250225151603535.png)

对于每张表随机数据的填充，我们应该将其放到独立的文件中，在`prisma`文件夹内创建一个文件夹`seeds`，用来存放不同表的随机数据填充文件，对于用户数据填充文件，新建一个`user.ts`文件：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";

const prisma = new PrismaClient();

export async function user() {
    for (let i = 0; i < 20; i++) {
        await prisma.user.create({
            data: {
                email: Random.email(),
                password: Random.string(),
                name: Random.string(),
                avatar: Random.image("200x200"),
                github: Random.url()
            },
        });
    }
}
```

同理，创建用于其他表的数据填充文件，栏目的填充文件：`category.ts`：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";

const prisma = new PrismaClient();

export async function category() {
    for (let i = 0; i < 5; i++) {
        await prisma.category.create({
            data: {
                title: Random.ctitle(2, 5)
            },
        });
    }
}
```

文章的填充文件：`article.ts`：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";

const prisma = new PrismaClient();

export async function article() {
    for (let i = 0; i < 10; i++) {
        await prisma.article.create({
            data: {
                title: Random.ctitle(),
                content: Random.cparagraph(10, 50),
                thumb: Random.image("200x200"),
                categoryId: Random.integer(1, 5)   // 外键关联，对应栏目表的id
            },
        });
    }
}
```

最后，在`seed.ts`文件中进行各种表数据填充函数的导入和使用：

```ts
import { user } from "./seeds/user";
import { category } from "./seeds/category";
import { article } from "./seeds/article";

async function main() {
    user();
    category();
    article()
}

main()
```

终端中运行：`npx prisma db seed`或者`npx prisma migrate reset`，将数据进行填充

##### 使用帮助函数来简化代码

对于冗余的代码，我们可以在`prisma`文件夹中创建一个帮助文件`helper.ts`，来同一存放具体的方法函数，供其他的文件使用，对于数据的循环填充，我们可以将循环抽离到帮助文件中，实际上就是一个回调函数，供其他使用者进行调用：

```ts
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

export async function create(count = 1, callback: (prisma: PrismaClient) => void) {
    for (let i = 0; i < count; i++) {
        callback(prisma);   // 循环执行回调函数
    }
}
```

在`user.ts`文件中，引入帮助函数进行简化代码：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";
import { create } from "../helper";

export async function user() {
    create(20, async (prisma: PrismaClient) => {
        await prisma.user.create({
            data: {
                email: Random.email(),
                password: Random.string(),
                name: Random.string(),
                avatar: Random.image("200x200"),
                github: Random.url()
            },
        });
    })
}
```

#### 数据填充的异步阻塞

对于栏目表和文章表，这两张表是存在一个外键关联，具有外键约束，在数据填充执行的时候使用的是一个异步函数，文章表填充数据和栏目表填充数据时不是阻塞的，这个时候可能会出现栏目正在创建的时候，文章也在创建，文章创建的时候对应的栏目`id`如果还不存在，这时就匹配不上，这时就会出现问题：`Foreign key constraint failed on the field: 'categoryId'`

因此我们需要在`seed.ts`文件中，让各个方法函数依次进行执行（主要是让文章填充函数等栏目填充函数执行完后，再进行调用执行），之前的情况是没有指定异步执行的，现在设置栏目表执行异步，其他两张表不进行异步操作，具体修改如下：`seed.ts`文件：

```ts
import { user } from "./seeds/user";
import { category } from "./seeds/category";
import { article } from "./seeds/article";

async function main() {
    user();
    await category();    // 执行时阻塞，执行完后再执行后面的内容
    article();
}

main()
```

`user.ts`文件修改为：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";
import { create } from "../helper";

export function user() {
    create(20, async (prisma: PrismaClient) => {
        await prisma.user.create({
            data: {
                email: Random.email(),
                password: Random.string(),
                name: Random.string(),
                avatar: Random.image("200x200"),
                github: Random.url()
            },
        });
    })
}
```

`category.ts`文件修改为：将栏目填充函数定义成异步的，栏目的数据填充完后，再执行后面的

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";
import { create } from "../helper";

export async function category() {
    await create(5, async (prisma: PrismaClient) => {
        await prisma.category.create({
            data: {
                title: Random.ctitle(2, 5)
            },
        });
    })
}
```

`article.ts`文件修改为：

```ts
import { PrismaClient } from "@prisma/client";
import { Random } from "mockjs";
import { create } from "../helper";

export function article() {
    create(10, async (prisma: PrismaClient) => {
        await prisma.article.create({
            data: {
                title: Random.ctitle(),
                content: Random.cparagraph(10, 50),
                thumb: Random.image("200x200"),
                categoryId: Random.integer(1, 5),
            },
        });
    })
}
```

`helper.ts`文件修改为：

```ts
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

export async function create(count = 1, callback: (prisma: PrismaClient) => Promise<void>) {
    for (let i = 0; i < count; i++) {
        await callback(prisma);   // 所有异步函数执行
    }
}
```

以上操作修改完后，就不会出现：``Foreign key constraint failed on the field: 'categoryId'``报错



