# CSS 揭秘 - part2 背景与边框
 
## 01 半透明边框
- 原理：背景会延伸到边框所在区域下层。
- CSS3 可以使用background-clip调整上述
	- 背景被裁剪到边框盒/内边距框/内容框


## 02 多重边框
-  offset-x | offset-y | blur-radius | spread-radius | color  
- box-shadow可以创建任意数量的投影，根据投影的叠加，第二层会在第一层下面伸展出。

// **outline 方案**
- box-shadow 只能模拟是实线边框
- outline不占空间，且可以接受负值 但是只能有两层边框

```
background: yellowgreen;
border: 10px solid #655
outline: 5px solid deepink;
```

## 03 灵活的背景定位
- 利用`background-position` 指定像素偏移量
- 或者使用内边距 使得logo跟着内边距走
- 一般`background-position` 指`padding-box`，通过改变`background-origin`来改变计算方式

// **calc()方案**
```
background: url("images/QQ.svg") no-repeat;
background-position: cacl(100% - 20px ) calc(100% -10px);
```

## 04 边框内圆角
- 原始方案：使用两个div，使用border+outline可以实现
- 另外一个单元素方案有点hack的味道，利用了描边不会跟着border-radius走，不过未来这个标准会改变....

## 05 条纹背景
[linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient)

- 利用`linear-gradient`的区域填充 当两个色标相同时，就没有渐变效果 变为实色
- 由于渐变是代码生成的图像，可以使用`background-size`调整尺寸
- 利用下面第二个特性来做成多条纹。
- 添加角度参数 改变条纹的生成方向。

> 如果多个色标具有相同的位置，它们会产生一个无限小的过渡区域，过渡的起止色分别是第一个和最后一个指定值。从效果上看，颜色会在那个位置突然变化，而不是一个平滑的渐变过程

> 如果某个色标的位置值比整个列表中在它之前的色标的位置值都要小，则该色标的位置值会被设置为它前面所有色标位置值的最大值

// **斜条纹处理方案**
- 42.426 来源： 2*15根号2 ， 具体需要参考原书。因为一个单位的条纹需要两种颜色交替填充
- 更好的替代方案， 可以制作30° 与 60°。 : `linear-gradient()` & `radial-gradient()`各有一个循环式的加强版 **`repeating-radial-gradient`**
- 通过叠加效果（`background-color`+`background-image`)使得有一个灵活的背景色。且这样写对不支持CSS渐变的浏览器来说这里背景色还起到了回退的作用。

## 06 复杂的背景图案
- 一个渐变效果能产生的图案并不多，当使用多个渐变组合的时候可以产生复杂的背景图案 —— 比如 竖直条纹+水平条纹 = 网格 

- 对于网格， 需要用到 [radial-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/radial-gradient)
- 棋盘的这个实现略复杂。。 需要参考原书 ，当未来实现角向渐变的时候 就不用那么麻烦的去拼合三角形去实现了。

ps: 用svg实现代码比较精简
```
<svg xmlns="http:www.w3.org/2000/svg"
	 width="100" height="100" fill-opacity=".25">
	<rect x="50" width="50" height="50" />
	<rect y="50" width="50" height="50" />
</svg>
```

## 07 伪随机背景
- 改变不同条纹层叠时的粗细，再改变重复的间隔使得看上去是随机的。
- 代码是每隔240px重复一次 实际较难发现 （就是40 60 80 三个间隔的最小公倍数）
- 要使随机效果更真实 即间隔更大，就让最小公倍数变大，这些数字最好是相对质数 使得它们的乘积相得较大

## 08 连续的图像边框
- 在背景图片的基础上，叠加白色的纯色背景使得背景以边框的形式透出来（`background-clip`) 
- 背景叠加， 通过线性渐变从白色渐变到白色，而图片背景放在第二个。