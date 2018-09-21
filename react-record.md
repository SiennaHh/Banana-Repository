# 1.为什么在onChange中去setFieldsValue是没有效果的呢？
看该方法源码:可以看出 onCollect 方法中调用了 onCollectCommon ，根据意思是通用的收集变化的处理方法，
```js
// action实际上是trigger 默认是onChange
onCollectCommon(name, action, args) {  
  const fieldMeta = this.fieldsStore.getFieldMeta(name);
  if (fieldMeta[action]) {
    // onchange getFieldProps中写法
    fieldMeta[action](...args);
  } else if (fieldMeta.originalProps && fieldMeta.originalProps[action]) {
    // onchange getFieldDecorator中的写法
    fieldMeta.originalProps[action](...args);
  }
  // some code...
  // onchange同步执行完后再执下面代码，下面的返回并不会受onchange同步执行的影响
  return ({ name, field: { ...field, value, touched: true }, fieldMeta });
},
```
onChange 之类的方法是在 fieldMeta[action](...args); 或者 fieldMeta.originalProps[action](...args); 这两行代码执行的，在这进行 setFieldsValue （代码片段三中执行的方法）会进行一次刷新，但是后续执行 setFields 会覆盖掉之前的数据， setFields 并不会受中间的 setFieldsValue 影响，还是设置原来本需要设置的值。所以就很好解释了 
## 查阅文档后发现有一个属性， options.normalize
官方解释：转换默认的 value 给控件；function(value, prevValue, allValues): any
示例：
```js
class Demo4 extends Component {  
  render() {
    const { getFieldProps } = this.props.form;
    return (
      <div>
        <Select
          {...getFieldProps('name', {
            normalize: (value, prevValue, allValues) => {
              let targetValues = []
              const nowValues = prevValue || [];
              if (nowValues.length > value.length) {
                targetValues = value;
              } else {
                const selectValue = value.find(nv => nowValues.indexOf(nv) === -1)
                if (selectValue === '0') {
                  targetValues = ['0']
                } else {
                  targetValues = value.filter(x => x !== '0');
                }
              }
              return targetValues;
            }
          })}
          style={{ width: 800 }}
          mode="multiple"
        >
          {data.map((item) => <Option key={item.value}>{item.name}</Option>)}
        </Select>
      </div>
    );
```
官方示例：
```js
const { Checkbox, Form } = antd;
const CheckboxGroup = Checkbox.Group;
const FormItem = Form.Item;

const options = [
  { label: 'All', value: 'All' },
  { label: 'Apple', value: 'Apple' },
  { label: 'Pear', value: 'Pear' },
  { label: 'Orange', value: 'Orange' },
];

class Demo extends React.Component {
  normalizeAll = (value, prevValue = []) => {
    if (value.indexOf('All') >= 0 && prevValue.indexOf('All') < 0) {
    	return ['All', 'Apple', 'Pear', 'Orange'];
    }
    if (value.indexOf('All') < 0 && prevValue.indexOf('All') >= 0) {
    	return [];
    }
    return value;
  };
  render() {
    return (
      <Form>
        <FormItem>
          {this.props.form.getFieldDecorator('fruits', {
            normalize: this.normalizeAll,
          })(<CheckboxGroup options={options} />)}
        </FormItem>
      </Form>
    );
  }
}

const WrappedDemo = Form.create()(Demo);

ReactDOM.render(<WrappedDemo />, mountNode);
```

# 2.React 路由传参
React路由取参数，有两种：
1.?a=1 ：这种属于 search 字符串，在 location.search 里取值；
2./a/123 ：这种需要从 match.params里取值；
但无论哪种，路由获取到的值，是跳转后的那一刻的值，而不是实时更新的最新值。


