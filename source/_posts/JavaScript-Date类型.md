---
title: JavaScript Date类型
date: 2017-10-30 21:48:07
tags: JavaScript
---
## new Date()
var now = new Date();
Date中保存1970年1月1日午夜（零时）开始经过的毫秒数来保存日期。
在不传递参数的情况下，新创建的对象自动获取当前时间。日期和时间都基于本地时区而非GMT来创建。
想根据特定的日期和时间创建对象可以直接传或者使用Date.parse()和Date.UTC()
```
//使用指定的年月日[时分秒]进行初始化
var d1 = new Date(2017, 10, 30);
var d2 = new Date('2017/10/30');
```

### Date.parse()
Date.parse()方法接收一个表示日期的字符串参数，然后根据这个字符串返回相应的毫秒数。
ECMA-262没有定义Date.parse()应该支持那种日期格式，因此该方法的实现因地区而异。将地区设置为美国的浏览器通常都接受一下日期格式：
 - “月/日/年”，如5/20/2004；
 - “英文月 日,年”，如Janunary 25,2004；
 - “英文星期几 英文月名 日 年 时:分:秒”，如Tue May 25 2004 00:00:00 GMT-0700；
如果传入Date.parse()方法的字符串不能表示日期，那么它返回NaN。

` var someDate = new Date(Date.parse("May 25, 2017"));`
### Date.UTC()
Date.UTC()方法同样也返回表示日期的毫秒数，但与Date.parse()在构建值时使用不同的信息。
Date.UTC()的参数分别是年份、基于0的月份（一月是0，二月是1，...）、月中的那天（1到31）、小时（0到23）、分钟（0到59）、秒（0到59）及毫秒数。这些参数中只有前两个参数（年和月）是必须的。

```
//GMT时间2017年1月1日午夜零时
var now = new Date(Date.UTC(2017, 0));
//GMT时间2017年10月30日下午18:25:30
var nowtime = new Date(Date.UTC(2017, 9, 30, 18, 25, 30));
```

### Date.now()
var start = Date.now();
该方法表示调用这个方法时的日期和和时间的毫秒数。

## ValueOf()
Date的ValueOf方法，根本不返回字符串，而是返回日期的毫秒表示。因此可以用来比较日期值。
```
var date1 = new Date(2017, 10, 30);
var date2 = new Date(2017, 11, 11);
alert(date1 < date2);  //true
alert(date1 > date2);  //false
```

## 日期常用方法
> 1.每个分量都有一对** get/set **方法，获取/设置该分量的值
2.命名：年月日 没有s,** 时分秒 有s **
3.取值/赋值：除了每月中的日之外，其余都是从0开始到-1结束
         每月中的日从1开始，到31结束
         月份：** 取值时要+1修正，赋值时-1修正 **
         星期：日 一 二 三 四 五 六 对应 0  1  2  3  4  5  6

## 日期计算：
> 1.两日期对象相减，得到毫秒
2.时间+-：var nowMs = date.getTime() 返回时间毫秒数
    var nextMs = mowMs +/- 5\*60\*1000;
    var next = new Date(nextMs);
使用毫秒做计算，不改变原时间对象！需要重新封装时间对象
最大仅能算到“天数”。再算“月数”无法确定没有天数
3.任意分量的计算
  先取出分量值，做计算，再set回去
  setXXX()：
  - 1.自动调整进制
  - 2.** 直接修改原日期对象 **
如何保留原日期对象呢？先new出新Date对象，再set
var newDate = new Date(oldDate.getTime());
使用毫秒做计算，不改变原时间对象！需要重新封装时间对象
最大仅能算到“天数”。再算“月数”无法确定没有天数

## 日期格式化方法
** 一般自定义format(date)方法 **
Date类型一些专门用于将日期格式化为字符串的方法：

|    方法  |说明|
|--------|--------------|
|toString()|返回带有时区信息的日期和时间（0-23）  |
|toLocaleString()|返回日期和时间的本地格式（AM、PM） |
|toDateString() | 以特定于实现的格式显示星期几、月、日和年 |
|toTimeString()  |以特定于实现的格式显示时、分、秒和时区|
|toLocaleDateString()|以特定于地区的格式显示星期几、月、日和年|
|toLocaleTimeString()|以特定于的格式显示时、分、秒|
|toUTCString()|以特定于实现的格式完整的UTC日期|

## 日期/时间组件方法
|    方法  |说明|
|--------|--------------|
|getTime|返回表示日期的毫秒数；与valueOf方法返回的值相同 |
|getDate()|从 Date 对象返回一个月中的某一天 (1 ~ 31)|
|getDay()|从 Date 对象返回一周中的某一天 (0 ~ 6)|
|getMonth()|    从 Date 对象返回月份 (0 ~ 11)|
|getFullYear()| 从 Date 对象以四位数字返回年份|
|getYear()| 请使用 getFullYear() 方法代替|
|getHours()|    返回 Date 对象的小时 (0 ~ 23)|
|getMinutes()|  返回 Date 对象的分钟 (0 ~ 59)|
|getSeconds()   |返回 Date 对象的秒数 (0 ~ 59)|
|getMilliseconds()| 返回 Date 对象的毫秒(0 ~ 999)|
|getTime()| 返回 1970 年 1 月 1 日至今的毫秒数|
|getTimezoneOffset()|   返回本地时间与格林威治标准时间 (GMT) 的分钟差|
|getUTCDate()|  根据世界时从 Date 对象返回月中的一天 (1 ~ 31)|
|getUTCDay()|根据世界时从 Date 对象返回周中的一天 (0 ~ 6)|
|getUTCMonth()|根据世界时从 Date 对象返回月份 (0 ~ 11)|
|getUTCFullYear()|根据世界时从 Date 对象返回四位数的年份|
|getUTCHours()|根据世界时返回 Date 对象的小时 (0 ~ 23)|
|getUTCMinutes()|根据世界时返回 Date 对象的分钟 (0 ~ 59)|
|getUTCSeconds()|根据世界时返回 Date 对象的秒钟 (0 ~ 59)|
|getUTCMilliseconds()|根据世界时返回 Date 对象的毫秒(0 ~ 999)|
|parse()|返回1970年1月1日午夜到指定日期（字符串）的毫秒数|
|setDate()|设置 Date 对象中月的某一天 (1 ~ 31)|
|setMonth()|设置 Date 对象中月份 (0 ~ 11)|
|setFullYear()|设置 Date 对象中的年份（四位数字）|
|setYear()  |请使用 setFullYear() 方法代替|
|setHours()|设置 Date 对象中的小时 (0 ~ 23)。
|setMinutes()|设置 Date 对象中的分钟 (0 ~ 59)|
|setSeconds()|设置 Date 对象中的秒钟 (0 ~ 59)|
|setMilliseconds()|设置 Date 对象中的毫秒 (0 ~ 999)|
|setTime()|以毫秒设置 Date 对象|
|setUTCDate()|  根据世界时设置 Date 对象中月份的一天 (1 ~ 31)|
|setUTCMonth()|根据世界时设置 Date 对象中的月份 (0 ~ 11)|
|setUTCFullYear()|根据世界时设置 Date 对象中的年份（四位数字）|
|setUTCHours()| 根据世界时设置 Date 对象中的小时 (0 ~ 23)|
|setUTCMinutes()    |根据世界时设置 Date 对象中的分钟 (0 ~ 59)|
|setUTCSeconds()|   根据世界时设置 Date 对象中的秒钟 (0 ~ 59)|
|setUTCMilliseconds()|根据世界时设置 Date 对象中的毫秒 (0 ~ 999)|

## 小案例
```
//张三2015年9月1日上大学
//在校4年，求离校日期？
//离校前一个月要答辩
//如果答辩日碰到周末则提前
//求答辩日期？
//提前一周提醒答辩，求提醒日期？
var hire = new Date("2017-9-1");
console.log("到校时间："+format(hire));
hire.setFullYear(hire.getFullYear()+3);
console.log("离校时间："+format(hire));
hire.setMonth(hire.getMonth()-1);
if(hire.getDay() == 6){ //减一天
    hire.setDate(hire.getDate()-1);
}else if(hire.getDay() == 0){//减两天
    hire.setDate(hire.getDate()-2);
}   
console.log("答辩时间："+format(hire));
hire.setDate(hire.getDate()-2);
console.log("提醒时间："+format(hire));  

//格式化时间
function format(date){
    function format(date){
    var weeks = ["日", "一", "二", "三", "四", "五", "六"];
    var year = date.getFullYear() + "年";
    var month = formatNum(date.getMonth() + 1) + "月";
    var day = formatNum(date.getDate()) + "日";
    var week = " 星期" + weeks[date.getDay()];
    var hour = formatNum(date.getHours()) + "时";
    var minute = formatNum(date.getMinutes()) + "分";
    var second = formatNum(date.getSeconds()) + "秒";
    //格式化小于10的数字
    function formatNum(num){
        if(num<10){
            num = "0" + num;
        }
        return num;
    }           
    var str = year + month + day + week +" "+ hour +":"+ minute +":"+ second;
    return str;
}
```
运行结果：
```
到校时间：2017年09月01日 星期五 09时:25分:30秒
离校时间：2020年09月01日 星期二 09时:25分:30秒
答辩时间：2020年07月31日 星期五 09时:25分:30秒
提醒时间：2020年07月29日 星期三 09时:25分:30秒
```
## 时间选择器
[时间选择器插件](https://github.com/syymo/DatePicker)
