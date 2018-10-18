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
## 2.Warning: Each child in an array or iterator should have a unique "key" prop.
1. 原因：数组循环时缺少unique "key" prop,意思就是要有一个不会重复的关键参数key。
* http://pandakeeper.net:8000/?p=254 中是这么说的：<br/>
`
react的key关乎到react的dom-diff算法 react中对于dom的操作是根据生成的data-reactid进行绑定的，添加key可以保证dom结构的完整性，而不会根据react自己对dom标记的key进行重新分配 react每次决定重新渲染的时候，几乎完全是根据data-reactid来决定的，最重要的是这个机制
`
2. 解决办法：循环的时候加个key={i} 虽然对开发并没啥用，但是必须加

## 3.Warning:Each record in table should have a unique `key` prop,or set `rowKey` to an unique primary key警告
解决办法：react报错写了，Each record in dataSource of table should have a unique `key` prop, or set `rowKey` of Table to an unique primary key。
*** 方法一：给数据加入key的键值对，"key": "1",
*** 方法二：给Table设置rowKey,
```js
<Table className="table" rowKey={record => record.goodsSn} columns={this.columns} dataSource={this.state.goodList} />
```
