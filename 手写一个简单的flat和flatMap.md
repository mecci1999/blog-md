### 前言

最近在公司的一个项目中，有用户反馈在低版本的谷歌浏览器中，不能正常登录系统，经过检查后发现在项目中使用了ES6的数组新方法flatMap，又查了下flatMap的支持浏览器版本，该方法只支持69以上版本的谷歌浏览器，所以为了兼容低版本的浏览器，不另外用babel进行配置，手写一个支持低版本浏览器的flatMap，废话不多说，下面让我们一起手写一个flatMap方法。

### 了解flat和flatMap

在动手之前我们先来了解一下flat和flatMap的用法，根据方法具体的用法、参数等等，再去手撸实现一个就很明白了。首先flat和flatMap是ES6中增加的数组新方法，flat()方法会按照**一个可指定的深度参数递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回**。换句话说就是把一个多维数组进行扁平化，得到一个一维或低维数组。它有一个可选参数depth，指定要提取的嵌套数组的结构深度，默认为1。（tip: flat方法会自动去除数组中的空项）下面是具体的使用例子：

![flat的具体使用](https://api.darwin.fun/images/serve?name=BLOG_03_IMG_01.jpg)

flatMap()方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 连着深度值为 1 的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些。它有两个参数，第一个参数为一个映射函数，也就是对应的map方法中的映射函数；第二个参数为this指向。换句话说，flatMap()就是首先使用map方法去操作操作数组中的每一个元素，最后再对map处理完后的数组进行flat(1)方法进行扁平化。下面是具体的使用例子：

![flatMap的具体使用](https://api.darwin.fun/images/serve?name=BLOG_03_IMG_02.jpg)

### 手撸一个flat

根据flatMap方法，我们需要先手撸一个flat方法，我们才能写另一个flatMap方法。按照上面对flat()方法的介绍，为了实现扁平化，第一时间肯定想到的方法就是使用递归进行一个遍历，达到一个扁平化的目的。简单介绍一下思路，遍历数组，首先判断数组的元素是否是数组，如果是数组进行递归，如果不是数组就将元素push到结果数组中，递归的base case就是判断是否为数组加上参数的值是否大于0，下面是具体的代码实现：

``` javascript
/**
 * @param {*数组} arr 
 * @param {*深度} depth 
 */
const myFlat = function(arr, depth) {
  // 判断depth是否存在
  depth = typeof depth === 'undefined' ? 1 : Number(depth) | 0

  // 结果
  const result = []

  // 递归
  const flatDeep = function(arr1, depth) {
    arr1.forEach(item => {
      if(depth > 0 && Array.isArray(item)) {
        flatDeep(item, depth - 1)
      }else {
        result.push(item)
      }
    })
  }

  flatDeep(arr, depth)

  return result
}
```

![flat方法代码实现](https://api.darwin.fun/images/serve?name=BLOG_03_IMG_03.jpg)

这样我们就实现了一个flat方法，但是看上去不够优美，我们可以再使用reduce()进行一个优化。

``` javascript
/**
 * @param {*数组} arr 
 * @param {*深度} depth 
 */
const myFlat = function(arr, depth) {
  // 判断depth是否存在
  depth = typeof depth === 'undefined' ? 1 : Number(depth) | 0

  // 递归
  const flatDeep = function(arr1, depth) {
    return arr1.reduce((result, item) => {
     return result.concat(depth > 0 && Array.isArray(item) ? flatDeep(item, depth - 1) : item) 
    }, [])
  
  }

  return flatDeep(arr, depth)
}
```

![flat方法代码优化](https://api.darwin.fun/images/serve?name=BLOG_03_IMG_04.jpg)

以上，我们实现了一个flat()方法，现在我们可以根据flat方法去写一个flatMap方法。

### 手撸一个flatMap

flatMap方法有两个参数，第一个参数为映射函数,第二个函数为this，思路就是结合map()和flat()，就可以得到，废话不多说，上代码。

``` javascript
const myFlatMap = function() {
  // 获取映射函数
  let callback = arguments[1]

  // 判断第一个参数是否为函数
  if(typeof callback !== 'function') throw new TypeError('The flatMap arguments callback must be a function')

  // 获得对象数组
  let arr = arguments[0]

  // 获取第二个参数，可为null
  let _this = arguments[1]

  // 遍历数组执行映射函数
  return myFlat(arr.map(item => {
    return callback.apply(_this, [item, arr])
  }), 1) 
}
```

![flatMap方法代码实现](https://api.darwin.fun/images/serve?name=BLOG_03_IMG_05.jpg)

以上就是flat()方法和flatMap()方法的实现过程，其实还能进行代码的优化，让代码看上去更优雅一些。

### 结束

结尾依旧是保留界面，附上一张日常图。

![日常图](https://api.darwin.fun/images/serve?name=BLOG_03_IMG_06.jpg)