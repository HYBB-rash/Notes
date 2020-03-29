# JavaScript实现拖放效果

笔者实现该效果也是套用别人的轮子的。[传送门](https://github.com/qgh810/dnd)

然后厚颜无耻的贴别人的readme~，笔者为了方便查阅就直接贴了，有不想移步的可以看这篇。不过还是最好请到原作者的GitHub去查看，支持一下原作者。

文章最后有贴`dnd.js`的源码。

## 使用介绍

## dnd.js

##### 拖放库 drag and drop

[查看DEMO](http://qgh810.github.io/demo/dnd/index.html)

不依赖任何第三方库的拖放库,兼容低版本浏览器,兼容移动端,自带常用动画效果,[如果在vue项目中使用](https://github.com/qgh810/dnd/tree/master/vue),可以拷贝上面vue文件夹中已经封装好的组件直接使用.

<img src="https://raw.githubusercontent.com/qgh810/draggable/master/demo/demo.gif" width="650">


### 安装方法

-  方式一

```bash
npm install dnd.js --save
```

- 方式二

```bash
下载项目中的dist/dnd.js, 然后用script标签插入到你的项目中, 如下
这种方式可以通过window.dnd访问
```

```html
<script type="text/javascript" src="dist/dnd.js"></script>
```

### 使用方法

-  最简单的使用方法可以可以查看[最小demo](https://github.com/qgh810/dnd/blob/master/demo/demo-mini.html)
-  也可以查看[稍微复杂一点的demo](https://github.com/qgh810/dnd/blob/master/demo/demo1.html)

```js
import { Drag, Drop } from 'dnd.js'

new Drag(element, options)
new Drop(element, options)
```

- 如果是使用方式二引入的话, 可以这样使用

```js
var Drag = dnd.Drag
var Drop = dnd.Drop
new Drag(element, options)
new Drop(element, options)
```

- 最小demo代码展示

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>最小化demo</title>
    <script type="text/javascript" src="../dist/dnd.js"></script>
    <style>
      html, body { width:100%; height:100%; }
      body { margin:0; padding:20px; box-sizing:border-box; }
      .drop { margin:20px auto; width:100%; height:100px; border:1px solid #000; }
    </style>
  </head>

  <body>
    <div class="drop"></div>
    <div class="drag">
      拖动我到上方框框
    </div>
  </body>

  <script>
    var Drag = dnd.Drag
    var Drop = dnd.Drop
    new Drag('.drag')
    new Drop('.drop', {
      onDrop: function (params) {
        params.el.appendChild(params.sourceNode)
      }
    })
  </script>
</html>
```


### 参数说明

-  创建

```js
new Drag(element, options)
new Drop(element, options)
```

| 参数    | 是否必填 | 类型                        | 说明                                     |
| :------ | :------: | :-------------------------- | :--------------------------------------- |
| element |    是    | String or htmlElementObject | 可以传入类名class 或者 id名 或者 dom节点 |
| options |    是    | Object                      | 相关参数和回调函数,具体说明看下面        |

-  options说明 (Drag)

例如:

```js
  let element = '.drag-container'
  let options = {
    data: '本次拖动希望传递给Drop对象的参数',
    onDragStart: function (params) {
      console.log('监听到拖动开始')
    },
    onDragEnd: function (params) {
      console.log('监听到拖动结束')
    }
  }
  new Drag(element, options)
```

##### options属性说明:

| 参数        | 是否必填 |   类型   | 说明                             |
| :---------- | :------: | :------: | :------------------------------- |
| data        |    否    |   任意   | 本次拖动希望传递给Drop对象的参数 |
| onDragStart |    否    | Function | 拖动开始回调函数                 |
| onDragEnd   |    否    | Function | 拖动结束回调函数                 |

-  options说明 (Drop)

例如:

```js
  let element = '.drop-container'
  let options = {
    name: '当前Drop对象的名字',
    onDragStart (params) {
      console.log('监听到拖动开始')
    },
    onDragEnter (params) {
      console.log('监听到被拖元素进入')
    },
    onDragOver (params) {
      console.log('监听到被拖动元素在自己上方移动')
      console.log('这个函数会被连续调用')
    },
    onDragLeave (params) {
      console.log('监听被拖动元素离开')
    },
    onDrop (params) {
      console.log('监听到被拖动元素在自己上方放下')
    },
    onDragEnd (params) {
      console.log('监听到拖动结束')
    }
  }
  new Drop(element, options)
```

##### options属性说明:

| 属性        | 是否必填 |   类型   | 说明                                                   | 回调函数参数说明 |
| :---------- | :------: | :------: | :----------------------------------------------------- | :--------------- |
| name        |    否    |  String  | 定义当前Drop对象的名字                                 | -                |
| onDragStart |    否    | Function | 拖动开始时调用                                         | 见下方说明       |
| onDragEnter |    否    | Function | 拖动进入时调用                                         | 见下方说明       |
| onDragOver  |    否    | Function | 被拖动元素在自己上方移动时候调用, 这个函数会被连续调用 | 见下方说明       |
| onDragLeave |    否    | Function | 拖动离开时候调用                                       | 见下方说明       |
| onDrop      |    是    | Function | 被拖动元素在自己上方放下时调用                         | 见下方说明       |
| onDragEnd   |    否    | Function | 拖动结束时候调用                                       | 见下方说明       |

##### 回调函数的参数params说明:

| 属性       | 类型    | 描述                                                         |
| :--------- | :------ | :----------------------------------------------------------- |
| data       | 不定    | 被拖动元素定义的data属性, 类型由Drag对象被创建的时候传入的data属性决定 |
| el         | Object  | 当前dom节点                                                  |
| enter      | Boolean | 是否进入当前范围的标志位, 布尔值                             |
| methods    | Object  | 见下方 回调函数参数中的methods说明                           |
| name       | String  | Drop名称, 在销毁当前Drag对象时候需要用到                     |
| sourceNode | Object  | 被拖动元素的dom节点                                          |

##### 回调函数的参数中的methods对象说明:

提供一些方法供回调函数调用


- showStateIcon: 显示状态图标<br />例如:<br />
	showStateIcon('add')
	<br />
	<img src="https://raw.githubusercontent.com/qgh810/qgh810.github.io/master/demo/dnd/assets/images/show-add.gif">
	<br />
	showStateIcon('error')<br />
	<img src="https://raw.githubusercontent.com/qgh810/qgh810.github.io/master/demo/dnd/assets/images/show-error.gif">
	<br />
	你也可以传入自己喜欢的图片 例如这样:<br /> showStateIcon('https://ss2.bdstatic.com/lfoZeXSm1A5BphGlnYG/icon/10062.png')<br />
	<img src="https://raw.githubusercontent.com/qgh810/qgh810.github.io/master/demo/dnd/assets/images/show-taobao.gif">
	<br />
	还有 showStateIcon('delete') 和 showStateIcon('reject') 分别出现'删除'图标和'禁止'的图标
	<br />
	<br />

- hideStateIcon: 隐藏状态图标
	<br />

- removeDragedNode: 移除跟随鼠标移动的被拖元素<br />例如:<br />
	removeDragedNode('fade')
	<br />
	<img src="https://raw.githubusercontent.com/qgh810/qgh810.github.io/master/demo/dnd/assets/images/hide-node-fade.gif">
	<br />
	removeDragedNode('back')
	<br />
	<img src="https://raw.githubusercontent.com/qgh810/qgh810.github.io/master/demo/dnd/assets/images/hide-node-back.gif">
	<br />
	removeDragedNode('blost')
	<br />
	<img src="https://raw.githubusercontent.com/qgh810/qgh810.github.io/master/demo/dnd/assets/images/hide-node-blost.gif">
	<br />


- getStateIconNode: 获取跟随鼠标移动的状态图标dom节点


- destroyDrop: 销毁当前Drop对象


| 方法名           | 示例                                                 | 参数说明                                                     | 描述                                                         |
| :--------------- | :--------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| showStateIcon    | params.methods.showStateIcon('add')                  | 参数类型: String, 内置三种常用图标: 添加('add'), 错误('error'), 删除('delete'), 拒绝(reject),传入对应的名字即可, 也可以自定义图标,直接传入图片的完整地址 | 显示状态图标, 调用后会在鼠标旁边出现一个跟随鼠标移动的小图标, 如果要隐藏只需要调用hideStateIcon函数即可 |
| hideStateIcon    | params.methods.hideStateIcon()                       | 无参数                                                       | 隐藏跟随鼠标移动的状态图标(如果没有调用showStateIcon函数的话图标默认不显示) |
| removeDragedNode | params.methods.removeDragedNode(animationName, time) | animationName(动画类型), 参数类型:String, 非必填, 可选: fade / blost / back, time(动画时间, 单位毫秒), 参数类型: Number, 非必填 | 移除跟随鼠标移动的被拖元素, 如果没有参数则直接消失, 有参数表示执行消失动画后再消失 目前支持三种动画, 分别是: 褪色(fade), 爆炸(blost), 反弹(back), 三种动画对应不同也应用场景, 例如: removeDragedNode('blost', 300) |
| getStateIconNode | params.methods.getStateIconNode()                    | 无参数                                                       | 返回跟随鼠标移动的状态图标dom节点                            |
| destroyDrop      | params.methods.destroyDrop({name})                   | 参数类型: String, Drag对象的名字                             | 销毁Drop对象,匹配所有名字跟传入参数一致的Drop对象并销毁, 销毁后将不能接收Drag对象 |

## dnd.js源码

```javascript
(function webpackUniversalModuleDefinition(root, factory) {
	if(typeof exports === 'object' && typeof module === 'object')
		module.exports = factory();
	else if(typeof define === 'function' && define.amd)
		define("dnd", [], factory);
	else if(typeof exports === 'object')
		exports["dnd"] = factory();
	else
		root["dnd"] = factory();
})(this, function() {
return /******/ (function(modules) { // webpackBootstrap
/******/ 	// The module cache
/******/ 	var installedModules = {};
/******/
/******/ 	// The require function
/******/ 	function __webpack_require__(moduleId) {
/******/
/******/ 		// Check if module is in cache
/******/ 		if(installedModules[moduleId])
/******/ 			return installedModules[moduleId].exports;
/******/
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = installedModules[moduleId] = {
/******/ 			i: moduleId,
/******/ 			l: false,
/******/ 			exports: {}
/******/ 		};
/******/
/******/ 		// Execute the module function
/******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
/******/
/******/ 		// Flag the module as loaded
/******/ 		module.l = true;
/******/
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/
/******/
/******/ 	// expose the modules object (__webpack_modules__)
/******/ 	__webpack_require__.m = modules;
/******/
/******/ 	// expose the module cache
/******/ 	__webpack_require__.c = installedModules;
/******/
/******/ 	// identity function for calling harmony imports with the correct context
/******/ 	__webpack_require__.i = function(value) { return value; };
/******/
/******/ 	// define getter function for harmony exports
/******/ 	__webpack_require__.d = function(exports, name, getter) {
/******/ 		if(!__webpack_require__.o(exports, name)) {
/******/ 			Object.defineProperty(exports, name, {
/******/ 				configurable: false,
/******/ 				enumerable: true,
/******/ 				get: getter
/******/ 			});
/******/ 		}
/******/ 	};
/******/
/******/ 	// getDefaultExport function for compatibility with non-harmony modules
/******/ 	__webpack_require__.n = function(module) {
/******/ 		var getter = module && module.__esModule ?
/******/ 			function getDefault() { return module['default']; } :
/******/ 			function getModuleExports() { return module; };
/******/ 		__webpack_require__.d(getter, 'a', getter);
/******/ 		return getter;
/******/ 	};
/******/
/******/ 	// Object.prototype.hasOwnProperty.call
/******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
/******/
/******/ 	// __webpack_public_path__
/******/ 	__webpack_require__.p = "";
/******/
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 7);
/******/ })
/************************************************************************/
/******/ ([
/* 0 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


Object.defineProperty(exports, "__esModule", {
  value: true
});
var DOCUMENT_ADDR = exports.DOCUMENT_ADDR = 'https://github.com/qgh810/dnd';
var REMOVE_ANIMATION_TYPES = exports.REMOVE_ANIMATION_TYPES = {
  'fade': 'animation_fade',
  'blost': 'animation_blost',
  'back': 'animation_back'
};

/***/ }),
/* 1 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


Object.defineProperty(exports, "__esModule", {
  value: true
});
exports.methods = undefined;

var _dragStore;

var _iconImages = __webpack_require__(5);

var _iconImages2 = _interopRequireDefault(_iconImages);

var _config = __webpack_require__(0);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function _defineProperty(obj, key, value) { if (key in obj) { Object.defineProperty(obj, key, { value: value, enumerable: true, configurable: true, writable: true }); } else { obj[key] = value; } return obj; }

var dragStore = (_dragStore = {
  /* ********** 被拖元素drag设置的变量 *************/
  //
  isMobile: false,
  data: null,
  draggedNode: null,
  sourceNode: null,
  markNode: null,
  stateIcon: null,
  mousePosition: null,

  /* ********** drop设置的变量 *************/
  targets: [],
  targetOnDragStarts: [],
  targetOnDragEnds: [],
  onDragEnters: [],
  onDragLeaves: [],
  onDragOvers: [],
  onDrops: [],
  targetPositions: [],

  /* ********** store设置的变量 *************/
  targetIndex: -1,
  _prevValidIndex: -1,
  _inTarget: false,

  /**
   * 监听拖动开始
   */
  onDragStart: function onDragStart(data, el) {
    var _this = this;

    this._initStore();
    this.sourceNode = el;
    this.data = data;
    // 广播拖动开始
    this.targetOnDragStarts.forEach(function (fn, index) {
      return fn && fn({
        data: data,
        enter: index === _this.targetIndex,
        el: _this.targets[index].el,
        sourceNode: _this.sourceNode,
        name: _this.targets[index].name,
        expand: _this.targets[index].expand,
        methods: methods
      });
    });
  },


  /**
   * 初始化store状态
   */
  _initStore: function _initStore() {
    this._inTarget = false;
    this._prevValidIndex = -1;
    this.targetIndex = -1;
    this.hideStateIcon();
  },


  /**
   * 监听拖动
   */
  onDragOver: function onDragOver(pageX, pageY) {
    this.mousePosition = [pageX, pageY];
    this.targetIndex = this.collision(pageX, pageY);
    this.setIconPosition(pageX, pageY);
    if (this.targetIndex >= 0) {
      // 判断是否在目标外 是的话表示刚刚进入
      if (!this._inTarget) {
        this._prevValidIndex = this.targetIndex;
        this._inTarget = true;

        var _params = {
          enter: true,
          data: this.data,
          el: this.targets[this.targetIndex].el,
          sourceNode: this.sourceNode,
          name: this.targets[this.targetIndex].name,
          expand: this.targets[this.targetIndex].expand,
          methods: methods
        };
        this.onDragEnters[this.targetIndex](_params);
      }
      // 调用回调
      var params = {
        enter: true,
        data: this.data,
        el: this.targets[this.targetIndex].el,
        sourceNode: this.sourceNode,
        name: this.targets[this.targetIndex].name,
        expand: this.targets[this.targetIndex].expand,
        pageX: pageX,
        pageY: pageY,
        methods: methods
      };
      this.onDragOvers[this.targetIndex](params);
    } else {
      // 判断是否在目标内  是的话表示刚刚离开
      if (this._inTarget) {
        this._inTarget = false;
        var _params2 = {
          enter: false,
          data: this.data,
          el: this.targets[this._prevValidIndex].el,
          sourceNode: this.sourceNode,
          name: this.targets[this._prevValidIndex].name,
          expand: this.targets[this._prevValidIndex].expand,
          methods: methods
        };
        this.onDragLeaves[this._prevValidIndex](_params2);
      }
    }
  },


  /**
   * 监听拖动结束
   */
  onDragEnd: function onDragEnd() {
    var _this2 = this;

    // 触发放置事件
    if (this.targetIndex >= 0) {
      var params = {
        enter: true,
        data: this.data,
        el: this.targets[this.targetIndex].el,
        sourceNode: this.sourceNode,
        name: this.targets[this.targetIndex].name,
        expand: this.targets[this.targetIndex].expand,
        methods: methods
      };
      this.targetIndex >= 0 && this.onDrops[this.targetIndex](params);
    }

    // 广播拖动结束事件
    this.targetOnDragEnds.forEach(function (fn, index) {
      if (!fn) return;
      var params = {
        enter: index === _this2.targetIndex,
        data: _this2.data,
        el: _this2.targets[index].el,
        sourceNode: _this2.sourceNode,
        name: _this2.targets[index].name,
        expand: _this2.targets[index].expand,
        methods: methods
      };
      fn(params);
    });
  },


  /**
   * 碰撞检测函数
   */
  collision: function collision(pageX, pageY) {
    var targetIndex = -1;
    // 碰撞检测
    for (var i = 0; i < this.targetPositions.length; i++) {
      var position = this.targetPositions[i];
      if (!position) continue;
      var index = i;
      if (pageX >= position.left && pageY >= position.top && pageX <= position.right && pageY <= position.bottom) {
        targetIndex = index;
        break;
      }
    }

    return targetIndex;
  },


  /**
   * 设置状态icon位置跟随鼠标
   */
  setIconPosition: function setIconPosition(x, y) {
    var style = this.stateIcon.style;
    style.left = x + 8 + 'px';
    style.top = y + 'px';
  },


  /**
   * 显示状态icon
   * url 可以是图片绝对路径 也可以是 add | error | delete
   */
  showStateIcon: function showStateIcon(url) {
    var _this3 = this;

    setTimeout(function () {
      if (_this3.isMobile) return console.warn('showStateIcon仅在pc端口可用 请参考相关说明' + _config.DOCUMENT_ADDR);
      url = _iconImages2.default[url] || url || 'add';
      var iconStyle = _this3.stateIcon.style;
      iconStyle.display = 'block';
      iconStyle.background = 'no-repeat url(' + url + ') center center / 100% auto';
    }, 0);
  },


  /**
   * 隐藏状态icon
   */
  hideStateIcon: function hideStateIcon() {
    try {
      this.stateIcon.style.display = 'none';
    } catch (e) {}
  },


  /**
   * 移除被拖动的节点
   * type 动画类型 不传则无动画直接消失 可选 fade | blost | back
   * time 动画持续时长 非必填
   */
  removeDragedNode: function removeDragedNode(type, time) {
    var _this4 = this;

    if (!type) return this.removeMark();
    if (!_config.REMOVE_ANIMATION_TYPES[type]) return this.removeMark();
    setTimeout(function () {
      clearTimeout(_this4.removeMarkTid);
    }, 0);
    this[_config.REMOVE_ANIMATION_TYPES[type]](time);
  }
}, _defineProperty(_dragStore, _config.REMOVE_ANIMATION_TYPES.fade, function () {
  var time = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 150;

  var style = this.draggedNode && this.draggedNode.style;
  if (!style) return;
  style.transition = 'all ' + time / 1000 + 's ease';
  style.opacity = '0';
  setTimeout(this.removeMark.bind(this), time);
}), _defineProperty(_dragStore, _config.REMOVE_ANIMATION_TYPES.blost, function () {
  var time = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 150;

  var style = this.draggedNode && this.draggedNode.style;
  if (!style) return;
  style.transition = 'all ' + time / 1000 + 's ease';
  style.boxShadow = '0 0 50px 30px rgba(0,0,0,0.3)';
  style.opacity = '0';
  setTimeout(this.removeMark.bind(this), time);
}), _defineProperty(_dragStore, _config.REMOVE_ANIMATION_TYPES.back, function () {
  var time = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 400;

  var style = this.draggedNode && this.draggedNode.style;
  if (!style) return;
  style.transition = 'all ' + time / 1000 + 's cubic-bezier(0.2,0.4,0.25,1.1)';
  style.transform = 'translate(0,0)';
  setTimeout(this.removeMark.bind(this), time);
}), _defineProperty(_dragStore, 'removeMark', function removeMark() {
  var _this5 = this;

  clearTimeout(this.removeMarkTid);
  this.removeMarkTid = setTimeout(function () {
    try {
      document.body.removeChild(_this5.markNode);
      _this5.draggedNode = null;
    } catch (e) {
      // console.log('出错', e)
    }
  }, 10);
}), _defineProperty(_dragStore, 'destroyDrop', function destroyDrop(name) {
  var _this6 = this;

  var result = false;
  this.targets.forEach(function (item, i) {
    if (item.name === name) {
      _this6.removeDrop(i);
      result = true;
    }
  });
  return result;
}), _defineProperty(_dragStore, 'removeDrop', function removeDrop(index) {
  delete this.targets[index];
  delete this.targetOnDragStarts[index];
  delete this.targetOnDragEnds[index];
  delete this.onDragEnters[index];
  delete this.onDragLeaves[index];
  delete this.onDragOvers[index];
  delete this.onDrops[index];
  delete this.targetPositions[index];
}), _defineProperty(_dragStore, 'getStateIconNode', function getStateIconNode() {
  return this.stateIcon;
}), _dragStore);

exports.default = dragStore;

/**
 * 供用户调用的静态方法
 */

var methods = exports.methods = {
  showStateIcon: dragStore.showStateIcon.bind(dragStore),
  hideStateIcon: dragStore.hideStateIcon.bind(dragStore),
  getStateIconNode: dragStore.getStateIconNode.bind(dragStore),
  removeDragedNode: dragStore.removeDragedNode.bind(dragStore),
  destroyDrop: dragStore.destroyDrop.bind(dragStore)
};

/***/ }),
/* 2 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


Object.defineProperty(exports, "__esModule", {
  value: true
});

var _typeof = typeof Symbol === "function" && typeof Symbol.iterator === "symbol" ? function (obj) { return typeof obj; } : function (obj) { return obj && typeof Symbol === "function" && obj.constructor === Symbol && obj !== Symbol.prototype ? "symbol" : typeof obj; };

exports.checkNode = checkNode;
function checkNode(el) {
  var result = el;
  if (!result) {
    return console.error('找不到当前节点', el);
  }
  if (typeof el === 'string') {
    var domName = el;
    result = document.querySelector(domName);
    if (!result) {
      return console.error('找不到当前节点', el);
    }
  } else if ((typeof el === 'undefined' ? 'undefined' : _typeof(el)) === 'object') {
    if (!el.nodeName) {
      return console.error('找不到当前节点', el);
    }
  }
  return result;
}

/***/ }),
/* 3 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


var _createClass = function () { function defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } } return function (Constructor, protoProps, staticProps) { if (protoProps) defineProperties(Constructor.prototype, protoProps); if (staticProps) defineProperties(Constructor, staticProps); return Constructor; }; }();

var _check = __webpack_require__(2);

var _store = __webpack_require__(1);

var _store2 = _interopRequireDefault(_store);

__webpack_require__(6);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function _toConsumableArray(arr) { if (Array.isArray(arr)) { for (var i = 0, arr2 = Array(arr.length); i < arr.length; i++) { arr2[i] = arr[i]; } return arr2; } else { return Array.from(arr); } }

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Drag = function () {
  function Drag(el, options) {
    _classCallCheck(this, Drag);

    this.initData(el, options) && this.init();
  }

  /**
   * 检查和初始化传入参数
   */


  _createClass(Drag, [{
    key: 'initData',
    value: function initData(el, options) {
      this.el = (0, _check.checkNode)(el);
      if (!this.el) return;

      this.el.style.MozUserSelect = 'none'; // 兼容火狐
      this.el.style.userSelect = 'none';
      this.el.style.cursor = 'default';

      options = this.checkOptions(options);
      this.options = options;
      this.data = options.data;
      this.mouseDownPosition = { left: -1, top: -1 };
      this.mouseDragging = false;
      this.mark = null;
      this.position = { left: 0, top: 0 };
      return true;
    }

    // 初始化

  }, {
    key: 'init',
    value: function init() {
      this.addEventListenerPC();
      this.addEventListenerMobile();
    }

    /**
     * 事件监听
     */

  }, {
    key: 'addEventListenerPC',
    value: function addEventListenerPC() {
      var dom = this.el;
      // 监听当前节点的鼠标点击事件
      function onMouseDowm(e) {
        var pageX = e.pageX,
            pageY = e.pageY;

        this.mouseDownPosition = { left: pageX, top: pageY };
        dom.onmousemove = this.onElMousemove.bind(this);
        dom.onmouseup = this.onElMouseUp.bind(this);
        document.addEventListener('mouseup', this.onElMouseUp.bind(this));
      }
      dom.addEventListener('mousedown', onMouseDowm.bind(this));
    }
  }, {
    key: 'addEventListenerMobile',
    value: function addEventListenerMobile() {
      // 兼容移动端
      this.el.addEventListener('touchmove', this.onElTouchMove.bind(this));
      this.el.addEventListener('touchend', this.onElTouchEnd.bind(this));
    }

    /**
     * 监听拖动开始
     */

  }, {
    key: 'onElMousemove',
    value: function onElMousemove(e) {
      var pageX = e.pageX,
          pageY = e.pageY;
      var _mouseDownPosition = this.mouseDownPosition,
          left = _mouseDownPosition.left,
          top = _mouseDownPosition.top;

      var EMIT_LENGTH = 3;
      if (Math.abs(pageX - left) < EMIT_LENGTH && Math.abs(pageY - top) < EMIT_LENGTH) return;
      if (this.mouseDragging) return;
      this.mouseDragging = true;

      _store2.default.onDragStart(this.data, this.el);
      this.position = this.el.getBoundingClientRect();
      // 创建蒙层
      this.mark = document.createElement('div');
      this.mark.className = 'x-drag-mark';
      this.setMarkStyle();
      this.mark.onmousemove = this.onMarkMouseMove.bind(this);
      this.mark.onmouseup = this.onMarkMouseUp.bind(this);
      this.mark.onmouseleave = this.onMarkMouseUp.bind(this);
      _store2.default.markNode = this.mark;
      document.body.appendChild(this.mark);
      // 创建复制元素
      _store2.default.draggedNode = this.el.cloneNode(true);
      this.setCloneNodeStyle();
      this.mark.appendChild(_store2.default.draggedNode);

      // 创建状态icon
      _store2.default.stateIcon = document.createElement('i');
      _store2.default.stateIcon.className = 'x-state-icon';
      this.setIconStyle();
      this.mark.appendChild(_store2.default.stateIcon);

      this.emit('onDragStart', {
        el: this.el,
        data: this.data,
        methods: _store.methods
      });
    }

    /**
     * 监听拖动结束
     */

  }, {
    key: 'onElMouseUp',
    value: function onElMouseUp() {
      this.mouseDragging = false;
      this.el.onmousemove = null;
      this.el.onmouseup = null;
      this.mark && (this.mark.onmousemove = null);
      this.mark && (this.mark.onmouseup = null);
      _store.methods.hideStateIcon();
      _store.methods.removeDragedNode();
      document.removeEventListener('mouseup', this.onElMouseUp.bind(this));
    }

    /**
     * 监听蒙层鼠标移动
     */

  }, {
    key: 'onMarkMouseMove',
    value: function onMarkMouseMove(e) {
      if (!_store2.default.draggedNode) return;

      var _ref = e.touches && e.touches[0] || e,
          pageX = _ref.pageX,
          pageY = _ref.pageY;

      var translateX = pageX - this.mouseDownPosition.left;
      var translateY = pageY - this.mouseDownPosition.top;
      _store2.default.draggedNode.style.transform = 'translate(' + translateX + 'px,' + translateY + 'px)';
      _store2.default.onDragOver(pageX, pageY);
    }

    /**
     * 监听蒙层鼠标放开
     */

  }, {
    key: 'onMarkMouseUp',
    value: function onMarkMouseUp() {
      document.removeEventListener('mouseup', this.onElMouseUp.bind(this));
      this.mouseDragging = false;
      this.mark.onmousemove = null;
      this.el.onmousemove = null;
      this.mouseDownPosition = { left: -1, top: -1 };

      this.emit('onDragEnd', {
        el: this.el,
        data: this.data,
        target: _store2.default.targets[_store2.default.targetIndex],
        methods: _store.methods
      });
      _store2.default.onDragEnd();
    }
  }, {
    key: 'onElTouchMove',
    value: function onElTouchMove(e) {
      e.preventDefault();
      _store2.default.isMobile = true;
      if (this.mouseDownPosition.left === -1) {
        var _e$touches$ = e.touches[0],
            pageX = _e$touches$.pageX,
            pageY = _e$touches$.pageY;

        this.mouseDownPosition.left = pageX;
        this.mouseDownPosition.top = pageY;
      }
      this.onElMousemove();
      this.onMarkMouseMove(e);
    }
  }, {
    key: 'onElTouchEnd',
    value: function onElTouchEnd(e) {
      this.onMarkMouseUp();
    }

    /**
     * 检查并且初始化options
     */

  }, {
    key: 'checkOptions',
    value: function checkOptions(options) {
      options = options || {};
      var baseOptions = {
        data: '这里可以放需要丢给目标的内容',
        el: this.el,
        removeanimationtype: 1
      };
      for (var option in baseOptions) {
        !options[option] && (options[option] = baseOptions[option]);
      }
      return options;
    }

    /**
     * 设置蒙层样式
     */

  }, {
    key: 'setMarkStyle',
    value: function setMarkStyle() {
      var markStyle = {
        position: 'absolute',
        top: '0',
        left: '0',
        width: '100%',
        height: '100%',
        zIndex: '10'
      };
      for (var style in markStyle) {
        this.mark.style[style] = markStyle[style];
      }
    }

    /**
     * 设置克隆节点样式
     */

  }, {
    key: 'setCloneNodeStyle',
    value: function setCloneNodeStyle() {
      var dom = _store2.default.draggedNode;
      var style = dom.style;
      var _position = this.position,
          left = _position.left,
          top = _position.top;

      var _el$getBoundingClient = this.el.getBoundingClientRect(),
          width = _el$getBoundingClient.width,
          height = _el$getBoundingClient.height;

      style.position = 'absolute';
      style.left = left + 'px';
      style.top = top + 'px';
      style.width = width + 'px';
      style.height = height + 'px';
      style.textAlign = this.el.currentStyle.textAlign;
      style.transform = 'translate(0,0)';
      style.zIndex = 1000;
      style.margin = 0;
    }

    /**
     * 设置状态icon样式
     */

  }, {
    key: 'setIconStyle',
    value: function setIconStyle() {
      var style = _store2.default.stateIcon.style;
      style.display = 'none';
      style.position = 'absolute';
      style.width = '20px';
      style.height = '20px';
      style.zIndex = '10001';
    }

    /**
     * 发布事件
     */

  }, {
    key: 'emit',
    value: function emit() {
      var _options;

      var args = Array.from(arguments);
      var functionName = args.shift();
      typeof this.options[functionName] === 'function' && (_options = this.options)[functionName].apply(_options, _toConsumableArray(args));
    }
  }]);

  return Drag;
}();

module.exports = Drag;

/***/ }),
/* 4 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


var _createClass = function () { function defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } } return function (Constructor, protoProps, staticProps) { if (protoProps) defineProperties(Constructor.prototype, protoProps); if (staticProps) defineProperties(Constructor, staticProps); return Constructor; }; }();

var _check = __webpack_require__(2);

var _config = __webpack_require__(0);

var _store = __webpack_require__(1);

var _store2 = _interopRequireDefault(_store);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function _toConsumableArray(arr) { if (Array.isArray(arr)) { for (var i = 0, arr2 = Array(arr.length); i < arr.length; i++) { arr2[i] = arr[i]; } return arr2; } else { return Array.from(arr); } }

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Drop = function () {
  function Drop(el, options) {
    _classCallCheck(this, Drop);

    this.initData(el, options) && this.init();
  }

  /**
   * 检查并初始化传入参数
   */


  _createClass(Drop, [{
    key: 'initData',
    value: function initData(el, options) {
      this.el = (0, _check.checkNode)(el);
      if (!this.el) return;
      this.options = this.checkOptions(options);
      if (!this.options) return;
      this.methods = _store.methods;
      this.index = -1; // 当前索引
      return true;
    }

    /**
     * 检查并且初始化options
     */

  }, {
    key: 'checkOptions',
    value: function checkOptions(options) {
      if (!options) {
        return console.error(this.el, '未检测到options 请参考相关说明' + _config.DOCUMENT_ADDR);
      }
      if (typeof options.onDrop !== 'function') {
        return console.error(this.el, 'onDrop 必须是一个函数 请参考相关说明' + _config.DOCUMENT_ADDR);
      }
      var baseOptions = {
        name: null
      };
      for (var option in baseOptions) {
        !options[options] && (options[options] = baseOptions[option]);
      }
      return options;
    }

    /**
     * 初始化
     */

  }, {
    key: 'init',
    value: function init() {
      this.setStore();
    }

    /**
     * 托管状态
     */

  }, {
    key: 'setStore',
    value: function setStore() {
      var index = _store2.default.targets.push({
        el: this.el,
        name: this.options.name,
        expand: this.options.expand
      }) - 1;
      this.index = index;

      _store2.default.targetOnDragStarts[index] = this.onDragStart.bind(this);
      _store2.default.targetOnDragEnds[index] = this.onDragEnd.bind(this);
      _store2.default.onDragEnters[index] = this.onDragEnter.bind(this);
      _store2.default.onDragOvers[index] = this.onDragOver.bind(this);
      _store2.default.onDragLeaves[index] = this.onDragLeave.bind(this);
      _store2.default.onDrops[index] = this.onDrop.bind(this);
    }

    /**
     * 目标监听到拖动开始
     */

  }, {
    key: 'onDragStart',
    value: function onDragStart(params) {
      this.setStorePositions();
      this.emit('onDragStart', params);
    }

    /**
     * 目标监听到拖动结束
     */

  }, {
    key: 'onDragEnd',
    value: function onDragEnd(params) {
      this.emit('onDragEnd', params);
    }

    /**
     * 目标监听到拖动进入当前范围
     */

  }, {
    key: 'onDragEnter',
    value: function onDragEnter(params) {
      this.emit('onDragEnter', params);
    }

    /**
     * 目标监听在自己上方拖动
     */

  }, {
    key: 'onDragOver',
    value: function onDragOver(params) {
      this.emit('onDragOver', params);
    }

    /**
     * 目标监听到离开当前范围
     */

  }, {
    key: 'onDragLeave',
    value: function onDragLeave(params) {
      this.emit('onDragLeave', params);
    }

    /**
     * 目标监听到被拖动元素在自己范围内放下
     */

  }, {
    key: 'onDrop',
    value: function onDrop(params) {
      this.emit('onDrop', params);
    }

    /**
     * 托管当前位置信息
     */

  }, {
    key: 'setStorePositions',
    value: function setStorePositions() {
      var _el$getBoundingClient = this.el.getBoundingClientRect(),
          left = _el$getBoundingClient.left,
          top = _el$getBoundingClient.top,
          width = _el$getBoundingClient.width,
          height = _el$getBoundingClient.height;

      _store2.default.targetPositions[this.index] = {
        top: top,
        bottom: top + height,
        left: left,
        right: left + width
      };
    }

    /**
     * 发布事件
     */

  }, {
    key: 'emit',
    value: function emit() {
      var _options;

      var args = Array.from(arguments);
      var functionName = args.shift();
      typeof this.options[functionName] === 'function' && (_options = this.options)[functionName].apply(_options, _toConsumableArray(args));
    }
  }]);

  return Drop;
}();

module.exports = Drop;

/***/ }),
/* 5 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


Object.defineProperty(exports, "__esModule", {
  value: true
});
var IMAGES = {
  'add': 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6RkUxNENCODcwMjdFMTFFNzhDQjhBODQ4QzIwNTVBM0QiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6RkUxNENCODgwMjdFMTFFNzhDQjhBODQ4QzIwNTVBM0QiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpGRTE0Q0I4NTAyN0UxMUU3OENCOEE4NDhDMjA1NUEzRCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpGRTE0Q0I4NjAyN0UxMUU3OENCOEE4NDhDMjA1NUEzRCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PsH+M4sAAAPNSURBVHja5FrPS1RRGP3mOuqMQS5EpI0tApEiRVq5UZBAbBW06QdUWuTGRYt+gH+AULZo0aaosYK0TdAqCUJw5SokoxDBRW5CBiGDdGz0Td+Z7kz3Xe8b5715M16n8zgwzLu+d473u/d+97sTyWQyFAKizMfMPmYLs54pPNo6zC3mKnOGOczcLlVApEQjk8wBZiOeFfAZELDOnGZerLSRWWY3s5bCRZo5x+z1+4fCZ/sEM8XsKYMJks/ske9IlKNHMAaWma1eDb5sfqaFjXl6vfaK0s5vHgiOx39OUK2oo/NNl6ijoYtOxE8Weu8K81gxY6gYI0PMR8y4fmNx8yt9YvHPk0850J1gsc3GrjZfp0421R4/bmqyyRzZq4f2MjLGvC17JI+l1CLN//pIE8knocbVYPMN6jp0itpi7fot9Mg4czSIEZi4w6xRv0QP3F25SeXEvdaH2R7SsMO872XGy8iQXBdcPTG19pJeJJ9RJXCl+RpdaLps6plhU5iZjED8T31MPPg+Rh/W31Mlcbqxn24dGTWNmcP6BGCafpdtMAHgnXi3hrjUWHAdSehTLMJpP0yoZqBBQ6seXnpopWSeFMrAftP2jhpEQ/bzhrNB55bOhDkBIF+LmXpkVjWBKbbU2amGr4i8atyTn29ACzQpqJeadxnpVlthnbANBk3dupFJNXfCih32YhcGoAnatNxsUjUyoC96tsKgbSBnJCr3E3kgd7IVBm3QHhVyBY+oWWzQBLASgDZodOWd7EHI7WkeCxaHVQGNfULusfPAfsJ2GDS2CHXtyO41eVNkOwwa64WepjgWj48CGkU0SNpR7CodEzHX57dtxeVsO3z5TWd8G0HuFAlY+VGNFZ6Z/Fd2BFUJfPcIstggoZVNrZ1U0aFVdiN+YhdjImcGJs4u9ZetRwSRewoQByDaDBodITco/9JJUWe9EYPGLRhZVb9BBdB2GDSuwsiM+k3H7nqSdTBonBGyTpSfuFGLjVg8TqBNqxdD+7CQ9aF19Q5qsbbCoA3at3P/+mn1TqfF4WXQNq2u7DgpSufuoCqOgrJtgCatYp+W2l2DYU5tgaq4bTBomjPlWr3qmoLSPopipQCpRkZeQdIOFdCiHTdskXJEp1caUYYcVL+oZAXeCx6V+Qn6e2pgNAJ8I63+u19FbMCjIo8juaOuabmajxXQYERviAeiiysZTgYTRm1ePZJDVRy9qWYO/GFoDlVxPK1OAFb/YIBgxAcTzFSm/EjJdxWt7b/9UY2azmC/OcX8oe5nAiAjnzEln9kbaKxVyw/P/ggwAL2hTh2cIlwtAAAAAElFTkSuQmCC',

  'error': 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6RUZGQzQzNkEwMjdGMTFFNzhDQjhBODQ4QzIwNTVBM0QiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6RUZGQzQzNkIwMjdGMTFFNzhDQjhBODQ4QzIwNTVBM0QiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpGRTE0Q0I4OTAyN0UxMUU3OENCOEE4NDhDMjA1NUEzRCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpGRTE0Q0I4QTAyN0UxMUU3OENCOEE4NDhDMjA1NUEzRCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PmdTj/IAAAXZSURBVHja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',

  'delete': 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6RTI0NEY4N0EwQzc0MTFFN0E2RDZCRkFGNDU1MkQ0ODUiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6RTI0NEY4NzkwQzc0MTFFN0E2RDZCRkFGNDU1MkQ0ODUiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpFRkZDNDM2QTAyN0YxMUU3OENCOEE4NDhDMjA1NUEzRCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpFRkZDNDM2QjAyN0YxMUU3OENCOEE4NDhDMjA1NUEzRCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PlQqDSEAAAZ9SURBVHja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',

  'reject': 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAMAAAAp4XiDAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6M0RGNkFBRkUwRDc1MTFFNzkxMzJCNEQxMzA3NjUzRkMiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6M0RGNkFBRkQwRDc1MTFFNzkxMzJCNEQxMzA3NjUzRkMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpGRTE0Q0I4NzAyN0UxMUU3OENCOEE4NDhDMjA1NUEzRCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpGRTE0Q0I4ODAyN0UxMUU3OENCOEE4NDhDMjA1NUEzRCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/Pl4Q6p4AAAKRUExURf///wAAANzc3AEBATs7O6SkpP39/UU0NEIyMnh4eIWFhSAgIPj4+JaWljY2NikpKampqXJfXxkZGfv7++/v7zo6OoiIiJWVlb+2tjU1NWVlZW9cXN3d3RAQECMYGFZWVhwcHAcHBx0dHQICAhQUFA0NDSodHRISEjssK+bm5tbW1gsLC21tbWBgYENDQ19MTH19fc3NzTUmJre3t+zs7GJiYsvGxgoKCnBwcMvFxYmJiWZmZrS0tCEXFufn52hoaNfX1ygoKKGhoVVVVT8vLyMjI9nZ2Q8PD3NzcyYmJjMzMywsLAkJCQ4ODgMDA4aGht/f38e/v7+/v7GxsZGRkbi4uCUlJcfDwu7u7v76+jIyMi0tLeXl5UFBQWZSUs7IyJmZmSoqKiEWFnJhX729vTEvLycbG8zMzKWlpYqKimpXV87OzsDAwO3t7ePj43FxcdLPzwwMDFA+Pr+1tTk5OV1JSaOjo9rZ2cfBwVRCQcfCwnl5efLy8qampvDw8OLi4j0uLT0tLdra2sG4uM/LyxgYGHx8fLq6uiEhIRcXF0dHR0JCQkVFRYODg2RkZE9PT05OTtTS0lRBQY6OjsfAwJycnMvGxTEkI9TR0TAwMBUVFVBQUNPQ0FhGRkk4OGxsbGZTU8PDw3p6em9vb6qqqm9sbJiYmJOTk97e3peXl4yMjHRhYdLS0vX19UhISP34+Nvb27u7u8TExGdnZ1lZWVFRUVRUVNXV1RoaGv78/Gtraz0uLoCAgMXFxXd3d/r6+pKSkkk3Nx8VFdLOzy8hISIiIlA+PfT09Pz8/PHx8ScnJxYWFjIkI+vr6z4+PvPz8zw8PGlpaT09PV5eXkExMUZGRpSUlPf390tLS1hYWP///xcBUUkAAADbdFJOU///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////ACDO6IAAAAMvSURBVHjahJYFW1sxFIZPbkuhtKUdRYb7GLbBYLgzGLAxGMzd3d3d3d3d3d3dff01i1Vv0uV57s3Nd/I2pycnAjb38jD71vpAFItQAkLIkIb2ZYQaU9y7gGuj1HavCnVGHqVdUZMUqantiISlQt8iRlLNgbyLARmCUbA/du8dV5IKFwiQ23e4ub8JFgO8AYAzn+EKHzh+oRpJW8scX7YI3IsxM5EyY3M8ENSN6NXnQFRC6K+Z3BE2+nWQFMJozLOMLsh+SpwEadHSDo2PHUh5A1V8FSniwyJpsSOR1TxWo6bLGTpOoimIIZ8cc9ZBPg74kw5rmhlCvsMPUmZprnemawtFDuPPL1BwiDLRB+RMPbbHTMTIBdJTD6C0+l8MdhJ7GUbKcD2DKJc7UWZYaymzGZtXloItDtdjqKKcp0xb6TgpZEZDYQCuAu3aKxa37hJER5AHUET+k11bMZMybRQvSbAFyCJ84hSPeI0BSYI0IBlsdYoFfpR5tkPuGZD5cVWV4ZTxU6SeEeSnu+zL/o8w1nqOpLvLsyMo80E0TjFHaj0N7ZlvU9TIBI5M9jT0Zr5FP1chVznyVWXh+fZU5VsPjoSpHejL8i3aMwZVBNHgl07NKBHCfCObIyThV6UontMYM0c9lX/JkhQhg7ep860ZC3mQit/vhTOtjFPl2zHcvgl96nB1TcjkLqFMiXMtdMHNnhD0CFc3xInLY+3IN5LIUTqwbSfqL8kCYXP6YjltWM34ewNe++MD8IdWguwZRJndin0eNVlkU5pEpua0bL3zfNsKUE7+ySq29Q0kor8EmcfWXMku5RupUxhyIsobw2PwI4y85/NtPKgwmTS1PmLm7n3Hnh3uOCzAYqCKhOH5hlAGOI8kayOVtDqJc3Op+Q+4Hny/Kys0cudGj6QIuB+vcJYPrmaOBiBXwnmIfwyJZ5a9Q95exGkO9MmJzI9jcgKorwpgMcdp+FD9tNhF/Ax1xGoTiC4kr03B68R3GHRcL7spZa0OSJiq6h+WWBwpvVzZbE2n0qMCkvJw+iH6JKPvl1JHeLmPkWLMtsToM+uSY+sbXub32hhaY/Xs8U+AAQBejshIcsznyAAAAABJRU5ErkJggg=='
};
exports.default = IMAGES;

/***/ }),
/* 6 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


HTMLElement.prototype.__defineGetter__('currentStyle', function () {
  return this.ownerDocument.defaultView.getComputedStyle(this, null);
});

/***/ }),
/* 7 */
/***/ (function(module, exports, __webpack_require__) {

"use strict";


Object.defineProperty(exports, "__esModule", {
  value: true
});
exports.Drop = exports.Drag = undefined;

var _drag = __webpack_require__(3);

var _drag2 = _interopRequireDefault(_drag);

var _drop = __webpack_require__(4);

var _drop2 = _interopRequireDefault(_drop);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

exports.Drag = _drag2.default;
exports.Drop = _drop2.default;

/***/ })
/******/ ]);
});
```

