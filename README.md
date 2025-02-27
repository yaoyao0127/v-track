# v-track

<a href="https://travis-ci.org/l-hammer/v-track"><img alt="Travis (.org) branch" src="https://img.shields.io/travis/l-hammer/v-track/master.svg?logoColor=%23666666&style=flat-square"></a>
<a href="https://unpkg.com/v-track/dist/v-track.min.js"><img src="https://img.badgesize.io/https://unpkg.com/v-track/dist/v-track.min.js?compression=gzip&style=flat-square" alt="Gzip Size" /></a>
<a href="https://www.npmjs.com/package/v-track"><img src="https://img.shields.io/npm/v/v-track.svg?colorB=brightgreen&style=flat-square"></a>
<a href="https://github.com/l-hammer/v-track/tree/master/tests/unit"><img src="https://img.shields.io/badge/tested_with-jest-99424f.svg?style=flat-square"></a>
<a href="http://hits.dwyl.io/l-hammer/v-track" alt="hit count"><img src="http://hits.dwyl.io/l-hammer/v-track.svg" /></a>

v-track通过 Vue [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)的方式将埋点代码与业务代码完全解耦~

## 安装

### YARN

```shell
$ yarn add v-track
```

### NPM

```shell
$ npm install v-track --save
```

### CDN

目前可以通过[unpkg.com/v-track](https://unpkg.com/v-track/)获取到最新版本的资源，在页面上使用 script 标签直接引入文件即可开始使用

```html
<script src="https://unpkg.com/v-track/dist/v-track.min.js"></script>
```

或者

```html
<script src="https://cdn.jsdelivr.net/npm/v-track/dist/v-track.min.js"></script>
```

> 建议使用 CDN 引入 v-track 的用户在链接地址上锁定版本，以免将来 v-track 升级时受到非兼容性更新的影响。锁定版本的方法请查看 [unpkg.com](https://unpkg.com/) or [jsdelivr.com](https://www.jsdelivr.com/)。

## 用法

```js
import Vue from "vue"
import VTrack from "v-track"
import trackEvents from "./track-events"

Vue.use(VTrack, {
  trackEvents, // 埋点事件对象
  trackEnable: {
    UVPV: true, // 是否开启UVPV统计，v0.8.3新增routeUpdate，即在当前路由参数发生改变时埋点，默认为false
    TONP: true // 是否开启页面停留时长统计，默认为false
  }
})
```

```js
/* track-events.js */
export default {
  /**
   * @name UVPV 固定名称不支持修改
   * @desc UV、PV埋点
   * @param {Object} context 当前上下文
   */
  UVPV(context) {
    ...
  },
  /**
   * @name TONP 固定名称不支持修改
   * @desc 页面停留时间埋点（Time on Page）
   * @param {Object} context 当前上下文
   * @param {Timestamp} et 进入页面时间
   * @param {Timestamp} dt 离开页面时间
   */
  TONP(context, { et, dt }) {
    ...
  },
  /**
   * @name 18015 埋点唯一标识ID（自定义）
   * @param {Object} context 当前上下文
   * @param {Object} args 剩余参数
   */
  18015(context, args) {
    ...
  }
  ...
}
```

```HTML
<!-- 页面行为埋点（track-view为v-track全局注册的组件） -->
<track-view v-track:18015></track-view>
<track-view v-track:18015.watch="{ rest }"></track-view>
<track-view v-track:18015.watch.delay="{ rest }"></track-view>
<track-view v-if="rest" v-track:18015></track-view>

<!-- 事件行为埋点（DOM） -->
<div v-track:18015.click="handleClick"></div>
<div v-track:18015.click="{ handleClick, item, index }"></div>
<div v-track:18015.click.async="{ handleSearch, rest }"></div>
<div v-track:18015.click.delay="handleClick"></div>

<!-- 事件行为埋点（组件） -->
<cmp v-track:18015.click="handleClick"></cmp>
<cmp v-track:18015.[自定义事件名]="handleSearch"></cmp>
<cmp v-track:18015.[自定义事件名].delay="handleSearch"></cmp>
<cmp v-track:18015.[自定义事件名].async="{ handleSearch, rest }"></cmp>

<!-- 区域展现埋点（block 可以是 DOM 或者组件） -->
<block v-track:18015.show></block>
<block v-track:18015.show.once></block>
<block v-track:18015.show.custom="{ ref: 'scroll' }"></block>
<block v-track:18015.show.custom.once="{ ref: 'scroll' }"></block>
```

## 指令修饰符

- `.click` 表示事件行为的埋点
- `.[custom-event]` 表示自定义事件行为的埋点
- `.native` 表示监听组件原生click事件行为的埋点
- `.watch` 表示页面异步行为的埋点
- `.async` 配合`.click`指令，表示异步事件行为的埋点
- `.delay` 表示埋点是否延迟执行，默认先执行埋点再执行回调
- `.show` 表示区域曝光埋点
- `.once` 配合`.show`指令，只执行一次埋点
- `.custom` 配合`.show`指令，表示使用自定义scroll事件

## LICENSE

[MIT](https://github.com/l-hammer/v-track/blob/master/LICENSE) © 2019-present, LHammer
