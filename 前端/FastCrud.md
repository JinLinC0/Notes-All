# `FastCrud`

## 基本概念

`FastCrud `（简称`fs`） 是基于`Vue3`的面向配置的`crud`开发框架，快速开发`crud`功能，可作为低代码平台的基础框架，只需简单编写`crud`配置就可以构建表格，在`crud`中公共配置都可以在`crud.tsx`页面里面被覆盖 ，没有则继承默认的规则

`FastCrud`运行过程：

构建`crudOptions` --> 调用`useCrud` --> 获得`crudBinding` --> 传入`fs-crud`组件

使用`fast-crud`，需要掌握如何配置正确的`crudOptions`，即可完成一个`crud`的开发工作



## 表格与配置对应关系

- `crudOptions.table` 表格配置
- `crudOptions.search` 查询框配置
- `crudOptions.pagination` 分页配置
- `crudOptions.columns` 字段配置
- `crudOptions.columns[key].column` 表格列配置（图中`columns[key]`部分）
- `crudOptions.columns[key].search` 查询表单内字段配置(图中状态字段在查询框内的配置)

- `crudOptions.form` 添加表单配置
- `crudOptions.viewForm` 查看表单配置
- `crudOptions.editForm` 编辑表单配置
- `crudOptions.addForm` 添加表单配置
- `crudOptions.columns` 字段配置
- `crudOptions.columns.form` 所有表单内该字段公共配置
- `crudOptions.columns.viewForm` 查看表单内字段配置
- `crudOptions.columns.addForm` 添加表单内字段配置
- `crudOptions.columns.editForm` 编辑表单内字段配置

调用`useCrud`：主要作用是初始化`crud`，将`crudOptions`转化为`crudBinding` 

转化过程主要做了两件事：

1.用户的`crudOptions`与公共配置、基础配置进行合并

2.字段配置分发，将`crudOptions.columns[key]`里面的`form`,`column`,`search`等配置分发到`table.columns`,`form.columns`,`search.columns`中去 



## 页面配置

一个`crud`主要分为3个部分：

- `crud.ts`： `crud`配置，每个`crud`最大的不同就在于此文件，数据管理和界面配置都存放在这个文件中
- `index.vue`： 页面组件，一般是较为固定的，只需修改名称即可，也可以在后续添加自定义的组件
- `api.ts`： 接口，可以用于实现添删改查请求接口

```js
const crudOptions ={
    request:{},     //http请求
    columns:{       //字段配置
        key:{       //字段key
            column:{},  //对应table-column配置，主要用于表格列的配置
            form:{},    //表单中该字段的公共配置，viewForm、addForm、editForm、search会集成此配置，支持对应ui的form-item配置
            viewForm:{}, //查看表单中该字段的配置，支持对应ui的form-item配置
            addForm:{}, // 添加表单中该字段的配置，支持对应ui的form-item配置
            editForm:{}, //编辑表单中该字段的配置，支持对应ui的form-item配置
            search:{}   //对应查询表单的form-item配置，search: { show: true } 表示显示查询
        }
    },
    search:{        //查询框配置 ，对应fs-search组件
        options:{}  //查询表单配置 ，对应el-from, a-form配置    
    },
    actionbar:{},   //动作条，添加按钮，对应fs-actionbar组件
    toolbar:{},     //工具条 ，对应fs-toolbar组件
    table:{         //表格配置，对应fs-table
        // 对应 el-table / a-table的配置
        slots:{}    // 对应el-table ,a-table的插槽
    },
    data:{},        //列表数据，无需配置，自动从pageRequest中获取 
    rowHandle:{},   //操作列配置，对应fs-row-handle
    form:{          //表单的公共配置,对应el-form，a-form配置
        wrapper:{}  //表单外部容器（对话框）的配置，对应el-dialog,el-drawer,a-model,a-drawer的配置
    },
    viewForm:{},    //查看表单的独立配置
    editForm:{},    //编辑表单的独立配置
    addForm:{},     //添加表单的独立配置
    pagination:{},  //分页配置 ，对应el-pagination / a-pagination
    container:{},   //容器配置 ，对应fs-container
}
```

在`.vue`界面中的具体内容解释：

```vue
<template>
    <fs-page>
        <fs-crud ref="crudRef" v-bind="crudBinding" customClass="crud-com-box crud-com-table"/>   <!--customClass用于相关样式的引入，可以引入多个-->
    </fs-page>
</template>

<script setup lang="ts">
    import { defineComponent, onMounted, ref } from "vue";
    import { useFs ,OnExposeContext, useCrud, useExpose } from "@fast-crud/fast-crud";
    import createCrudOptions from "./crud";  <!--引入对应的.tsx文件-->
    import {FirstRow} from "./crud";

    //此处为组件定义
    export default defineComponent({
        name: "FsCrudFirst",
        setup(props:any,ctx:any) {
            // // crud组件的ref
            // const crudRef = ref();
            // // crud 配置的ref
            // const crudBinding = ref();
            // // 暴露的方法
            // const {crudExpose} = useExpose({crudRef, crudBinding});
            // // 你的crud配置
            // const {crudOptions} = createCrudOptions({crudExpose});
            // // 初始化crud配置
            // const {resetCrudOptions , appendCrudBinding} = useCrud({crudExpose, crudOptions});

            //  =======以上为fs的初始化代码=========
            //  =======你可以简写为下面这一行========
            const context: any = {props,ctx}; // 自定义变量, 将会传递给createCrudOptions, 比如直接把props,和ctx直接传过去使用
            function onExpose(e:OnExposeContext){} //将在createOptions之前触发，可以获取到crudExpose,和context
            const { crudRef, crudBinding, crudExpose } = useFs<FirstRow>({ createCrudOptions, onExpose, context});
            
            // 页面打开后获取列表数据
            onMounted(async () => {
                crudExpose.doRefresh();
            });
            return {
                crudBinding,
                crudRef
            };
        }
    });
</script>

<style lang="scss" scoped>
.crud-com-box {
    margin-top: 8px;
    padding: 0 10px;
    border-radius: 8px 0 0 8px;
    box-sizing: border-box;
    background-color: #fff;
}

.crud-com-table {
    height: 100%;
}
</style>
```

在`api.ts`文件中的基本格式为：

```ts
import {AddReq, DelReq, EditReq, InfoReq, UserPageQuery} from "@fast-crud/fast-crud";
import {request} from "/@/utils/service";
export const apiPrefix = 'api/rbi_system/';

export function postFault(query: AddReq) {
    return request({
        url: apiPrefix + `fault/`,
        method: 'post',
        data: query,
    });
}
```

> 这段代码是一个函数 `postFault(query: AddReq)`，用于向服务器发送一个 `POST` 请求来添加故障数据
>
> - `export function postFault(query: AddReq) { ... }：`这行代码定义了一个导出的函数` postFault`，接受一个参数 `query`，类型为 `AddReq`。这个函数负责向服务器发送` POST `请求来添加故障数据。
>
> - `return request({ ... });`：在函数内部，通过调用` request` 函数发送` HTTP `请求
>
> - `url: apiPrefix + 'fault/'：`指定了请求的` URL `地址，包括了 `apiPrefix `变量和` 'fault/' `字符串，构成了完整的请求地址
>
> - `method: 'post'`：指定了请求的方法为` POST`，表示这个请求是用来提交数据给服务器端的，用于添加新的故障数据
>
> - `data: query`：将传入的` query `对象作为请求的数据发送到服务器端，这个对象包含了需要添加的故障数据的信息
>
> 综合来看，这段代码定义了一个函数` postFault(query: AddReq)`，通过向指定` URL `发送 `POST` 请求来添加故障数据，请求中携带了需要添加的数据信息。这样的函数可以方便在前端代码中调用，实现向服务器提交故障数据的操作

最后还需要在路由文件夹下添加路由程序，具体程序格式如下：

```ts
export const crudResources = [
    {
        title: "CRUD示例",
        name: "crud",
        path: "/crud",
        redirect: "/crud/basis",
        meta: {
            icon: "ion:apps-sharp"
        },
        children: [
            // 在此位置增加路由配置
            {
                title: "myFirstCrud",
                name: "myFirstCrud",
                path: "/test/myFirstCrud",
                component: "/test/myFirstCrud/index.vue"
            },
        ]
    }
]
```



## `index.vue`和`crud.tsx`之间的传值

#### `index.vue`传值给`crud.tsx`

`index.vue`中创建`ref`

```js
const props = defineProps({...})
const fooRef = ref(0)
const context: any = {
  fooRef //将fooRef 通过context传递给crud.tsx
  props,
};
```

`crud.ts`中使用`fooRef`

```js
export default function ({ crudExpose, context }: CreateCrudOptionsProps): CreateCrudOptionsRet {
  const {fooRef,props} = context  // 从context中获取fooRef
  return {
    crudOptions: {
      columns:{
        test:{
          title:'foo',
          valueChange(scope){
            // 使用或者修改 fooRef
            console.log(fooRef.value)
            fooRef.value = scope.value
          }
        }
      }
    }
  }
}
```

#### `crud.tsx`传值给`index.vue`

`crud.tsx` 中创建`ref`

```js
export default function ({ crudExpose, context }: CreateCrudOptionsProps): CreateCrudOptionsRet {
  const fooRef = ref(0)
  context.fooRef = fooRef //将fooRef 通过context传递给index.vue
  return {
    crudOptions: {
      ...
    }
  }
}
```

`index.vue`中使用`fooRef`

```js
const {crudRef, crudBinding, crudExpose, context} = useFs({createCrudOptions});
const {fooRef} = context //从context中获取fooRef
```

#### 直接将`crud.tsx`写在`index.vue`中

直接将`crud.tsx`写在`index.vue`中，就没有什么传值的问题了，直接正常使用`ref`即可

```js
const fooRef = ref(0)
function createCrudOptions({crudExpose}: CreateCrudOptionsProps): CreateCrudOptionsRet {
  return {
    crudOptions: {
      ...
      //使用或者修改 fooRef
    }
  }
}
```



## `columns`的配置

在`crudOptions`中，最重要的是`columns`的配置

```tsx
const crudOptions ={
    columns:{
        key:{                              // 字段的key
            title:'字段名',                 // 字段名称
            type:'dict-select',            // 字段类型
            dict: dict({url:'/dict/get'}), // 字典配置（如果组件需要）
            column:{ component:{} },       // 列配置
            form:{ component:{} },         // 表单字段公共配置
            // 以下是独立配置，会与上面的form配置合并。一般不需要配置
            addForm:{ component:{} },      // 添加表单字段独立配置
            viewForm:{ component:{} },     // 查看表单字段独立配置
            editForm:{ component:{} },     // 编辑表单字段独立配置
            search:{ component:{} },       // 查询表单字段独立配置
            valueBuilder(){},          // 值构建，具体请参考api/crud-options/columns.html文档
            valueResolve(){}               // 值解析
        }
    }
}
```



## `component`组件配置项

组件配置项`component`，一般用来对具体的组件进行具体的配置

```js
component:{   // 组件配置
  name: 'fs-dict-select', // 表单组件名称，支持任何v-model组件
  // name: shallowRef(SubTable),局部引用组件，见嵌套表格示例

  // v-model绑定属性名，element一般为'modelValue'（可以不传）
  // antdv一般为'value'，必须要传
  // 也可以是其他支持v-model属性名，比如a-checkbox的checked属性
  vModel: 'modelValue', 

  disabled: false, // 组件是否禁用
  readonly: false, // 组件是否是只读
  show: true, // 是否显示该组件
  on:{ // 组件事件监听
    onClick(context){console.log(context)} // 监听组件的事件
  },
  children:{ // 组件的插槽(仅支持jsx)
     default:(scope)=>{  // 默认插槽
        return (<div>{scope.data}</div>)
     },
     slotName:(scope)=>{  // 具名插槽
        return (<div>{scope.data}</div>)
     }
  },

  // html属性,会直接传递给dom
  style:{width:'100px'},
  class:{'mr-5':true},

  // 还可以在此处配置组件的参数，具体参数请查看对应的组件文档，不同组件参数不同
  separator:",",        // 这是fs-dict-select的参数
    
  // fs-dict-select内部封装了el-select组件，所以此处还可以配置el-select的参数
  // 如果ui用的是antdv，则支持a-select的参数
  filterable: true,     // 可过滤选择项,
  multiple: true,       // 支持多选
  clearable: true,      // 可清除
    
  // 如果组件的参数与上面的参数有冲突，则需要配置在props下。
  props:{
    // 比如name、vModel、props、on、children等等
    // 如果你要用的组件里面需要配置以上这些名字的参数的话，可以配置在此处
  }

}
```



## 字段类型

字段类型背后代表了一段默认配置，当你配置了`type`时，你可以省略它代表的这部分默认配置

当没有字段类型时，你需要写如下这一大段字段配置，所以字段类型是非常有必要的

|     类型      |     描述     |
| :-----------: | :----------: |
|   `number`    | 数字类型字段 |
|    `text`     | 文本类型字段 |
| `dict-select` | 选择类型字段 |
| `dict-radio`  | 单选类型字段 |

更多的字段类型查看官方文档：[字段类型](http://fast-crud.docmirror.cn/api/types.html)

`crud`字典内容格式：

```ts
dict: dict({
    getData: async () => {
        let faultModeList = await getFaultModeList({ device_category_second__contains: context.propsRef.value.level2Category });
        let data_list = [];
        for (let item of faultModeList.data) {
            data_list.push({ label: item.description, value: item.id });
        }
        return data_list;
    },
}),
```

对于选择类型的字段，需要对`dist`进行配置，一般本地配置如下所示：

```js
dict: { 
        data:[  // dict-select字段类型需要配置数据字典
          {value:'sz',label:'深圳'},
          {value:'bj',label:'北京'} 
        ] 
      } 
```

`dist`的内容也可以进行远程调用去获取数值

在单个配置里面你也可以通过配置`dict.getData`方法来覆盖这个默认的请求方法

```js
dict:dict({
    // 本dict将会走此方法来获取远程字典数据
    async getData(dict,context){
        return request(dict.url)
    }   
})
```



## 动态计算

动态计算主要用于解决配置需要动态变化的问题是`fs-crud`最重要的特性之一

配置动态变化可以有如下4种方式实现：

1. 直接修改`crudBinding`对应的属性值。比如`crudBinding.value.search.show=false`即可隐藏查询框。
2. 给属性配置`ref、computed`类型的值，通过修改`ref.value`，就能够动态变化。
3. `compute` 同步计算，类似`computed`，不同的是它可以根据上下文进行动态计算。
4. `asyncCompute`异步计算，基于`watch 和 computed`实现,与`compute`相比,它支持异步计算。

`compute`同步计算常见的需求：

一个用户表，有个用户类型字段`userType`，可能的值为：`公司`或`个人`。 我们要实现，当选择`公司`时，需要额外`上传营业执照`、`填写信用代码`的功能。 就需要在`userType`字段选中`公司`的时候，将`上传营业执照`和`信用代码`的输入框显示出来。选择`个人`时则不显示。

```js
import {useCompute} from '@fast-crud/fast-crud'
const {compute} = useCompute()
const crudOptions = {
    columns:{
        userType:{
            title: '用户类型',
            type: 'dict-select',
            dict: dict({data:[
                    {value:1,label:'个人'},
                    {value:2,label:'公司'}
                ]})
        },
        businessLicenceImg :{
            title: '营业执照上传',
            type: 'avatar-uploader',
            form:{
                show:compute((context)=>{
                    // 通过动态计算show属性的值，当前表单内的userType值为公司，才显示此字段
                    return context.form.userType ===2
                })
            }
        },
        businessLicenceCode :{
            title: '营业执照号码',
            type: 'text',
            form:{
                show:compute((context)=>{
                    // 通过动态计算show属性的值，当前表单内的userType值为公司，才显示此字段
                    return context.form.userType ===2
                })
            }
        }
    }
}
```

用户类型（`userType`）是一个下拉选择框，当用户选择不同的值时会改变`form.userType`的值，同时会触发`businessLicenceImg`和`businessLicenceCode`这两个字段中`form.show`的重新计算。 从而让营业执照上传和营业执照号码根据`userType`的值不同而显隐。

`asyncCompute`异步计算：

当我们要计算的值需要从网络请求或者从其他地方异步获取时可以使用此方法配置

- 方法：`asyncCompute({watch?,asyncFn})`
- 参数`watch`：`Function(context)` ，可为空，监听一个值，当这个返回值有变化时，触发`asyncFn`。不传则`asyncFn`只会触发一次
- 参数`asyncFn`：`asyncFn:Function(watchValue, context) `,异步获取值

```js
import {compute} from '@fast-crud/fast-crud'
const crudOptions = {
    columns:{
        grade:{
            title: '年级',
            type: 'text',
            form: {
                component:{
                    name:'a-select',
                    // 配置异步获取选择框的options
                    options: asyncCompute({
                        // 没有配置watch，只会触发一次
                        asyncFn: async ()=>{
                            // 异步获取年级列表，这里返回的值将会赋给a-select的options
                            return request({url:"/getGradeList"})
                        }
                    })
                }
            }
        },
        class :{
            title: '班级',
            type: 'avatar-uploader',
            form:{
                component:{
                    name:'a-select',
                    // 配置异步获取选择框的options
                    options: asyncCompute({
                        // 监听form.grade的值
                        watch((context)=>{
                            return context.form.grade
                        }),
                        // 当watch的值有变化时，触发asyncFn,获取班级列表
                        asyncFn: async (watchValue,context)=>{
                            // 这里返回的值 将会赋值给a-select的options
                            return request({"/getClassList?grade=" + watchValue})
                        }
                    })
                }
            }
        },
    }
}
```

只有在需要用到行数据(`row`)、表单数据(`form`)参与动态计算的地方，才使用`compute`和`asyncCompute`，其他时候使用`ref`或`computed`



## 案例解决方案积累

### 给弹出框添加滚动条

```tsx
editForm: {
    wrapper: {
      style: {
        maxHeight: '80vh',
        minWidth: '50vw',
        overflowY: 'auto',
        scrollbarColor: 'rgb(20, 20, 20) #000',
        scrollbarWidth: 'thin',
      }
    },
},
```

