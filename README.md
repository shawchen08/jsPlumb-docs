# jsPlumb 文档

只翻译了部分常用的，更详细的请看[官方文档](https://jsplumbtoolkit.com/community/apidocs/classes/jsPlumb.html)

[jsPlumb](https://github.com/jsplumb/jsplumb) 是一个在元素之间创建可视化连接的 js 库，并内置拖拽功能，兼容 IE9+（最后一个兼容 IE8 的版本是 1.7.10）

## 对外接口

jsPlumb

### 创建实例

```js
var myInstance = jsPlumb.getInstance(options)
```

## `options`

**注：** 在 `jsPlumb.getInstance(options)` 中使用（创建实例）时，首字母大写，其他地方使用首字母小写

<i id="to-anchor"></i>

### Anchor / Anchors

锚点（用来放置连接点的地方）位置参数，单个用 `Anchor`，多个用 `Anchors`

#### 内置连接点位置方向定义

`String`

- `Bottom`: 底边中间，方向朝下，`BottomCenter` 的替代者
- `Top`: 顶边中间，方向朝上，`TopCenter` 的替代者
- `Left`: 左边中间，方向朝左，`LeftMiddle` 的替代者
- `Right`: 右边中间，方向朝右，`RightMiddle` 的替代者
- `BottomLeft`: 左下角，方向朝左下
- `BottomRight`: 右下角，方向朝右下
- `TopLeft`: 左上角，方向朝左上
- `TopRight`: 右上角，方向朝右上
- `Center`: 元素中心，方向任意
- `AutoDefault`: 根据连线终点位置，自动从 Top, Right, Bottom, Left 中选择
- `Continuous`: 类似 `AutoDefault`，**待研究**
- `ContinuousBottom`: 类似 `Bottom`，**待研究**
- `ContinuousTop`: 类似 `Top`，**待研究**
- `ContinuousLeft`: 类似 `Left`，**待研究**
- `ContinuousRight`: 类似 `Right`，**待研究**
- `Perimeter`: 跟踪某些形状周长的锚点，用给定的锚点位置去逼近它，**待研究**

#### 自定义连接点位置方向定义

`Array`

`[x, y, dx, dy, ox, oy]`

- x, y: 位置，基于元素左上角（x 轴朝右，y 轴朝下），(1, 1) 就是右下角，必需
- dx, dy: 方向，基于元素中心（x 轴朝右，y 轴朝下），(0, -1) 向上，(0, 1) 向下，(1, 0) 向右，(-1, 0) 向左，（0, 0） 任意，不传就是 (0, 0)。其他方向基本不用
- ox, oy: 位置偏移，基于元素左上角（x 轴朝右，y 轴朝下），单位为 px，不传就是 (0, 0)

使用示例：

```js
Anchor: 'Bottom'

// or

Anchors: ['Bottom', 'Left', [1, 0.3, 1, 0]]
```

<i id="to-overlays"></i>

### Overlays

连接点和连接线上的标签，有四种：

- `Arrow`: 尾部缩进的箭头，有以下属性：
  - `width`: `Number` 箭头宽度，默认 20
  - `length`: `Number` 箭头长度，默认 20
  - `location`: `Number` 在线条上的位置，0 在起点，1 在终点，默认 0.5
  - `direction`: `Number` 方向，从起点指向终点为 1，反之为 -1
  - `foldback`: `Number` 箭头尾部缩进程度，设置为 1 则尾部不缩进，小于 1 向内缩进，大于 1 向外突出，小于等于 0 取默认值。 默认 0.623
  - `paintStyle`: `Object` 箭头绘制样式配置，也就是 svg 相关属性
  - `visible`: `Boolean` 是否可见，默认 true
- `Diamond`: 菱形，这是 `Arrow` 的一个特殊实例，`Arrow` 的 `foldback` 配置为 2 就变成了菱形
- `PlainArrow`: 三角形，这是 `Arrow` 的一个特殊实例，`Arrow` 的 `foldback` 配置为 1 就变成了三角形
- `Label`: 文本标签，有以下属性：
  - `label`: `String` 文本值
  - `location`: `Number` 在线条上的位置，0 在起点，1 在终点，默认 0.5
  - `cssClass`: `String` 样式类
  - `visible`: `Boolean` 是否可见，默认 true
  - `labelStyle`: `Object` css 样式
    - `font`: `String` 字体
    - `color`: `String` 颜色
    - `fill`: `String` 背景
    - `borderStyle`: `String` 边框颜色
    - `borderWidth`: `String` 边框宽度
    - `padding`: `String` 内填充

使用方式（可多个标签一起使用）：

- 使用默认配置可简写为：

```js
overlays: ['Arrow']
```

- 要调整配置则需写为：

```js
overlays: [['Arrow', { width: 30 }]]
```

- 多个一起用：

```js
overlays: [['Arrow', { location: 0.7 }], 'Diamond', ['Arrow', { location: 0.3, direction: -1 }]]
```

- 多个一起用时可抽取公共配置：

```js
var arrowCommon = { foldback: 0.7, fill: color, width: 14 };

...
overlays: [['Arrow', { location: 0.7 }, arrowCommon], ['Arrow', { location: 0.3, direction: -1 }, arrowCommon]]
...
```

### Endpoint / Endpoints

`String`，连接点类型，可用类型如下：

- Dot: 圆
- Rectangle: 矩形
- Triangle: 三角形
- Image: 图片
- Blank: 空

<i id="to-endpoint-style"></i>

### EndpointOverlays

连接点标签，主要是文本，参考[Overlays 的 Label 类型](#to-overlays)

### EndpointStyle / EndpointStyles

`Object | Array`，连接点样式，有以下属性（svg 有的属性基本都有）：

- stroke: 边线颜色
- strokeWidth: 边线宽度
- fill: 填充颜色
- radius: 半径，Dot 专用，默认 10
- width: 宽度，Rectangle、Triangle 可用，默认分别为：20、55
- height: 高度，Rectangle、Triangle 可用，默认分别为：20、55
- src: 图片路径，Image 必选

### EndpointHoverStyle / EndpointHoverStyles

连接点 hover 样式，同[EndpointStyle / EndpointStyles](#to-endpoint-style)

### PaintStyle

连接线样式，可用属性参考[EndpointStyle](#to-endpoint-style)

### HoverPaintStyle

连接线 hover 样式，可用属性参考[EndpointStyle](#to-endpoint-style)

<i id="to-connector"></i>

### Connector

连接线类型，默认 `Bezier`

- `Straight`: 直线连接，可用属性：
  - `stub`: `Integer | Array` 连线两端短截线的最小长度。`Integer`--为连接的两端赋予同样的值，`[Integer, Integer]`--分别为起点、终点赋值，默认 0
  - `gap`: `Integer | Array` 连线两端和端点（endpoint）所在的元素之间的间隙，与 `stub`，可以一起赋值也可以分别赋值，默认 0
- `Flowchart`: 折线连接，可用属性：
  - `stub`: `Integer | Array` 连线两端短截线的最小长度。`Integer`--为连接的两端赋予同样的值，`[Integer, Integer]`--分别为起点、终点赋值，默认 30
  - `gap`: `Integer | Array` 连线两端和端点（endpoint）所在的元素之间的间隙，与 `stub`，可以一起赋值也可以分别赋值，默认 0
  - `cornerRadius`: `Number` 连线转向处的圆角，默认 0
  - `alwaysRespectStubs`: `Boolean` true--固定短截线长度；false--当两个元件彼此非常接近（小于两个短截线的总和），则调整短截线。默认 false
  - `midpoint`: `Number` 设置连线的中点位置，默认 0.5
- `Bezier`: Bezier 曲线连接，可用属性：
  - `curviness`: `Number` 曲率，值越大，弯曲程度越大，默认 150
  - `stub`: `Integer | Array` 连线两端短截线的最小长度。`Integer`--为连接的两端赋予同样的值，`[Integer, Integer]`--分别为起点、终点赋值，默认 0（实际使用没发现有什么用）
- `StateMachine`: 二次 Bezier 曲线连接，可用属性：
  - `curviness`: `Number` 曲率，值越大，弯曲程度越大，默认 10
  - `margin`: `Integer` 连线两端和端点（endpoint）所在的元素之间的间隙，默认 5
  - `proximityLimit`: `Integer` 当两个元素之间的距离小于给定距离时，曲线变直线，默认 80
  - `showLoopback`: `Boolean` 同一个元素的不同端点间连接类型：true--形成回环（一个圆，起点就是终点），false--与其他一样。默认 true
  - `loopbackRadius`: `Integer` 回环半径，`showLoopback` 为 true 时有效，默认 25

使用例子：

```js
// 使用默认配置
Connector: 'Flowchart'

// 使用自定义配置
Connector: ['Flowchart', { stub: [10, 20], gap: 10, cornerRadius: 5 }]
```

### ConnectionOverlays

连接线上的标签，可用属性同[Overlays](#to-overlays)

### ConnectionsDetachable

`Boolean`

连接是否可拆卸（使用鼠标），默认 `true`

### ReattachConnections

`Boolean`

是否重新连接用户已使用鼠标分离然后删除的连接。 默认值为 `false`

<i id="to-container"></i>

### Container

`String`

容器选择器，指定 `jsPlumb` 的作用区

### DoNotThrowErrors

`Boolean`

是否在用户指定未知锚点，端点或连接器类型时抛出错误。默认 `false`

### LogEnabled

`Boolean`

是否启用 `jsPlumb` 日志。 默认为 `false`

### MaxConnections

`Number`

端点的默认最大连接数。 默认为 `1`

### Scope

`String`

待研究 默认为 `_jsPlumb_Default_Scope`

### DragOptions / DropOptions

`Object`

拖放配置，待研究，在 `connect` `makeTarget` `addEndpoint` 方法中用到

## 方法

### addEndpoint(id, params, referenceParams)

将端点添加到给定元素

- `id`: `String | Element` 作用节点 id 选择器或 DOM
- `params`: `Object` 配置，可用属性如下：
  - `anchor`: `String | Array` 锚点（连接点位置）配置，参考[Anchors](#to-anchor)。`String` 用的是内置的或者自行注册的，`Array` 用的是具体的配置，如 `[x, y, dx, dy, ox, oy]`
  - `endpoint`: `String`，连接点类型，可用类型如下：
    - Dot: 圆
    - Rectangle: 矩形
    - Triangle: 三角形
    - Image: 图片
    - Blank: 空
  - `paintStyle`: `Object`，连接点样式，有以下属性（svg 有的属性基本都有）：
    - stroke: 边线颜色
    - strokeWidth: 边线宽度
    - fill: 填充颜色
    - radius: 半径，Dot 专用，默认 10
    - width: 宽度，Rectangle、Triangle 可用，默认分别为：20、55
    - height: 高度，Rectangle、Triangle 可用，默认分别为：20、55
    - src: 图片路径，Image 必选
  - `hoverPaintStyle`: 连接点 hover 样式，同 `paintStyle`
  - `cssClass / hoverClass`: 样式类及 hover 样式类
  - `enabled`: `Boolean` 是否响应鼠标事件（drag / drop），默认 true
  - `source`: `String | Element` 选择器或节点，用来指定作用节点，**必需**
  - `container`: 同[Container](#to-container)
  - isSource: `Boolean`，设为 true 标记为起点，默认 false
  - isTarget: `Boolean`，设为 true 标记为终点，默认 false
  - hoverPaintStyle: `Object`，连接点 hover 时的样式，属性同 paintStyle
  - connector: `Array`，连接线配置，起点可用
  - connectorStyle: `Object`，连接线的样式，起点可用，有以下属性（svg 有的属性基本都有）：
    - stroke: 线颜色
    - strokeWidth: 线宽度
    - joinstyle: 'round', 不明作用
    - outlineStroke: 线边缘颜色
    - outlineWidth: 线边缘宽度
    - 'stroke-dasharray': 虚线配置
    - dashstyle: 同 stroke-dasharray
  - connectorHoverStyle: `Object`，连接线 hover 时的样式，属性同 connectorStyle，起点可用
  - cssClass: `String` 附加到 Endpoint 创建的元素的 CSS 类
  - hoverClass: `String` 当鼠标悬停在元素或连接的线上时附加到 EndPoint 创建的元素
- `referenceParams`: `Object` 共享配置（我们可以把多个 Endpoint 共用部分抽出来放在这里），可用属性同 `params`

### addEndpoints(id, params, referenceParams)

将端点列表添加到给定元素，与 `addEndpoint` 别在于第二个参数是个数组，也就是多个端点配置

### batch(fn, doNotRepaintAfterwards)

暂停绘图，运行给定的函数，然后重新启用绘图，doWhileSuspended 方法的替代者，jsPlumb 的大部分操作都要求放入 fn 中进行

- `fn`: `Function`，给定函数
- `doNotRepaintAfterwards`: `Boolean`，如果传 true，则在运行给定的函数后不会运行重绘

### bind(event, callback, insertAtStart)

绑定事件

- `event`: `String` 事件类型：
  - `click`: 连线点击事件
  - `connection`: 连接事件
  - `beforeDetach`: 连接断开前事件
  - `connectionDetached`: 连接断开事件
  - `connectionDrag`: 连线开始拖拽事件（端点）
  - `connectionDragStop`: 连线拖拽结束事件（端点）
  - `connectionMoved`: 连线更换端点事件
  - 更多类型待研究
- `callback`: `Function` 事件回调，具体传参自行研究
- `insertAtStart`: 是否置顶此事件（在其它已注册事件之前触发），默认 `false`，**选填**

### cleanupListeners()

移除所有事件

### registerConnectionType(id, options)

注册连接类型

- id: `String`, 连接类型的 id，确保唯一，切换类型时用到
- options: `Object`, 类型配置
  - connector: `String`, 四个取值：`Straight`--直线连接，`Flowchart`--折线连接，`Bezier`--Bezier 曲线连接，`StateMachine`--二次 Bezier 曲线连接
  - paintStyle: `Object`, 样式
  - hoverPaintStyle: `Obejct`, hover 时的样式
  - overlays: `Array`

### connect(params, referenceParams)

连接端点

- `params`: `Object`，有以下属性
  - `source`: `String | Object`，起点，可以是选择器或节点，也可以是 Endpoint
  - `target`: `String | Object`，终点，可以是选择器或节点，也可以是 Endpoint
  - `uuids`: `Array`，`[source, target]`，source 和 target 的合体
  - `type`: `String`，自定义的类型（通过`registerConnectionType(id, options)`注册的）
  - `connector`: `String`，连接类型，参考[Connector](#to-connector)
  - 连线的相关配置都可以在此使用（比如 overlays）

### deleteConnection(connection)

删除指定连接，connection 可通过 `getAllConnections / getConnections` 方法获取

### deleteEndpoint(endpoint, doNotRepaintAfterwards)

删除端点，同时会删除其连线

- `endpoint`: `String`，端点 uuid 或 Endpoint（可通过 `getEndpoint` 方法获取）
- `doNotRepaintAfterwards`: `Boolean`，是否重新绘制所有内容，默认 `false`，**选填**

### draggable(selector, options)

添加拖动功能

- `selector`: 需要添加拖动的元素选择器，也就是流程图的块，可以是 DOM 列表或选择器：
  - `Array`: `jsPlumb.getSelector('.draw-canvas .item')` 或 `document.querySelectorAll('.draw-canvas .item')`
  - `String`: `'.draw-canvas .item'`
- `options`: `Object`，拖动配置，不配置就使用全局配置（`jsPlumb.getInstance({DragOptions: options})`），参考 [jQuery-ui 的 draggable](http://www.jqueryui.org.cn/api/3722.html)，下面列出一些常用的：
  - `grid`: `Array`, 位置对齐参数：`[x, y]`，越小拖动越平滑，如：`[1, 1]`
  - `containment`: `String`，约束在指定元素或区域的边界内拖拽（支持更多类型，这里不在多列举，具体参考 jQuery-ui 的 draggable）
  - `stop`: `Function`，结束拖拽的回调
  ```js
  stop: function(event) {
    // event.el       拖动元素
    // event.finalPos 结束时的位置，[x, y]
  }
  ```

### getAllConnections()

获取所有连接

### getConnections(params)

用法不明，请使用 `getAllConnections`

### getContainer()

获取容器节点

### getDefaultScope()

获取默认 scope

### getEndpoint(uuid)

根据 uuid 获取 Endpoint

### getInstance(options)

创建实例

### makeSource(el, params)

将指定的 DOM 元素创建成为连接源，允许从它/它们拖动连接，而无需事先注册任何端点。建立连接后，传入此方法的端点配置将用于创建端点（如果未提供，则将使用默认端点配置）

- `el`: `String | Element`，选择器或 DOM
- `params`: `Object`，创建配置，有以下属性：
  - `endpoint`: `String | Array`，端点配置
  - `parent`: `String | Element`，选择器或 DOM。建立连接时添加端点的元素，如果省略，端点将被添加到`el`
  - `scope`: `String`，从此元素出发的连接线的连接的范围
  - `dragOptions`: `Object`，拖动配置
  - ... 更多配置待研究

### makeTarget(el, params)

将指定的 DOM 元素创建成为连接目标，而无需事先注册任何端点。建立连接后，传入此方法的端点配置将用于创建端点（如果未提供，则将使用默认端点配置）

- `el`: `String | Element`，选择器或 DOM
- `params`: `Object`，创建配置，有以下属性：
  - `endpoint`: `String | Array`，端点配置
  - `parent`: `String | Element`，选择器或 DOM。建立连接时添加端点的元素，如果省略，端点将被添加到`el`
  - `scope`: `String`，从此元素出发的连接线的连接的范围
  - `dragOptions`: `Object`，拖动配置
  - ... 更多配置待研究

### toggleVisible(el)

显示/隐藏指定节点的连线

- `el`: `String | Element`，选择器或 DOM

### toggleDraggable(el)

切换指定节点的可拖拽状态

- `el`: `String | Element`，选择器或 DOM

### removeAllEndpoints(el)

移除指定节点的所有端点

- `el`: `String | Element`，选择器或 DOM

### remove(el)

移除指定节点及其所有端点、连线，当然也包括子节点

- `el`: `String | Element`，选择器或 DOM

### empty(el)

清空指定元素：属于子元素的所有端点和连接，以及子元素本身，保留属于元素本身的端点和连接。

- `el`: `String | Element` 选择器或节点

### reset(doNotUnbindInstanceEventListeners)

清空所有端点、连接及事件。

- `doNotUnbindInstanceEventListeners`: `Boolean` 若设为 true，事件绑定不会被移除
