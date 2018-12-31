# 数据可视化-gojs插件使用技巧总结

随着云计算时代的到来，由于Web技术的快速革新以及为了提供高质量的用户体验，数据可视化成为了前端技术发展的一大方向。为了解决这个问题，现如今涌现了很多优秀的第三方的javascript图形库，比如highcharts.js,echarts.js,d3.js,go.js…

## 数据可视化javascript插件对比

在HTML5标准支持下，web实现图形标准主要分为canvas和svg，上述的javascript图形库都是依赖2者之一作为底层库。Canvas基于像素，提供2D绘制函数，是一种HTML标签，依赖于HTML，只能通过javascript 绘制图形；SVG为矢量，提供一系列图形元素、动画、事件机制，既可以独立使用，也可以嵌入HTML中使用。 图形库的封装程度也有区分，像highchart.js,echart.js属于过度封装，只暴露了数据模型接口，作为开发者很难修改内部API实现，不利于企业实现定制化需求，而像d3.js,go.js更多的是对图表的节点、连线及工具的封装，并预留了接口便于开发者进行定制化开发。另外，图形库的商业版权也各尽不同，像highchart.js,go.js都是要收费的，而echart.js,d3.js则是免费开源的。

## gojs简介及特点

gojs是一款基于canvas的图形库，是由Northwoods公司开发的商用javascript插件，可以基于项目的不同需求实现具有交互性的各类图表，比如流程图，树图，关系图，力导图等等。gojs采用面向对象思想，以图形对象表示绘图单元，JSON对象作为数据模型，图形对象通过属性绑定的方式从数据模型获取相关的属性值。

### 图形对象与数据模型关系图

![](https://rudy.org.cn/images/gojs/1.png)

## gojs数据模型

gojs的数据模型以是否为树图分为`GraphLinksModel`和`TreeModel`两种JSON对象，GraphLinksModel包含nodeDataArray和linkDataArray属性，而TreeModel只包含nodeDataArray属性。

## gojs绘图单元

gojs的绘图单元很好理解，比如图中一个节点，一条线都可以理解成一个绘图单元，gojs通过不同的绘图模板实现不同的绘图单元，比如node,group,line…另外，gojs通过模板地图的方式管理不同样式的相同类型的绘图单元。

## gojs图表实践

gojs绘图流程包括创建图形对象，构建数据模型，设置图形对象属性，绑定数据模型，添加交互行为。

### gojs创建流程图

![](https://rudy.org.cn/images/gojs/2.png)

1、创建图形对象可以把$理解成一个画笔，而myDiagram理解成画布。
```java
var $ = go.GraphObject.make;
```
画图时，通过$调用gojs自身的属性和方法 ， 完成节点和连线的绘制，attrs为图形对象属性。
```java
var myDiagram = $( go.Diagram, "dom_id" , {attrs});
myDiagram.nodeTemplate = $( go.Node, "Auto", {attrs});
myDiagram.linkTemplate = $( go.link, {attrs});
```
2、构建数据模型 
数据模型分为2种，下面以图形连线模型为例，它包括nodeDataArray和linkDataArray:
```java
var dataModel = $(go.GraphLinksModel);
dataModel.nodeDataArray = [{},{}];
dataModel.linkDataArray = [{},{}];
myDiagram.model = dataModel;
```
3、图形对象属性绑定 
举例说明，比如将图形对象的边框宽度strokeWidth和数据模型的宽度Width进行绑定：
```java
new go.binding("strokeWidth","width");
```
4、添加交互行为 
举例说明，比如为node添加鼠标事件，通过给其属性添加相应方法进行事件绑定：
```java
{mouseEnter:onNodeMouseEnter}

function onNodeMouseEnter(){
   //do something
}
```
5、本地调试 
建议安装Node.js，完成后安装http-server:
```java
npm install http-server
```
然后在项目主目录启动本地Web服务： 

![](https://rudy.org.cn/images/gojs/3.png)

## gojs学习与思考

目前，官方网站[gojs](https://gojs.net/latest/index.html)是学习gojs的最佳选择。学习路径：*learn->introduction->samples->api*

### 元数据地图实践 

gojs不足之处在于对于css动画支持不够，商用版权导致开发成本增加。优势在于canvas库封装较好，提供丰富的交互事件，能够满足实际项目的个性化需求。在项目使用中，对于常见图表，项目实际使用echartjs作为替代选择，对于定制化需求则采用gojs实现。

