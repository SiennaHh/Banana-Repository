## 1.`getFieldDecorator` will override `value`警告
在使用Form组件时，如果我们手动添加value属性，会导致warning：Warning: `getFieldDecorator` will override `value`, so please don't set `value` directly and use `setFieldsValue` to set it.
如：
```js
<RadioGroup onChange={this.onChange} value={1}>  //手动添加了value属性
    <Radio value={1}>A</Radio>
    <Radio value={2}>B</Radio>
</RadioGroup>
```
* 原因：使用getFieldDecorator（）方法包装后的组件会自动更新表单组件的value以及onChange事件，无需再手动添加value属性,但onChange事件可根据需求添加以便监听数据变化。
如果需要填写初始默认值可使用initialValue进行设置。
如:
```js
<FormItem{...formItemLayout} label="用户类型">
      {getFieldDecorator('userType', {
          initialValue: 1,    //可以在这里设置默认初始值
          rules: [ {
              required: true, message: '请选择用户类型！',
          }],
      })(
          <RadioGroup onChange={this.onChange} >
              <Radio value={1}>采购商</Radio>
              <Radio value={2}>供应商</Radio>
          </RadioGroup>
      )}
  </FormItem>
```
