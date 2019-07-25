# vue-week-datepicker

`vue-week-datepicker`是使用vue开发的一个移动端周历日期选择器组件。

* 灵活切换，容易上手
* 四种日期格式共选择（YYYY-MM-DD, YYYY/MM/DD, YYYY年MM月DD日, YYYYMMMDDD）
* 一键返回今日快捷键

## 动图演示

![image](https://github.com/KBeginner/vue-week-datepicker/blob/master/public/datepicker.gif)

## 快速开始

#### 拷贝组件
把组件拷贝到自己项目

#### 安装[Vant-ui](https://youzan.github.io/vant/#/zh-CN/quickstart)
```
npm i vant -S  // 通过 npm 安装
yarn add vant  // 或者通过 yarn 安装
```

#### 引入组件
```
import Datepicker from './components/Datepicker.vue'      // 根据存放组件的位置自行修改
```

#### 声明组件
```
components: {
    Datepicker
}
```
#### 调用组件
```
<Datepicker :time="time" :format="format" @change="changeDate"/>
```

## API说明

#### Props
|参数|说明|类型|默认值|必选|
|:---|---|---|---|---|
|time|初始值|`String`|-|否|
|format|日期格式，可选值为 `YYYY-MM-DD` `YYYY/MM/DD` `YYYY年MM月DD日` `YYYYMMDD`|`String`|`YYYY-MM-DD`|否|

#### Events
|事件名|说明|回调参数|
|:---|---|---|
|change|选择日期后触发的事件|选择的日期|


## 启动该项目

#### 初始化
```
npm install
```

#### 安装vant-ui
```
npm i vant -S       // 通过 npm 安装
yarn add vant       // 或通过 yarn 安装
```

#### 启动
```
npm run serve
```
