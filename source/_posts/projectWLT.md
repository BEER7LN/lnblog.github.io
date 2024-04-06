---
title: 课程平台
date: 2023-04-26
tags: [前端项目, 前端开发]
---

- 背景：

  为广大农业从业者和爱好者提供一个便捷、高效、专业的在线学习平台，帮助他们掌握农业技术、理论和管理知识，提升农业生产能力和水平。简而言之，是一个线上课程学习平台。

- 需求功能结构图：

![Alt text](WTL.png)

- 项目地址：

  http://1.12.236.243:2080/

  http://1.12.236.243:1080/login

- 记录

1. 全局前置守卫：后台权限管理，仅登录状态可以进行各页面访问

```javascript
router.beforeEach(async (to, from) => {
  if (
    // 检查用户是否已登录
    !isAuthenticated &&
    // ❗️ 避免无限重定向
    to.name !== "Login"
  ) {
    // 将用户重定向到登录页面
    return { name: "Login" };
  }
});
```

2. 引入 md 文档：vue3-markdown-it 和 github-markdown-css 组件

```vue
<!-- template 中接受md文件 -->
<Markdown :source="md_text" :breaks="true"></Markdown>
<!-- style 中设置样式 -->
<style>
.markdown-body {
}
</style>
```

3. 横向滚动推荐列表

- `v-if="scroll"` 父级组件 Props 通信传入，判断内容长度是否长到需要左右滚动
- `v-show="scroll_vis.prv"`
- `@click="handlePrev"` `scrollTo`方法控制 `scrollLeft`从而实现滚动

```vue
<script>
const handlePrev = () => {
  scrollContainer(-props.scrollStep);
};
const scrollContainer = (step) => {
  const container = cardContainer.value;
  // element.scrollLeft 设置/读取元素滚动条到元素左边的距离
  const newScrollLeft = container.scrollLeft + step;
  container.scrollTo({
    // top, left, behavior
    left: newScrollLeft,
    behavior: "smooth",
    // behavior: 'instant'
  });
};
</script>

<template>
  <img
    src="/_img/class/next.svg"
    @click="handlePrev"
    v-if="scroll"
    v-show="scroll_vis.prv"
  />
</template>
```

- `ref` 注册元素或子组件到 this.$refs 中
- `slot` 插槽引用具体卡片
- `@scroll` 滚动监听事件，`handleScroll`根据滚动距离设置左右箭头显示与否

```vue
<template>
  <div
    class="card_container"
    ref="cardContainer"
    @scroll="handleScroll"
    :class="{ no_scroll: !scroll }"
  >
    <slot></slot>
  </div>
</template>

<script>
const handleScroll = () => {
  const container = cardContainer.value;
  const scrollLeft = Math.ceil(container.scrollLeft);
  // scrollWidth值等于元素在不使用水平滚动条的情况下适合视口中的所有内容所需的最小宽度。包括由于 overflow 溢出而在屏幕上不可见的内容。
  // clientWidth值等于元素的内容区域加上内边距，不包括边框和外边距的宽度。
  const maxScrollLeft = container.scrollWidth - container.clientWidth;
  switch (scrollLeft) {
    // 最左侧
    case 0:
      scroll_vis.value.prv = false;
      scroll_vis.value.next = true;
      break;
    // 最右侧
    case maxScrollLeft:
      scroll_vis.value.prv = true;
      scroll_vis.value.next = false;
      break;
    // 其他
    default:
      scroll_vis.value.prv = true;
      scroll_vis.value.next = true;
      break;
  }
};
</script>
```

4. 实现文本超过多少字符之后的省略号表示

- 现有的 `element-ui` 的 `el-text` 使用 `truncated`

```vue
<el-text class="w-150px mb-2" truncated>
    Self element set width 100px
  </el-text>
```

- 自己实现对字数的把控

```js
export const truncateText = (text, maxLength) => {
  // 匹配所有汉字,英文字母和数字
  const pattern = /[^\x00-\xff]|\w/g;
  const matchArray = text.match(pattern);

  if (!matchArray) {
    // 如果没有匹配到任何字符，返回空字符串
    return "";
  }

  let truncatedText = "";
  let charCount = 0;

  for (const char of matchArray) {
    // 如果字符是汉字，占用 1 个位置
    // 如果是英文字符，占用 0.5 个位置
    charCount += char.charCodeAt(0) > 255 ? 1 : 0.25;

    if (charCount <= maxLength) {
      truncatedText += char;
    } else {
      break;
    }
  }

  // 如果字符串被截断，添加省略号
  if (truncatedText !== text) {
    truncatedText += "...";
  }
  console.log(truncatedText);
  return truncatedText;
};
```

5. 分页器

- 分别处理`@current-change` `@prev-click` `@next-click`（仅有跳转到的目标页数的区别）
- `handelPageChange` 获取接口(参数包括页数)再更新 data

```vue
<script>
const page_info = ref({
  pageNum: 1,
  pageSize: 12,
  total: 200,
});
const handelPageChange = async (val) => {
  // console.log(selectedTag.value.tagId);
  if (selectType.value) {
    await getPageInfoByClassify(
      val,
      page_info.value.pageSize,
      selectedTag.value.tagId
    );
  } else {
    await getPageInfo(val, page_info.value.pageSize);
  }
};
</script>
<el-pagination
  :page-size="page_info.pageSize"
  @current-change="handelPageChange"
  @prev-click="handelPagePrev"
  @next-click="handelPageNext"
  layout="prev, pager, next"
  :total="page_info.total"
  background
/>
```

6. 视频播放媒体

- 使用 `html5` 的 `<video>` 视频嵌入元素：视频尽量为 `mp4` 格式，以支持多款浏览器

```vue
<video
  controls
  :src="target_info.video"
  ref="video"
  @pause="player_status = false"
></video>
```

7. `echarts` 图表绘制

- 学会基础的使用

```vue
<template>
  <div style="width: 60vw; height: 60vh" id="container"></div>
</template>
<script setup>
// 按需引入，减少包体积
import { BarChart } from "echarts/charts";
import { GridComponent } from "echarts/components";
import * as echarts from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { onMounted } from "vue";
echarts.use([GridComponent, BarChart, CanvasRenderer]);
var option = {
  title: {
    text: "ECharts 入门示例",
  },
  tooltip: {},
  legend: {
    data: ["销量"],
  },
  xAxis: {
    data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
  },
  yAxis: {},
  series: [
    {
      name: "销量",
      type: "bar",
      data: [5, 20, 36, 10, 10, 20],
    },
  ],
};
function fetchData() {
  var myChart = echarts.init(document.getElementById("container"));
  myChart.setOption(option);
}
onMounted(() => {
  console.log("onMounted");
  fetchData();
});
</script>
```

8. 原生 `html5` 和 `css3` 来实现定制化表格
