# 使用Redux管理React数据流要点浅析

## Store管理state并绑定视图

![](https://rudy.org.cn/images/redux-react/1.png)

在图中，使用Redux管理React数据流的过程如图所示，Store作为唯一的state树，管理所有组件的state。组件所有的行为通过Actions来触发，然后Action更新Store中的state,Store根据state的变化同步更新React视图组件。

那么Store是如何和视图绑定的呢？
在主入口文件index.js中，通过`Provider`标签把`Store`和`View`绑定：

```java
    <Provider store={store}>
         //do something
    </Provider>
```

在项目中，Store是如何具体管理State呢？实际上是通过Reducers根据不同的Action.type来更新不同的state，即（previousState,action） => newState。最后利用Redux提供的函数combineReducers将所有的Reducer进行合并，更新整个State树。

在Redux项目中，利用Container容器组件作为桥梁，将React组件和Redux管理的数据流联系起来。Container通过connect函数将Redux的state和action转化成展示组件即React组件所需的Props。

```java
    //将state.data绑定到相应的React组件的Props的data
    function mapStateToProps(state){
       return {
          data:state.data
       }
    }
    //将actions的所有方法绑定到相应的React组件的Props上
    function mapDispatchToProps(dispatch){
       return bindActionCreators(actions,dispatch)
    }
    //通过react-redux提供的connect方法将我们需要的state中的数据和actions中的方法绑定到相应的React组件的Props上
    export default connect(mapStateToProps,mapDispatchToProps)(reactComponent)
```

通过上面的分析，我们总结为一幅更详细的图示：

![](https://rudy.org.cn/images/redux-react/2.png)

***

## 注册Store：

```java
    //applyMiddleware可以包装Store的dispatch,thunk的作用是使action创建函数可以返回一个function替代action对象
    const createStoreWithMiddleware = applyMiddleware(thunk)(createStore)
    const store = createStoreWithMiddleware(reducer,initialState)
```

这里可能有同学会有疑问，React的state和Redux所说的state是同一回事吗？下面我们简要分析一下：

首先，React组件的state和props定义如下：

state-只表示一些临时的、内部的状态数据，例如窗口的打开或关闭状态。

props-通常存储一些方法，一些可能需要库存的长期数据和一些需要传递的共享数据。

然后，Redux和React基本上只有2种联系：

要么React从Redux的state中读取数据，要么React通过dispatch分发action到Redux,Redux的reducer来返回一个新的state。

结论是Redux的state存放的是全局的长期数据，也就是对应的React组件的Props数据，而组件React的state应该是临时的内部状态数据，所以这两个state没有半毛钱关系。

## 下面我们总结一下Redux的三大原则和数据流的管理：

Redux三大原则：

1、单一数据源，这个应用的state被存储在一棵object tree中，并且这个object tree只存在于唯一的Store中。

2、state是只读的，唯一改变state的方法就是触发action，action是一个用于描述已发生事件的普通对象。

3、使用纯函数来执行修改，为了描述action如何改变state tree，需要编写reducer。

Redux数据流的管理：

1、action:把数据传递到Store，唯一数据来源。

2、reducer:action只描述有事情发生，reducer指明如何更新state，即设计state结构和action处理。

3、Store:把action和reducer联系到一起，负责维持、获取和更新state。

4、生命周期：数据流严格且单向

  调用Store.dispatch(action)->Store调用传入的reducer函数，Store会把两个参数传入reducer:当前的state树和action->根reducer将多个子reducer输出合并成一个单一的state树->Store保存了根reducer，并返回完整的state树。








