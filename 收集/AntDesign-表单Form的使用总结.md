> 表单组件是管理系统中使用非常频繁的一个组件
>
> 对AntDesign表单的使用做一个总结

![image-20220523152554783](http://rb3i5hs61.hn-bkt.clouddn.com/image-20220523152554783.png)

认识表单：

上图是表单最基本的一种形式。对于图中Username这样的输入形式而言，将Username称为`lable`(标签)，紧跟其后的是它的内容区。一个标签加上一个内容区就形成了表单的一个项`form-item`。（这描述的只是输入型的表单项，并且输入型的表单项可以没有`label`。还有其他类型的表单项，比如单选多选等等。）

在AntDesign组件中，众多的表单项`<a-form-item>`按顺序排放在`<a-form>`这样一个标签中。注意，如上图Submit这样一个按钮，也是一个表单项。在`<a-form-item>`标签中写入你需要的内容，如`<a-input>`,`<a-checkbox>`等等。



基本使用：

给`a-form`标签中`model`绑定上一个对象，该对象的内容对应着你需要使用表单所提交的内容

`@finish`提交表单且数据验证成功后回调事件，回调参数`function(values)`

`@fisishFailed`提交表单且数据验证失败后回调事件，回调参数`function({ values, errorFields, outOfDate })`



在表单提交之前还需要注意的是，每个表单项的内容需要双向绑定到预先定义好的表单对象中。



提交表单的方式：

第一种：在`<a-form>`内的最后一个`<a-form-item>`内定义上一个按钮`<a-button>`。并且将该按钮标签的`html-type`属性赋值`'submit'`，意为设置 `button` 原生的 `type` 值。`<a-button>`是不设置点击触发事件，当点击这个按钮时就触发了提交表单事件。根据一些验证规则（如何定义验证规则看下文）的验证情况再回调上述的`@finish`或者`@finishFailed`事件。

第二种：在`<a-button>`中定义提交事件的方法，一般是需要定义一个表示表单组件的对象，即对`<a-form>`中的`ref`属性赋值（字符串如：formRef），然后定义一个对象

```vue
const formRef = ref();
```

可以调用该对象的方法，`formRef.value.validate().then(()=>{}).catch(()=>{})`

这里`then`和`catch`所做的事情，就像上面一种方法中`@finish`和`@finishFailed`所做的事情。



如何定义验证规则呢？

> 提前说明：只要涉及到验证或者对表单域的操作，`<a-form-item>`中的name属性是必填的，值就是表单对象对象的键名。

第一种：

还是以上图展示的基础表单举例，其中username部分对应代码如下：

```vue
<a-form-item
	label="Username"
	name="username"
	:rules="[{ required: true, message: 'Please input your username!' }]"
>
	<a-input v-model:value="formState.username" />
</a-form-item>
```

可以看到，想要定义规则，则可以向一个表单项`<a-form-item>`中的`rules`绑定一个数组。

该数组中是一个对象，对象有固定的几个键，比如`required`表示该项是否必须有值;`message`表示该项不满足验证条件时给出的消息提示。

第二种：

自定义验证规则

此时，不同对每个表单项单独设置rules。对`<a-form>`绑定`rules`。

```vue
import type { Rule } from "ant-design-vue/es/form";
const rules: Record<string, Rule[]> = {
    pass: [{ required: true, validator: validatePass, trigger: 'change' }],
    checkPass: [{ validator: validatePass2, trigger: 'change' }],
    age: [{ validator: checkAge, trigger: 'change' }],
};
//rules对象中的每个键对应表单中的每个项。validator的值是方法，一般都需要自定义。trigger表示对该项进行验证的时机。
```

同时，`<a-form>`中还要处理`@validate`事件， 任一表单项被校验后触发，回调参数`Function(name, status, errorMsgs)`。



如果需要提交的项数目是动态变化的呢？

我们可以动态增减表单项。

大致思路是，此时我们的表单对象内会有一个键，该键是表示每项内容的一个数组。

定义好增加和减少的事件，增加和减少数组的内容。

`<a-form-item>`需要结合`v-for`使用，同时表单项的`name`的值也要做相应改变。





Form.useForm

使用该功能，可以理解为不再需要为`<a-form>`绑定太多的内容，但需要更多的使用更多的方法和对象，并且每个表单项都需要做特殊绑定。