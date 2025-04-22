# 开发积累-`RBM`项目

## 基本概念

`RBM`项目是重大设备风险监测管理项目，前端主要涉及到`Vue`、`Gojs`和`FastCrud`等技术框架，后端主要涉及到`Django`、`ThingsBoard`等技术框架，在开发`RBM`项目中，我主要参与的是前端的部分（包括数据图表的前端展示，`Gojs`界面的搭建和可视化实时数据的呈现，和数据大屏的制作等），同时也涉及了一小部分的后端接口的编写，在开发中遇到的一些问题，依次在本文档中进行记录。



## 下拉选项禁用设置

下拉选项框当没有子层内容时，这个选项框设置为不能被选中（禁用）：

```vue
<template>
  <el-cascader
    v-model="item.methods"
    :options="confirmDialog.data?.methods"
    :props="cascaderProps"
    placeholder="请选择"
    style="width: 200px;"
  />
</template>

<script setup>
import { reactive } from 'vue';

const item = reactive({
  methods: [],
});

// 数据类型介绍
const confirmDialog = reactive({
  data: {
    methods: [
      {
        value: '1',
        label: '选项1',
        children: [], // 空数组，应该禁用
      },
      {
        value: '2',
        label: '选项2',
        children: [
          {
            value: '2-1',
            label: '子选项2-1',
          },
        ],
      },
    ],
  },
});

// 下拉选项框当没有子层内容时，这个选项框设置为不能被选中
const cascaderProps = {
  //允许选择任意一级，最外层也可被选中
  //checkStrictly: true,
  disabled: (data) => {
    return data.children && data.children.length === 0;
  },
};
</script>
```

实现效果：（如果当前的选项没有子内容，这个父级的选项就被禁用，变成灰色，鼠标放上去显示无法点击）

![image-20241114205710210](D:\Myproject\develop-study-notes\images\image-20241114205710210.png)



## 页面路由跳转

页面的跳转有两种情况，在当前的页面上进行跳转；打开一个新的页面进行跳转

在当前页面的基础上进行跳转：通过动态路由`router`进行跳转

```js
import { useRouter } from 'vue-router'
const router = useRouter()
function goToPage(data) {
    router.push(`/pidPreview/?id=${data.id}`)
}
```

新开一个页面，在新开的页面上进行跳转：也是通过动态路由进行跳转，但是使用`window.open`新开一个窗口：

```js
import { useRouter } from 'vue-router'
const router = useRouter()
function goToPage(data) {
    const routeUrl = router.resolve({
        path: `/pidPreview/`,
        query: { id: data.id }
    });
    window.open(routeUrl.href, '_blank');
}
```

> `window.open(routeUrl.href, '_blank');` 是 `JavaScript` 中用于在新窗口或新标签页中打开指定` URL` 的代码，具体介绍：
>
> - `window.open`: 是` JavaScript` 的一个内置函数，用于打开一个新的浏览器窗口或标签页，它可以接受三个参数：
>
>   - `URL`: 要打开的页面的 `URL`
>
>   - `target`: 指定新窗口的名称或目标，常见的值包括：
>
>     -  `_blank`（在新窗口或标签页中打开）
>
>     -  `_self`（在当前窗口中打开）
>     -  `_parent`（在父框架中打开）
>     -  `_top`（在顶层框架中打开）
>
>   - `windowFeatures`（可选参数）: 可以指定新窗口的特性，如大小、位置等。这个参数在现代浏览器中通常被忽略，因为浏览器可能会阻止弹出窗口



## 样式不生效问题

### `span`标签样式不生效

对于`<span>`标签，要想对其内容样式的修改，特别是内外边距的添加，需要在标签外部包裹一层`<div>`标签，在该标签中添加相关的样式，这样才能正常生效

***

### 非常规添加样式

对于非常规的样式添加（一般是传统的样式添加格式无法生效），打开`F12`，选择该部分的主体，在该级进行新建样式规则

调整好样式后，将添加的`css`代码放入到代码中，并且用`:deep`进行包裹

```ts
:deep(span.el-radio-button__inner) {
    width: 100%;
}
```

这样就能在界面上生效，强制的提高了样式的优先级（非不得已，不推荐使用）



## 工厂和工段相关

### 数据无法提交到数据库

在工段数据提交时，遇到了提交到数据库`lkt_factory_section`的数据都显示`Null`的情况，经过排查，发现接口设置错误，将返回值的`data`写成了`params`

```ts
/**
 * @description 新增工段信息
 * @param query
 */
export function postSectionList(query: AddReq) {
  return request({
    url: apiPrefix + `factory_section/`,
    method: 'post',
    data: query,
  });
}
```

修改完之后，新增的工段数据就可以正常的提交了

***

### 工厂名称无法在下拉选择列表中获取

在新增工段界面中，需要对新增工段的工厂进行选择，所以要及时的获取工厂`id`和`name`，但是在调用数据时遇到了问题：调用到的数据不能正常的在下拉选择框中显示，于是对代码重新进行编写：

```ts
interface factoryType {
  id: string,
  name: string,
}

const factoryNames = ref<factoryType[]>([])

await getFactoryList({}).then((res:any)=>{
  factoryNames.value = res.data
});
```

修改完之后，就可以正常的在相应组件中进行数据的填入：

```vue
<el-form-item label="选择工厂：">
  <el-select v-model="newSectionForm.factorySelection" placeholder="选择需创建工段的工厂" style="width: 195px">
    <el-option v-for="item in factoryNames" :key="item.id" :label="item.name" :value="item.id"/>
   </el-select>
</el-form-item>
```

***

### 下拉列表框内容什么时候刷新问题

在一开始，设置下拉列表框内容的渲染是在整个页面刷新时进行渲染，后面考虑到过早的渲染是没有必要的，只需在新建按钮点击时，进行下拉列表框内容的渲染即可，所以，将代码进行了一些调整，将获取工厂`id`和`name`的代码放到新建表单出现前调用：

```ts
async function newTable() {
  await getFactoryList({}).then((res:any)=>{
    factoryNames.value = res.data
  });
  createForm.value.visible = true
}
```

***

### 联动选项显示弹出对话框的标题

在新建弹出对话框中，当按钮在工厂端，对话框的标题显示新建工厂；当按钮在工段端，对话框的标题显示新建工段

```vue
<el-dialog class="h-[400px]" v-model="createForm.visible" :title="dialogTitle">
    <el-switch v-model="showForm" active-text="工段" inactive-text="工厂" />
    <div v-if="showForm" style="padding-top: 15px;">
        ...
    </div>
    <div v-else style="padding-top: 15px;">
        ...
    </div>

<script setup lang="ts">
const showForm = ref(false)

const dialogTitle = computed(() => {
  return showForm.value ? '新建工段' : '新建工厂';
});
</script>
```

***

### 添加工厂/工段后触发刷新

在创建完工厂和工段后，需要手动刷新才能使新工厂或新工段在设备档案中心界面显示，考虑如何实现新增完工厂/工段实现自动刷新，使新工厂/工段及时显示在树型控件中，通过自定义事件`emits`使子组件向父组件传递自定义事件，实现刷新



## 风险等级相关

### 父子控件如何进行数据双向绑定

在父控件中获取了数据库`lkt_factory_setting`中的数据后，需要在子控件的图表中进行填充，所以就需要进行父子控件的传值

父控件传值部分代码：

```vue
<template>
	<div class="w-1/3">
        <safeMap :chartDatas="chartData"/>
    </div>
</template>

<script setup lang="ts">
    const chartData = ref<any[]>([]) // chartData为调用数据库后处理的数据
</script>
```

子控件传值部分代码：

```vue
<script setup lang="ts">
interface Data {
    chartDatas: number[][],
}

const props = defineProps<Data>()
// 调用后就可以在子控件中使用父控件调用的数据库数据了
props.chartDatas // 数据
</script>
```

***

### 子控件调用父控件的数据无法在图表中进行显示

在从数据库`lkt_factory_setting`获取数据后，需要进行数据的父子调用，在子控件调用到父控件的数据后，子控件中的数据无法填充到图表中，在经过排查后，发现在父控件中的被调用数据仅仅是数组对象的声明，没有对其进行响应式绑定：

```ts
const chartData = ref<any[]>([])
```

在对图表数据`chartData`进行响应式绑定后，就能正常的进行图表数据的填充

***

### 点击启用编辑按钮前保证不能编辑图表内容

在原先的界面中，在没有点击启用编辑按钮前，点击图表内容，其数据是可编辑的，为了解决这个问题，需要设置一个`flag`标志来控制其可编辑状态：

在父控件进行传值绑定，来传递其编辑状态：

```vue
<template>
	<div class="w-1/3">
        <safeMap :chartDatas="chartData" :flag="editable"/>
    </div>
</template>
```

在子控件的编辑函数中引入可编辑标志来控制图表是否可编辑：

```ts
interface Data {
    chartDatas: number[][],
    flag: boolean
}

const props = defineProps<Data>()

function handleClick(params: any) {
  if(props.flag){
    params.data[2] = (params.data[2] + 1) % 5
    option.value.series[0].data[params.dataIndex] = params.data
    chartEl.value.setOption(option.value)
  }
}
```



## `FastCrud`表格相关

### 在`FastCrud`配置本地数据时出现的数据无法导入到表格中

一定要加上`total`参数，否则无法实现数据传递到表格中，两种格式的输入都可以

```ts
const pageRequest = async (query: UserPageQuery) => {
    return {total:2,
            data:[
                {"classification":'腐蚀减弱', "damageMode":'大气腐蚀（有隔热层）'},
                {"classification":'腐蚀减弱', "damageMode":'盐酸腐蚀'},
                {classification:'腐蚀减弱', damageMode:'大气腐蚀（有隔热层）'},
                {classification:'腐蚀减弱', damageMode:'二氧化碳腐蚀'},
            ],
            page:1,
            limit:20}
    // return await getFaultMechanismList(query);
};
```

***

### 表格页面触发一次刷新

在`.tsx`文件中如果按钮点击事件需要触发一次表格数据刷新，可以通过以下的代码进行触发：`rudExpose.doRefresh();`

该方法暴露在其`.vue`父文件中，在触发页面时自动刷新

```ts
onMounted(() => {
    crudExpose.doRefresh();
})
```

***

### 自定义一个按钮事件

```ts
alarmAcknowledgement: {
    icon: 'Select',  //按钮的图标
    text: '告警确认',  //按钮的名字
    order:1,  //按钮的先后顺序
    style: { backgroundColor: '#409eff', color: 'white' },  //按钮的样式设置
    click: compute(({row}) => {   //按钮的自定义点击事件
        return async () => {
            await postFeedbackAlarm({ alarmId: row.id, feedback: '告警确认' });
            crudExpose.doRefresh();  //触发一次界面刷新
        };
    }),
    disabled: compute(({ row }) => {  //按钮禁用触发事件
        return row.feedback !== null;
    }),
},
```

***

### 多行标签展示（防止显示不正常）

```tsx
'activities.ids': { //数据支持多级结构 row={key:xx,activities:{ids:xxx}}
    title: '维护活动',
    type: 'dict-select',
    search: { show: false },
    column: { show: true, sortable: false, minWidth: 150},
    addForm: { show: true },
    editForm: { show: true },
    form: {
        component: {
            props: {
                multiple: true  //设置支持列表内容多选
            }
        }
    },
    dict: dict({  //获取列表数据，最好用以下的方法
        value: 'id',
        label: 'active',
        cache: true,
        getData: async () => {
            return await getRepairMaintenanceWorkList({ limit: 0 }).then((res: any) => {
                return res.data;
            });
        },
    }),
},
```

***

### 对于创建时间的条目创建和时间筛选

字段创建：

```tsx
create_datetime: {
    title: '创建时间',
    type: 'datetime',
    search: { show: true, col: { span: 6 }, component: { type: 'datetimerange', shortcuts: shortcuts } },
    column: { show: true, sortable: true, minWidth: 150 },
    addForm: { show: false },
    editForm: { show: false },
},
```

筛选时间代码：在`export const createCrudOptions`中添加

```tsx
if (query.create_datetime) {
    newQuery.create_datetime__gt = query.create_datetime[0]
    newQuery.create_datetime__lt = query.create_datetime[1]
    delete query.create_datetime
}
```

***

### 表单操作界面插槽设计

在`.tsx`对应的`.vue`文件中进行进行插槽的添加

```vue
<fs-crud ref="crudRef" v-bind="crudBinding" customClass="crud-com-box crud-com-table">
    <template #form-body-bottom>
		想要插入的内容
    </template>
</fs-crud>
```

上式为在表单操作界面的底部进行插槽的插入

其他组件插槽的添加：去`element-Plus`组件库中找对应的组件所提供的插槽样式，再进行插槽的添加

如`input`输入框的插槽`API`：

```vue
<el-input>
    <template #prepend>
		{{ "维护人" }}
    </template>
</el-input>
```

***

### 表单操作界面获取表格内的数值

通过表单事件`wrapper`方法获取表单数据，如点击详情操作触发数据获取：对话框打开完成事件处理方法

```ts
viewForm: {
    wrapper: {
        onOpened: ({form}) => {
            context.maintenanceInfo.value = form.personnel_times
        },
    }
},
```

通过`context`将获取到表单的数据传递给外部`.vue`文件使用

***

### 重新加载某字段的字典

调用其`reloadDict`方法重新加载字典

```ts
editForm: {
    wrapper: {
        onOpened: ({form}) => {
            crudExpose.getFormComponentRef('fault').reloadDict();
        },
    }
},
```

上述为重新加载`fault`字段的字典

***

### 去除操作对话框的底部按钮

```ts
viewForm: {
    wrapper: {
        buttons: {
            cancel: {show: false}, // 取消按钮
            reset: {show: false},  // 重置按钮
            ok: {show: false},     // 确认按钮
        }
    }
},
```

***

### 表单某些字段内容提交默认的数据

```ts
const addRequest = async ({ form }: AddReq) => {
    form.status = 'maintenance_handle_status_2'
    return await postMaintenance(form);
};
```

上述为将`status`字段进行默认数据的提交

***

### 表单某字段显示默认值

点击添加表单数据时，使其中的设备字段默认显示内容：

```ts
addForm: {
    labelPosition: 'top',
    wrapper: {
        onOpened: async ({ form }) => {
            form.device = context.props?.deviceId; // 将外界的设备id传入进行设备name的选择
        },
    },
},
    
// 设备字段是根据其id选择对应的设备name
device: {
    title: '设备',
    type: 'dict-select',
    search: { show: false },
    column: {
    show: false,
    sortable: false,
    minWidth: 150,
    formatter({ value, row, index }: { value: any; row: any; index: any }) {
        return value?.name || null;
    },
},
    addForm: { show: true, rules: [{ required: true, message: '设备字段不能为空', trigger: 'blur' }] },
    editForm: { show: true },
    dict: dict({
        label: 'name',
        value: 'id',
        getData: async () => {
            let res = await getDeviceList({
                limit: 1,
                id: context.props?.deviceId,
            });
            return res.data;
        },
    }),
    form: {
        valueChange({ form, value, getComponentRef }: { form: any; value: any; getComponentRef: any }) {
            form.mode = '';
            getComponentRef('fault').reloadDict();
        },
},
},
```

***

### 函数延迟运行

```ts
addForm: {
    labelPosition: 'top',
    wrapper: {
        onOpened: async ({ form }) => {
            context.maintenanceInfo.value = [];
            form.device = context.props?.deviceId;  // 运行速度比getFormComponentRef慢
            // 先有设备在去匹配其故障记录
            setTimeout(()=>{
                crudExpose.getFormComponentRef('fault').reloadDict();  // 运行速度快，需要延时
            })
        },
    },
},
```

***

### 点击操作按钮后改变按钮的样式

提醒用户哪个按钮被点击了

```ts
const selectItem = ref('');

export const createCrudOptions = function ({ crudExpose, context }: CreateCrudOptionsProps): CreateCrudOptionsRet {
  const pageRequest = async (query: UserPageQuery) => {
    return await getDamageModeList(query).then((res: any) => {
      // console.log(res);
      context.modeInfo(res.data[0]);
      selectItem.value = res.data[0].id;
      return res;
    });
  };
}

view: {
    icon: 'Search',
    order: 1,
    // style: { color: '#409eff' },
    type: compute(({ row }) => {
        return row.id === selectItem.value ? 'primary' : 'default';
    }),
    click: async ({ row }) => {
        context.modeInfo(row);
        selectItem.value = row.id;
    },
},
```



## 按钮相关

### 自定义表格删除行和添加行

删除行按钮绑定操作：

```vue
<el-button type="danger" @click="maintenanceInfo.splice(maintenanceInfo.indexOf(item), 1)">删除</el-button>
```

添加行按钮绑定操作：

```vue
<el-button class="ml-2" size="small" type="primary" @click="maintenanceInfo.push({ ...medium })">添加</el-button>
```

```ts
const maintenanceInfo = ref<any>([]) // 是一个响应式的列表数据
```

***

### 插槽` #default="scope"`

在`Vue 3`中，`#default="scope"`是一种插槽（`Slot`）的使用方式。它用于接收具名插槽的默认内容，并将其赋值给一个名为`scope`的变量

这种方式可以让我们更灵活地使用插槽，并在父组件和子组件之间进行数据传递和操作

***

### 编辑、保存和取消保存按钮

一开始只显示一个编辑按钮，点击了编辑按钮后显示保存和取消保存按钮

```vue
<el-button type="primary" @click="editable = !editable" v-if="editable">取消保存</el-button>
<el-button type="primary" @click="modeSave">{{ editable ? '保存' : '编辑' }}</el-button>
```



## 前后端接口交互

根据`id`获取值接口编写：

```ts
export function getDeviceCategoryById(query: InfoReq) {
    return request({
        url: apiPrefix + `device_category/${query.id}/`,
        method: 'get',
    });
}
```

接口调用方法；

```ts
const equipmentType = ref()

async function equipmentChange() {
  equipmentType.value = await getDeviceCategoryById({ id: props.deviceInfo?.category_first }).then((res: any) => {
   return res.data[0].name;
})
}

console.log(equipmentType.value)  // equipmentType.value就是根据id获取到的name值
// 如果想要监听式的获取id对应的name值，需要通过watch监听器进行设置
watch(() => props.deviceInfo, (nVal) => {
  equipmentChange()
}, {deep:true, immediate: true })
```

