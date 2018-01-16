## react.js 是 React 的核心库
## react-dom.js 是提供与 DOM 相关的功能
## Browser.js 的作用是将 JSX 语法转为 JavaScript 语法

## ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
`ReactDOM.render(
    <h1>hi</h1>,
    document.getElementById('root')
)
`
### 注意：script标签中要加上 `type="text/babel"`  JSX语法需要

## React.createClass 方法就用于生成一个组件类，使用`this.props.xxx`来获取组件使用时传入的值
`let Message = React.createClass({
        render(){
            return <h1>hi,{this.props.name}</h1>
        }
    })
    
ReactDOM.render(          
    <Message name="lisa" />,
    document.getElementById('root')
)
`
### 注意，组件类的第一个字母必须大写，只能包含一个顶级标签

### 在ReactDOM.render方法中，使用组件时可以是双标签，也可以是单标签，单标签必须是要以`<  />`的形式来写

### JSX语法最好写在（ ）中

## 在JSX语法中，遇到`<>`当做html代码，遇到`{}`当做JS代码执行

## 通过`this.props.children`来读取组件的子节点
### 由于子节点有可能有多个又或者没有，所以会有三种情况：当没有子节点时，值为`undefined`，如果只有一个，那么值类型为`object`，如果值有多个，那么值类型为`array`。
### React提供一个工具方法React.Children来处理这种情况，可以不用考虑值类型
`<ol>
    {
        React.Children.map(this.props.children,child => {
            return <li>{child}</li>
        })                          
    } 
</ol>
`

## `PropTypes`验证使用组件时传入的值是否符合要求
`propTypes:{
    title : React.PropTypes.string.isRequired 
},
`
### 验证title的值必须为字符串且不能为空,任意类型加上 `isRequired` 来使 prop 不可空

## `getDefaultProps`可以设置组件属性的默认值，当组件调用但未传值时，使用默认值  类似vue的data属性
`getDefaultProps(){
    return {
        title : '相见'
    }
},
render(){
    return <h1>{this.props.title}</h1>
}
`

## 获取真实DOM节点 `ref="xxx"` 配合 `this.refs.xxx`

## `getInitialState`定义组件中的初始属性，返回一个对象，可以通过`this.state`来获取，通过`this.setState（）`方法修改状态，会自动调用`this.render`方法渲染页面
`render(){
    let test = this.state.liked?'like':'haven\'t liked';
    return (
        <p onClick={this.inputClick}>
            Your {test} this.Click to toggle
        </p>
    )
},
getInitialState(){
    return {
        liked:false
    }
},
inputClick(event){
    this.setState({liked : !this.state.liked})
}
`
### 注意：`this.props` 表示那些一旦定义，就不再改变的特性，而 `this.state` 是会随着用户互动而产生变化的特性

## 表单之中实现双向数据绑定 
`getInitialState(){
    return {value:'木'}
},
myChange(e){
    // console.log(e.target);
    this.setState({value : e.target.value})
},
render(){
    return (
        <div>
            <input type="text" onChange={this.myChange} value={this.state.value}/>
            <p>{this.state.value}</p>
        </div>
    )
}
`

## 组件的生命周期
1. Mounting：已插入真实 DOM
2. Updating：正在被重新渲染
3. Unmounting：已移出真实 DOM

## `will` 函数在进入状态之前调用，`did` 函数在进入状态之后调用，所以一共有五种处理函数
1. componentWillMount()
2. componentDidMount()
3. componentWillUpdate(object nextProps, object nextState)
4. componentDidUpdate(object prevProps, object prevState)
5. componentWillUnmount()

## 组件中使用style样式要用双`{{}}` , `style={{opacity:this.state.opacity}}`

## ajax请求，在`componentDidMount`生命周期函数，真实DOM插入之后执行,使用`this.setState`渲染

## 请求方式可以引入jQuery，使用`$.get`，也可以使用`fetch()`方法
`componentDidMount(){
    fetch(this.props.source).then(result => result.json()).then(result => {
        console.log(result);
        let data = result[0];
        if(this.isMounted()){
            this.setState({
                ownerLogin : data.owner.login,
                comments : data.comments
            })
        }
    })
}`

## 也可以将一个promise对象传入组件
` <Message promise={$.getJSON('https://api.github.com/search/repositories?q=javascript&sort=stars')}/>`
## 在组件中这样渲染
`componentDidMount(){
    this.props.promise.then(
        value => this.setState({loading: false, data: value}),
        error => this.setState({loading: false, error: error})
    );
}`






