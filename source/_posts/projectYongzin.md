---
title: 搜索引擎迭代
date: 2023-07-18
tags: [前端项目, 前端开发]
---

- 背景：

    业务架构太老了，并且视觉不够现代美观，需要技术迭代。

- 需求功能结构图：

- 记录：

1. 搜索框

- 父子组件间通信（跳转至不同的搜索结果页面）
    父->子：`props`：在子组件中使用`defineProps()`来声明，从而接收参数

```vue
<!-- 子组件 -->
<script>
    const props = defineProps(["searhToPage", "myFun", "default"]);
    const searhToPage = props.searhToPage;
</script>
<!-- 父组件 -->
<template>
    <SearchInput :searhToPage="searhToPage" />
</template>

<script>
const searhToPage = (word) => {
  console.log(word);
  if (word) {
    router.push(`/search/webres?keyword=${word}&order=1&pageNo=1&pageSize=10&sortType=1`);
  }
};
</script>
```

- 历史搜索记录

    将历史记录数组、对历史记录的增删查操作存储在pinia的store中，以实现持久化

```javascript
import { defineStore } from "pinia";
import { ref } from "vue";
import {deleteHistoryXhr, getHistoryXhr, addHistoryXhr } from "../api/news"
// search 为在store中的id
export const useSearchStore = defineStore(
  "search",
  // 传入一个函数，该函数定义了一些响应式属性和方法，并且返回一个带有我们想暴露出去的属性和方法的对象
  () => {
    // ref() 就是 state 属性
    const history = ref([]);

    // computed() 就是 getters
    // function() 就是 actions
    const addHistory = async (val) => {
      await addHistoryXhr(val)
      await loadHistory()
    };

    const setHistory = (arr) => {
      history.value = arr;
    };

    const loadHistory = async () => {
      const res = await getHistoryXhr();
      history.value = res.data;
      history.value.reverse();
      history.value = history.value.slice(0,3);
    };

    const clearOneHistory = async (userId, id) => {
      await deleteHistoryXhr(userId, id);
      await loadHistory();
    };

    const clearAllHistroy = () => (history.value = []);

    return {
      history,
      addHistory,
      clearOneHistory,
      clearAllHistroy,
      setHistory,
      loadHistory
    };
  },
  {
    persist: {
      enabled: true,
      strategies: [
        {
          key: "searchHistroy",
          storage: localStorage,
          paths: ["history"],
        },
      ],
    },
  }
);
```

    使用
```js
import { useSearchStore } from "../store/search";
const searchStore = useSearchStore();
// `history` 被提取为 ref, 并且会跳过所有的 action 或非响应式 (不是 ref 或 reactive) 的属性
const { history } = storeToRefs(searchStore);
const { addHistory, clearOneHistory, setHistory, loadHistory } = searchStore;
// 之后就可以调用了
```

- 输入框获取焦点后在下方显示历史搜索记录和热搜栏。但是移动到下拉框时，输入框会失去焦点然后下拉框关闭。 `focus` 事件在元素获取焦点时触发, `blur` 事件在一个元素失去焦点的时候被触发

```vue
<template>
    <input
        type="text"
        @focus="showPreview"
        @blur="hidePreview"
        class="searchInput"
        v-bind:value="searchInput"
        v-on:input="perview"
        ref="SIDom"
    />
    <div class="searchPreview" v-auto-animate v-if="isPreview">
</template>
<script>
    const isPreview = ref(false);
    const showPreview = () => (isPreview.value = true);
</script>
```

    所以在输入框失去焦点后需要跟踪鼠标去向。如果鼠标移动到下拉框内部，就可以设置`SIDom.value.focus();`将输入框设置为获取焦点的状态。
    
    具体而言就是使用`EventTarget.addEventListener()`将指定的监听器注册到 EventTarget 上，当该对象触发指定的事件时，指定的回调函数就会被执行。

```js
const hidePreview = () => {
  if (inSearchBox.value == true) {
    SIDom.value.focus();
  } else {
    isPreview.value = false;
  }
};
// 监听鼠标，检查是否在搜索框内
const checkPointer = () => {
  // 获取要检查的元素
  const element = document.querySelector(".searchBox");

  // 添加鼠标移动事件监听器
  document.addEventListener("mousemove", (event) => {
    // 获取鼠标的坐标位置
    const mouseX = event.clientX;
    const mouseY = event.clientY;

    // 获取元素的位置和尺寸信息
    const elementRect = element.getBoundingClientRect();

    // 检查鼠标是否在元素范围内
    if (
      mouseX >= elementRect.left &&
      mouseX <= elementRect.right &&
      mouseY >= elementRect.top &&
      mouseY <= elementRect.bottom
    ) {
      // 鼠标在元素范围内
      inSearchBox.value = true;
    } else {
      // 鼠标不在元素范围内
      inSearchBox.value = false;
    }
  });
};
```

2. GASP动画

- 初始载入的效果。首页搜索框上方有网站logo，下方有信息流推送，在往下滚动时，logo的位置需要从输入框上方变到输入框左方，并且改变大小。就做了一个从左到右的滑入效果。

```js
const gsapEnevt = () => {
  // 最初出现的动画。透明度变化，且存在时间差
  gsap
    .timeline()
    .from("header", {
      duration: 1,
      opacity: 0,
      y: 20,
    })
    .from(".searchLogo, .searchPosition", {
      duration: 1.2,
      opacity: 0,
      y: 20,
    });

  // 鼠标滑动触发动画
  ScrollTrigger.create({
    trigger: "header",
    start: "top top",
    end: `+=350rem`,
    scrub: true,
    // markers: true,
    // pin: true,
    animation: gsap
      .timeline()
      .to(".menus", { opacity: 0, y: "-1rem" }, "<")
      .to(".searchLogo", { top: "-7rem" }, "<")
      .to(".search-body", { top: "0.5rem" }, "<"),
      // .to(".searchNow", { height: "3rem" }, "<"),
    onEnterBack: () => {
      miniVisible.value = false;
    },
    onLeave: () => {
      miniVisible.value = true;
      miniVisible.value = true;
      // const timer = setTimeout(() => {
      //   if (document.querySelector(".mini-searchLogo")) {
      //     gsap
      //       .timeline({ fps: 60 })
      //       .from(".mini-searchLogo", { x: -200, duration: 2 })
      //       .to(".mini-searchLogo", { opacity: 1, duration: 1 }, "<");
      //     clearInterval(timer);
      //   }
      // }, 2);
    },
    onEnter: () => {
      gsap.timeline().to(".nr", { opacity: 1, duration: 1 });
    },
  });
};

onMounted(() => {
  gsapEnevt();
});
```

3. QQ扫码登录，微信扫码登录，短信验证码，用户表单验证

扫码登录流程：去微信开放平台申请网站应用（`https://open.weixin.qq.com/connect/qrconnect?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect `） -> 后端封装接口，服务器存储appid -> 外部扫码，跳转新窗口；内部扫码，引入官方js文件。前端为icon设置`window.open`,填入`redirect_uri`（需要encodeURIComponent）`client_id``response_type`等参数信息 ->  重定向的vue文件里，判断`code`和`state` -> 再调用后端接口（通过code获取access_token再调用）会返回一个code -> 根据code判断是否已经完成注册 -> 跳转到注册页面 / 登录成功重定向到之前页面 

```
const QQLogin = () => {

  const api =
    "https://graph.qq.com/oauth2.0/show?which=Login&display=pc&client_id=101648029&redirect_uri=https://user.yongzin.com/authority/qqloginController/authuserQQLogin.do&response_type=code&state=a940b6f7cbe8ae22a58e502a3b300120&scope=get_user_info,add_topic,add_one_blog,add_album,upload_pic,list_album,add_share,check_page_fans,add_t,add_pic_t,del_t,get_repost_list,get_info,get_other_info,get_fanslist,get_idollist,add_idol,del_ido,get_tenpay_addr";
  window.open(api, "_blank");
};
```
4. 图片瀑布流

- 使用组件`vue-waterfall2`相关属性有：`col`多少列,
- 实现方式：封装一个waterfall组件，从外部接收数据和列数，然后通过获取容器宽度、计算每列卡片宽度，初始化一个二维数组来存放数据内容，然后根据数据数组的index来计算应当放入哪一列；再添加一个窗口大小调整的监听器`window.addEventListener('resize', this.initColumns)`

```vue
<template>
    <waterfall :col="col" :data="albumData.content" :gutterWidth="5">
        <div
          class="cell-item"
          v-for="(item, index) in albumData.content"
          :key="index"
          @click="goToDetail(item.id)"
        >
          <img :src="item.url" alt="加载错误" />
        </div>
    </waterfall>
</template>
```
5. 适配移动端

`<meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1, user-scalable=no">`
不允许放大缩小

- @media 响应式 ：范围功能
```css
/* 还有print等设备 */
@media screen and (min-width: 900px) {
    article {
        padding: 1rem 3rem;
    }
}
@media only screen and (min-width: 320px) and (max-width: 480px) and (resolution: 150dpi) {
  body {
    line-height: 1.4;
  }
}
/* 当用户的主要输入机制（例如鼠标）可以悬停在元素上时 */
@media (hover: hover) { ... }
@media (max-width: 12450px) { ... }
/* 适用于任何带有彩色屏幕的设备 */
@media (color) { ... }
/* 将样式限制为宽度至少为 30 em 的横向的设备 */
@media (min-width: 30em) and (orientation: landscape) { ... }
/* 将样式限制为带有屏幕的设备，可以将媒体功能链接到screen媒体类型 */
@media screen and (min-width: 30em) and (orientation: landscape) { ... }
/* 当用户的设备与各种媒体类型，功能或状态中的任何一种匹配时，可以使用逗号分隔的列表来应用样式 */
@media (min-height: 680px), screen and (orientation: portrait) { ... }
/* not关键字会反转整个媒体查询的含义。它只会否定要应用的特定媒体查询。 */
@media not all and (monochrome) { ... }

@media (height > 600px) {
  body {
    line-height: 1.4;
  }
}

@media (400px <= width <= 700px) {
  body {
    line-height: 1.4;
  }
}

```
- 使用rem适配，使用`amfe-flexible`（窗口调整时，重新设置font-size）和`postcss-pxtorem`(px转rem)组件
    - rem是等比缩放的
- 同时采用

6. 优化策略

- VUE
    - 函数型组件<template functional>
    - 组件拆分，一些处理放在子组件中，生命周期会更短
    - 多用v-show，少用v-if
    - 提高渲染效率
    ```vue
    <keep-alive>
        <router-view/>
    </keep-alive>
    ```
    - 异步组件`defineAsyncComponent`
    - 按需引入`echats`等

- 优化首屏
对网络加载、

- webpack打包优化
    - 使用CDN。
    在vite.config.js中，在webpack打包时使用`external`取消下载相关的包
    在index.html中使用`<script src="xx"/>`的方式引入，Vue, Vue-router, element-ui等，src在`bootcdn`上找

- 图片使用CDN地址，图片懒加载
