# React
- 组件的属性(className)给children：{...this.props}
- 改变state不使用push方法
# 状态提升
你一定听说过变量提升，函数提升，那么状态提升是什么呢？

首先你得了解双向绑定和单向数据流，双向绑定中，数据可以在不同的组件之间实现共享，这样做的确有很大的好处，但是在react中，不推荐使用双向绑定，而是使用状态提升的方式。

状态提升：state推崇单向数据流，数据从父组件通过props流向子组件，如果你在子组件中，需要修改state来和其他子组件共享数据更新，你需要使用回调函数给使数据更新给父组件，然后从父组件流向其他的子组件，这样做是保证数据有单一的来源。

如果子组件和子组件之间任意共享数据，那么，后期维护会比较痛苦，特别是找bug的时候。

看一个状态提升的例子吧。
<pre><code>

class Child extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.upDateValue(e.target.value);
  }

  render() {
    const {name, value} = this.props;
    return (
      <div>
        <p>{name}：</p>
        <input value={value}
               onChange={this.handleChange}
          />
      </div>
    );
  }
}

class Demo extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: '', name: ''};

    this.upDateValue = this.upDateValue.bind(this);
  }

  upDateValue(value) {
    this.setState({value: value})
  }

  render() {
    const {value} = this.state;

    return (
      <div>
        <Child name="组件1" value={value} upDateValue={this.upDateValue} />
        <Child name="组件2" value={value} upDateValue={this.upDateValue} />
      </div>
    );
  }
}

ReactDOM.render(
  <Demo />,
  document.getElementById('root')
);
</pre></code>
<pre><code>传递属性给子组件
const childrenWithProps = React.Children.map(this.props.children,
	child => React.cloneElement(child, {
		equityRate: this.publishTasks,
	}),
);
</pre></code>
<pre><code>this.context用法
getChildContext() {		// 父组件
	return {
		USER: this.state.USER,
	};
}
Commen.childContextTypes = {
  USER: PropTypes.object,
};
Content.contextTypes = {    // 子组件
  USER: PropTypes.object,
};
</code></pre>