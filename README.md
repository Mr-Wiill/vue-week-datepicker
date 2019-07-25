# vue-week-datepicker

`vue-week-datepicker`是使用vue开发的一个移动端周历日期选择器组件。

周历选择器在移动端中选择日期和切换日期都会更加灵活，同时该组件还支持四种常用的日期格式供选择（YYYY-MM-DD, YYYY/MM/DD, YYYY年MM月DD日, YYYYMMMDDD），拥有一键返回今日日期的快捷键，已经封装成了一个组件，引入即可使用。

## 动图演示

![image](https://github.com/KBeginner/vue-week-datepicker/blob/master/public/datepicker.gif)

## 快速开始

#### 安装vant-ui
```
npm i vant -S  // 通过 npm 安装
yarn add vant  // 或者通过 yarn 安装
```

#### 引入组件
```
import Datepicker from './components/Datepicker.vue'      // 根据自身项目路径进行修改
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


## 启动项目

### 初始化
```
npm install
```

### 启动
```
npm run serve
```

### 打包
```
npm run build
```
