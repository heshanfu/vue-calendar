# calendar

 [Chinese](https://github.com/kylin-z/vue-calendar/blob/master/README.zh-CN.md)
 
 This is a calendar component based on vue.js . support custom content. No dependencies.
 Currently, It only supports month view. You can click the control button to change the month.
 
 [Simple Live Demo](http://kylin.himmas.cc/vue-calendar/example/index.html)
 
 ![](http://7xrqm7.com1.z0.glb.clouddn.com/readme-vue-calendar-example-1.png?imageView2/1/w/1200/h/800/q/100)

## Install

### npm

```bash
$ npm install himmas-vue-calendar

```

### script

```html
<script src='dist/vue-calendar.js'>
```

## Usage

### Global Registration

```js
//main.js
import Vue from 'vue'
import App from './App.vue'
//...

import Calendar from 'himmas-vue-calendar'
Vue.use(Calendar)

//...

new Vue({
  el: '#app',
  render: h => h(App)
})

```

```html
<!--app.vue-->
<template>
  <div id="app">
    <!-- 'kl-' prefix -->
    <kl-calendar height="800px" width="800px"/>
  </div>
</template>

<script>

  export default {
    name: 'App'
  }
</script>

```

### Local Registration

```html
<!--app.vue-->
<template>
  <div id="app">
    <calendar height="800px" width="800px"/>
  </div>
</template>

<script>
  import Calendar from 'himmas-vue-calendar'
  export default {
    name: 'App',
    components:{Calendar}
  }
</script>

```

## Attributes

|Attribute|Description|Type|Accepted Values	|Default|
|---|---|---|---|---|
| width|Calendar's width|String|-|100%|
| height|Calendar's height|String|-|100%|
| border|whether Calendar has vertical border|Boolean|true/false|true|
| show-lunar|whether lunar info is visible.if `render-content` is has been defined, this attribute does not work)|Boolean|true/false|true|
| show-festival|whether festival is visible.if `render-content` is has been defined, this attribute does not work|Boolean|true/false|true|
| show-term|whether solar terms is visible.if `render-content` is has been defined, this attribute does not work|Boolean|true/false|true|
|week-count|the number of weeks|Number|-|5|
|week-title-align|the alignment of head information|String|left/right/center|right|
|week-title|head content | Array<String> |-|['周日', '周一', '周二', '周三', '周四', '周五', '周六']|
|render-content|render function for date, support jsx|Function(h,date)|-||
|before-render|callback before rendering|Function(year,month,next)|-||

## Events

| Event	| Description | params |
|---|---|---|
| year-change  | This event will be fired when the currently rendered year changes  |  year,month |
| month-change  | This event will be fired when the currently rendered month changes  |  year,month |
| date-click  | This event will be fired when you click a date |  date |

## Methods
| Method	| Description | params |
|---|---|---|
|renderThisMonth|render a month|year, month|
|getRenderedMonth|get the currently rendered month information||

## date

`render-content` second param `date`

| Key | Description  |
|---|---|
| date  | Date Object  | 
| year  | year  | 
| month  | the month of the year  | 
| day  |  the day of the month  | 
| weekDay  | the day of the week(0-6)  | 
| lunar  | lunar info | 
| festival  | festival | 
| term  | solar term | 
| isToday  | isToday | 
| isWeekend  | isWeekend | 
| isOtherMonthDay  | whether it belongs to the current rendering month | 
| renderYear  | the current rendering year | 
| renderMonth  | the current month is rendered | 


## example

- [default](http://kylin.himmas.cc/vue-calendar/example/index.html)
- [custom example](http://kylin.himmas.cc/vue-calendar/example/demo.html)
  
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <style>
    .date-box {
      position: absolute;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      box-sizing: border-box;
    }
    .first-info{
      flex: 1;
      display: flex;
      align-items: flex-end;
      justify-content: center;
      font-size: 18px;
      font-weight: bold;
    }
    .second-info{
      flex: 1;
      display: flex;
      justify-content: center;
      color: #999;
      font-size: 12px;
    }
    .second-info.festival{
      color: #f43;
    }
    .sign{
      display: none;
      position: absolute;
      top: 0;
      left: 0;
      background: #f43;
      width: 20px;
      height: 20px;
      color: #fff;
      line-height: 20px;
      text-align: center;
    }
    .date-box.today{
      background: #fb0;
      color: #fff;
    }
    .date-box.today .second-info{
      color: #fff;
    }
    .weekend{
      background: #f6f8fa;
    }
    .holiday .sign{
      display: block;
    }
    .date-box.other-month .second-info,.date-box.other-month .first-info{
      color: #999;
    }
    .date-box:hover{
      border: 3px solid #fb0;
    }
  </style>
  <body>
  <script src="./lib/vue.min.js"></script>
  <script src="../dist/vue-calendar.js"></script>
  <div id="app">
    <kl-calendar width="600px" height="500px"
                 :render-content="renderContent"
                 :week-title="weekTitle"
                 :border="false"
                 :before-render="beforeRender"
                 @year-change="changeHandle"
                 @month-change="changeHandle"
  
    />
  </div>
  <script>
  
    Vue.use(Calendar)
  
    new Vue({
      el: '#app',
      data() {
        return {
          weekTitle: ['日', '一', '二', '三', '四', '五', '六'],
          holiday: [
            '2018-01-01',
            '2018-02-15',
            '2018-02-16',
            '2018-02-17',
            '2018-02-18',
            '2018-02-19',
            '2018-02-20',
            '2018-02-21',
          ]
        }
      },
      methods: {
        twoDigit:function(num){ return ('000'+num).slice(-2) },
        renderContent(h, data) {
          var {isToday,isWeekend,isOtherMonthDay, year, day, month, renderYear, renderMonth, lunar, weekDay, festival, term} = data
  
          // lunar info
          var {lunarDayChiness} = lunar
  
         
          var secondInfo = festival ?
            festival : (term ? term : (lunarDayChiness || ''))
  
          var dateStr = `${year}-${this.twoDigit(month)}-${this.twoDigit(day)}`
  
          var isHoliday = (!!~this.holiday.indexOf(dateStr)) || isWeekend
  
          return h('div', {
            class: {
              'date-box': true,
              'today':isToday,
              'weekend':isWeekend,
              'holiday':isHoliday,
              'other-month':isOtherMonthDay
            }
          }, [h('div',{
            class: {
              'first-info': true
            }
          },day),h('div',{
            class: {
              'second-info': true,
              'festival':festival
            }
          },secondInfo),h('div',{
            class: {
              'sign': true
            }
          },'休')])
        },
        beforeRender(year,month,next){
          console.log('before-render',year,month)
          next()
        },
        changeHandle(year,month){
          console.log('change',year,month)
        }
      }
    })
  </script>
  </body>
  </html>

  ```

## tips

- IE9- not support
- based on vue.js v2.1.5+


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```
