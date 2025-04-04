# `VeeValidate`

## 基本概念

对于一些`UI`库，是提供一些表单验证的规则。但是，如果我们不使用`UI`库，我们使用原生的方式来构建我们的表单，我们如何做到表单验证呢？我们可以通过一些库，来辅助我们完成表单的验证，其中`vee-validate`就是一个常用的表单规则验证的库



## 安装配置

安装命令：`npm install --save vee-validate @vee-validate/rules yup`

- `vee-validate`是表单验证的核心库
- `vee-validate/rules`是验证规则
- `yup`支持定义链式声明的验证规则



## 基本使用

```vue
<template>
    <Form @submit="onSubmit">
        <Field name="account" v-model="account" :rules="emailRule" 
           :validate-on-input="true" />
        <ErrorMessage name="account" />
        <hr />
        <button>提交表单</button>
    </Form>
</template>

<script setup lang="ts">
import { Form, Field, ErrorMessage } from 'vee-validate';  // 导入相关包
import { ref } from 'vue';

const account = ref('');
// 定义表单规则
const emailRule = (value: any) => {
    // 通过@符号，判断是否是邮箱
    return /@/.test(value) ? true : '邮箱格式错误';
}
// 表单提交
const onSubmit = (values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
}
</script>

<style lang="scss">
div {
    @apply flex w-screen h-screen justify-center items-center;
    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    }
}
</style>
```

> `vee-validate`包是通过主键来进行操作，其中`Form, Field, ErrorMessage`等都是包的主键（主键就是可以作为标签进行使用，和`element-UI`类似）
>
> - `Form`主键：表单的顶级主键（整个表单主键），必须要有
> - `Field`：每个具体字段的主键
>   - 为主键设置名称`<Field name="account" />`
>   - `:rules="emailRule"`表示为主键设置对应的规则
>   - 该字段输入框是在失去焦点的时候触发验证规则，我们可以进行修改为输入框发生改变的时候进行校验规则的触发：`:validate-on-input="true"`
> - `ErrorMessage`主键：用于显示错误消息的主键，根据`name`进行具体表单的匹配，为这个具体的表单显示对应的错误消息



## 配合插槽使用原生表单

主键都有插槽，`vee-validate`包中的主键也不例外

如果我们想要使用原生表单的形式去设置我们的表单，我们可以使用插槽的形式进行优化：

```vue
<template>
    <Form @submit="onSubmit">
        <Field name="account" v-model="account" :rules="emailRule" 
            :validate-on-input="true"
            #default="{ field, errorMessage }">
            <input v-bind="field" v-model="account" />
            <hr />
            <p>{{ errorMessage }}</p>
        </Field>
        <hr />
        <button>提交表单</button>
    </Form>
</template>

<script setup lang="ts">
import { Form, Field } from 'vee-validate';
import { ref } from 'vue';

const account = ref('');
// 定义表单规则
const emailRule = (value: any) => {
    // 通过@符号，判断是否是邮箱
    return /@/.test(value) ? true : '邮箱格式错误';
}
// 表单提交
const onSubmit = (values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
}
</script>

<style lang="scss">
div {
    @apply flex w-screen h-screen justify-center items-center;

    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    }
}
</style>
```

> `#default="{ field, errorMessage }"`：在插槽中接收到数据（表单数据和错误消息数据），将`field`数据绑定到原生的表单`<input>`中



我们可以使用系统提供的验证规则进行规则验证，`vee-validate/rules`包提供了大量系统编写好的验证规则，如是否唯一，最小数量和最大数量等等，用的时候需要进行导入：

`import { required, min, max, confirmed, email } from '@vee-validate/rules';`

```vue
<template>
    <Form @submit="onSubmit">
        <Field name="account" v-model="account" 
            :rules="{ email: true, required: true }" 
            :validate-on-input="true"
            #default="{ field, errorMessage }">
            <input v-bind="field" v-model="account" />
            <hr />
            <p>{{ errorMessage }}</p>
        </Field>
        <hr />
        <button>提交表单</button>
    </Form>
</template>

<script setup lang="ts">
import { Form, Field, defineRule } from 'vee-validate';
import { required, email } from '@vee-validate/rules';
import { ref } from 'vue';

const account = ref('');
// 声明系统提供的验证规则
defineRule('email', email);
defineRule('required', required);
// 表单提交
const onSubmit = (values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
}
</script>

<style lang="scss">
div {
    @apply flex w-screen h-screen justify-center items-center;

    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    }
}
</style>
```

上述的验证规则的提示信息是英文的，如果我们需要中文的验证规则提示，我们需要安装一个多语言包

`npm install @vee-validate/i18n`

使用的时候先导入这个包和中文字典：

```vue
<template>
    <Form @submit="onSubmit">
        <!-- <Field name="account" v-model="account" :rules="emailRule" :validate-on-input="true" />
        <ErrorMessage name="account" /> -->
        <Field name="account" label="账号" v-model="account" 
            :rules="{ email: true, required: true}" :validate-on-input="true"
            #default="{ field, errorMessage }">
            <input v-bind="field" v-model="account" />
            <hr />
            <p>{{ errorMessage }}</p>
        </Field>
        <hr />
        <button>提交表单</button>
    </Form>
</template>

<script setup lang="ts">
import { Form, Field, defineRule, configure } from 'vee-validate';
import { required, email } from '@vee-validate/rules';
import { localize } from '@vee-validate/i18n';
import zh_CN from '@vee-validate/i18n/dist/locale/zh_CN.json';
import { ref } from 'vue';

const account = ref('');
// 声明系统提供的验证规则
defineRule('email', email);
defineRule('required', required);
// 配置中文
configure({
    generateMessage: localize('zh_CN', zh_CN)
})

// 表单提交
const onSubmit = (values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
}

</script>

<style lang="scss">
div {
    @apply flex w-screen h-screen justify-center items-center;

    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    }
}
</style>
```

> `label="账号"`表示为`name="account"`设置中文别名，使在出现错误信息时可以将`account`用账号中文代替



## 使用`Api`操作验证表单

使用`Api`操作验证表单，我们就可以不需要使用`VeeValidate`提供的主键进行操作，我们可以使用编程的方式进行验证，我们需要使用`useField`函数进行验证的设置

```vue
<template>
    <form @submit="onSubmit">
        <input type="text" v-model="usernameValue" />
        <p>{{ errors.username }}</p>
        <button>表单提交</button>
    </form>
</template>

<script setup lang="ts">
import { configure, defineRule, useField, useForm } from 'vee-validate';
import { required, email } from '@vee-validate/rules';
import { localize } from '@vee-validate/i18n';
import zh_CN from '@vee-validate/i18n/dist/locale/zh_CN.json';

// 声明系统提供的验证规则
defineRule('email', email);
defineRule('required', required);

// 中文语言配置
configure({
    generateMessage: localize('zh_CN', zh_CN)
})

// 定义验证函数中间件拦截，处理验证操作，防止表单为空点击提交可以进行内容的提交操作
const { handleSubmit, errors } = useForm();
// 定义验证，得到数据
const { value: usernameValue } = useField('username', { required: true, email: true }, { label: '用户名' });

// 表单提交
const onSubmit = handleSubmit((values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
})
</script>

<style lang="scss" scoped>
div {
    @apply flex w-screen h-screen justify-center items-center;

    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    }
}
</style>
```

一个一个的定义验证规则比较麻烦，我们通常都是在`const { handleSubmit, errors } = useForm();`中进行集中统一的定义，具体完整代码为：

```vue
<template>
    <form @submit="onSubmit">
        <section>
            <input type="text" v-model="usernameValue" />
            <p class="error" v-if="errors.username">{{ errors.username }}</p>
        </section>
        <section>
            <input type="text" v-model="passwordValue" />
            <p class="error" v-if="errors.password">{{ errors.password }}</p>
        </section>
        <button>表单提交</button>
    </form>
</template>

<script setup lang="ts">
import { configure, defineRule, useField, useForm } from 'vee-validate';
import { required, email } from '@vee-validate/rules';
import { localize } from '@vee-validate/i18n';
import zh_CN from '@vee-validate/i18n/dist/locale/zh_CN.json';

// 声明系统提供的验证规则
defineRule('email', email);
defineRule('required', required);

// 中文语言配置
configure({
    generateMessage: localize('zh_CN', zh_CN)
})

// 定义验证函数中间件拦截，处理验证操作，防止表单为空点击提交可以进行内容的提交操作
const { handleSubmit, errors } = useForm({
    // 在这里进行统一的初始值和验证数据结构的定义
    // 定义字段的初始值
    initialValues: {
        username: 'jlc',
        password: ''
    },
    // 定义验证数据结构
    validationSchema: {
        username: { required: true, email: true },
        password: { required: true }
    }
});
// 定义验证，得到数据
const { value: usernameValue } = useField('username', {}, { label: '用户名' });
const { value: passwordValue } = useField('password', {}, { label: '密码' });

// 表单提交
const onSubmit = handleSubmit((values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
})
</script>

<style lang="scss" scoped>
div {
    @apply flex w-screen h-screen justify-center items-center;

    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    };

    .error {
        @apply bg-red-600 border border-gray-800 text-white;
    }
}
</style>
```



## `yup`的基本使用

`yup`包提供了更加丰富的验证规则，可以进行链式的操作，更加的灵活：

```ts
<template>
    <form @submit="onSubmit">
        <section>
            <input type="text" v-model="usernameValue" />
            <p class="error" v-if="errors.username">{{ errors.username }}</p>
        </section>
        <section>
            <input type="text" v-model="passwordValue" />
            <p class="error" v-if="errors.password">{{ errors.password }}</p>
        </section>
        <button>表单提交</button>
    </form>
</template>

<script setup lang="ts">
import { configure, defineRule, useField, useForm } from 'vee-validate';
import { required, email } from '@vee-validate/rules';
import { localize } from '@vee-validate/i18n';
import zh_CN from '@vee-validate/i18n/dist/locale/zh_CN.json';
import * as yup from 'yup';

// 声明系统提供的验证规则
defineRule('email', email);
defineRule('required', required);

// 中文语言配置
configure({
    generateMessage: localize('zh_CN', zh_CN)
})

// 定义验证函数中间件拦截，处理验证操作，防止表单为空点击提交可以进行内容的提交操作
const { handleSubmit, errors } = useForm({
    // 在这里进行统一的初始值和验证数据结构的定义
    // 定义字段的初始值
    initialValues: {
        username: 'jlc',
        password: ''
    },
    // 定义验证数据结构
    validationSchema: {
        // 使用yup对用户名进行验证，验证内容为字符串，且必须输入，且格式为邮箱，同时可以自定义错误信息
        username: yup.string().required('用户名不能为空').email('邮箱格式错误'),   
        password: { required: true }
    }
});
// 定义验证，得到数据
const { value: usernameValue } = useField('username', {}, { label: '用户名' });
const { value: passwordValue } = useField('password', {}, { label: '密码' });

// 表单提交
const onSubmit = handleSubmit((values: any) => {
    // values是表单项数据
    console.log(values);
    alert('验证通过')
})
</script>

<style lang="scss" scoped>
div {
    @apply flex w-screen h-screen justify-center items-center;

    input {
        @apply border-2 p-2 rounded-md border-violet-950 outline-none;
    };

    .error {
        @apply bg-red-600 border border-gray-800 text-white;
    }
}
</style>
```



## 封装验证规则

