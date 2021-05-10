# vue-pithy-calendar

## 方式一：从Github下载
```
yarn install
或者
cnpm install
```

运行：

```
yarn serve
```

## 方式二：从npm下载

```
cnpm install vue-pithy-calendar --save 
或者
yarn add vue-pithy-calendar
```

引入：

在`mian.js`中引入

```
import VuePithyCalendar from 'vue-pithy-calendar'

import 'vue-pithy-calendar/dist/vue-pithy-calendar.css'

Vue.use(VuePithyCalendar)
```

使用：

```
<template>
  <div id="app">
  <vue-pithy-calendar @chooseDay="getChooseDay"/>
    
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
  
  },
   methods: {
   getChooseDay(msg){
     console.log(msg);
   }
    
  },
}
</script>

<style lang="scss">

</style>

```

|   方法    | 是否必写 |        说明        |    返回结果     |
| :-------: | :------: | :----------------: | :-------------: |
| chooseDay |    否    | 获取点击选中的日期 | 例如：2021-5-10 |

效果：

![](https://i.loli.net/2021/05/10/BpDkq5WRfTtgZlP.png)