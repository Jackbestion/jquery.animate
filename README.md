﻿# jquery.animate

#### 重写了 jQuery.animate 的默认行为，使其优先使用 CSS3 Transition。<br>

#### Rewrite the <b>jQuery.animate</b>"s default behavior, make it priority use of <b>CSS3 Transition</b>.<br>


## 为什么使用？ / Why use?

这个插件是对 jQuery.animate 方法的改写,您不需要考虑修改您的代码，您只需要导入插件，就可以让你所有的 jQuery 动画转化为 CSS3 Transition。<br>

This plugin is the rewriting of <b>jQuery.animate</b> method, you do not need to consider modifying your code, you just need to include plug-in, can let all of your jQuery animation into <b>CSS3 Transition</b>.<br>


## Usage

将 [jquery.animate.js] 引在 jQuery 之后。<br>


Just include [jquery.animate.js] after jQuery.<br>


``` html

<script src="jquery.js"></script>

<script src="jquery.animate.js"></script>

```
[jquery.animate.js]: https://github.com/baijunjie/jquery.animate/blob/master/jquery.animate.js


## cssHook

可以直接通过 .css() 方法来设置 Transform 属性。<br>
Can be directly through the <b>.css()</b> method to set the <b>Transform</b> properties.<br>

``` js
$("div").css({ x: "100px" });                       //=> translate(100px, 0)
$("div").css({ y: "100%" });                        //=> translate(0, 100%)
$("div").css({ translate: [200,100] });             //=> translate(200px, 100px)
$("div").css({ translate3d: [200,100,300] });       //=> translate(200px, 100px) translateZ(300px) 
$("div").css({ scale: 2 });                         //=> scale(2)
$("div").css({ scaleX: 3 });                        //=> scale(3,1)
$("div").css({ scaleZ: 2 });                        //=> scaleZ(2)
$("div").css({ scale3d: [2, 1.5, 4] });             //=> scale(2,1.5) scaleZ(4) 
$("div").css({ rotate: "30deg" });                  //=> rotate(30deg)
$("div").css({ rotateX: 45 });                      //=> rotateX(45deg)
$("div").css({ rotateY: "1rad" });                  //=> rotateX(1rad)
$("div").css({ rotateZ: 90 });                      //=> rotate(90deg)
$("div").css({ rotate3d: [60,30,90] });             //=> rotateX(60deg) rotateY(30deg) rotate(90deg)
$("div").css({ skewX: "30deg" });                   //=> skewX(30deg)
$("div").css({ skewY: 60 });                        //=> skewY(60deg)
$("div").css({ skew: [30,60] });                    //=> skewX(30deg) skewY(60deg)
```


读取属性值。<br>
Read the attribute value.<br>
``` js
$("div").css({ x: 100 }).css("x");                       //=> "100px"
$("div").css({ x: 100, y: 200 }).css("translate");       //=> ["100px", "200px"]
$("div").css({ scaleX: 3, scaleY: 3 }).css("scale");     //=> "3"
$("div").css({ scaleX: 3 }).css("scale");                //=> ["3","1"]

```


支持删除属性<br>
Support delete attributes.<br>
```js
.css({ translate: "2, 5" });       //=> "translate(2px, 5px)"
.css({ translate: "" });           //=> remove "translate(2px, 5px)"
```


## animate

支持 jQuery.animate 原生的所有写法。<br>
Support all <b>jQuery.animate</b> native writing.<br>

```js
$("div").hide(2000);
$("div").hide().slideDown(2000, "easeOutBack");
$("div").hide().fadeIn(2000, function() { ... });

$("div").animate({left: 100});
$("div").animate({x: 200}, 1000, "easeOutBack", function() { ... });
$("div").animate({opacity: 0},{
	queue: false,
	duration: 2000,
	easing: "linear"
});
$("div").animate({
	left: [100, "linear"],
	top: [200, "easeOutBack",
	scale: [[2,3], "easeInBack"]
});
$("div").animate({
	left: 100,
	top: 200
},{
	queue: false,
	duration: 2000,
	specialEasing: {
		left: "linear",
		top: "easeOutBack"
	}
});
```
Transform 属性还支持特殊的写法。<br>
<b>Transform</b> properties also supports special writing.<br>

```js
$("div").animate({"scale": [1, 2]});
$("div").animate({"rotate3d": "30,20,90"});
```


可以完美支持 .stop() 以及 .finish() 方法。<br>
Can perfect support <b>.stop()</b> and <b>.finish()</b> method.<br>
```js
$("div").animate({x: 200}, 2000, "linear");
window.setTimeout(function() {
	$("div").stop();
	console.log($("div").css("x")); //=> 100px
}, 1000);
```
```js
$("div").animate({x: 200}, 2000, "linear");
$("div").finish();
console.log($("div").css("x"));         //=> 200px
```


注意，由于 CSS3 Transition 动画的只支持三次贝塞尔曲线，因此[jQuery.easing]中的某些缓动无法支持，比如：Bounce<br>
如果使用了它不支持的 easing，那么将会使用原来的 jQuery.animate 方法来实现该动画。<br>

Note that because of <b>CSS3 Transition</b> animation support only three times bezier curve, so some slow moving in [jQuery.easing] cannot support, such as: Bounce<br>
If use it does not support <b>easing</b>, We will use the original <b>jQuery.animate</b> methods to achieve this animation.<br>

[jQuery.easing]: https://github.com/gdsmith/jquery.easing

以下是本插件支持的缓动列表。<br>
The following is a list of transit support slow.<br>

```js
$.cssEase = {
    "_default":       "swing",
    "swing":          "easeOutQuad", // 和 jQuery Easing 相同，查看详情 https://github.com/gdsmith/jquery.easing
    "linear":         "cubic-bezier(0,0,1,1)",
    "ease":           "cubic-bezier(.25,.1,.25,1)",
    "ease-in":        "cubic-bezier(.42,0,1,1)",
    "ease-out":       "cubic-bezier(0,0,.58,1)",
    "ease-in-out":    "cubic-bezier(.42,0,.58,1)",

    "easeInCubic":    "cubic-bezier(.550,.055,.675,.190)",
    "easeOutCubic":   "cubic-bezier(.215,.61,.355,1)",
    "easeInOutCubic": "cubic-bezier(.645,.045,.355,1)",
    "easeInCirc":     "cubic-bezier(.6,.04,.98,.335)",
    "easeOutCirc":    "cubic-bezier(.075,.82,.165,1)",
    "easeInOutCirc":  "cubic-bezier(.785,.135,.15,.86)",
    "easeInExpo":     "cubic-bezier(.95,.05,.795,.035)",
    "easeOutExpo":    "cubic-bezier(.19,1,.22,1)",
    "easeInOutExpo":  "cubic-bezier(1,0,0,1)",
    "easeInQuad":     "cubic-bezier(.55,.085,.68,.53)",
    "easeOutQuad":    "cubic-bezier(.25,.46,.45,.94)",
    "easeInOutQuad":  "cubic-bezier(.455,.03,.515,.955)",
    "easeInQuart":    "cubic-bezier(.895,.03,.685,.22)",
    "easeOutQuart":   "cubic-bezier(.165,.84,.44,1)",
    "easeInOutQuart": "cubic-bezier(.77,0,.175,1)",
    "easeInQuint":    "cubic-bezier(.755,.05,.855,.06)",
    "easeOutQuint":   "cubic-bezier(.23,1,.32,1)",
    "easeInOutQuint": "cubic-bezier(.86,0,.07,1)",
    "easeInSine":     "cubic-bezier(.47,0,.745,.715)",
    "easeOutSine":    "cubic-bezier(.39,.575,.565,1)",
    "easeInOutSine":  "cubic-bezier(.445,.05,.55,.95)",
    "easeInBack":     "cubic-bezier(.6,-.28,.735,.045)",
    "easeOutBack":    "cubic-bezier(.175, .885,.32,1.275)",
    "easeInOutBack":  "cubic-bezier(.68,-.55,.265,1.55)"
};
```

## Thanks

这个插件是参考以下插件编写的，感谢作者！<br> 
This plug-in is written in reference to the following plug-ins, thanks to the author!<br>

__[jquery.transit](https://github.com/rstacruz/jquery.transit)__
__[jQuery 2D transformations](https://github.com/heygrady/transform/)__


