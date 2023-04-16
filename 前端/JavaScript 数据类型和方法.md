## JavaScript 数据类型和常用方法

- 对应的数据类型有: **字符串 , 数值 , 布尔值 , 数组 , 对象**

### 1.字符串方法

- `.length` 属性返回字符串的长度：

  ```JavaScript
  var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  var sln = txt.length;
  ```

- `.indexOf()` 方法返回字符串中指定文本**首次**出现的索引（位置,如果不存在返回值为-1）

  ```JavaScript
  var str = "People's China  Republic of China.";
  var pos = str.indexOf("China");
  ```

- `lastIndexOf() `方法返回指定文本在字符串中***最后***一次出现的索引,如果未找到文本

  - indexOf() 和 lastIndexOf() 均返回 -1。

  - 两种方法都接受作为检索起始位置的第二个参数。

    ```JavaScript
    var str = "The full name of China is the People's Republic of China.";
    var pos = str.indexOf("China", 18);
    ```

- `.search()` 方法搜索特定值的字符串，并返回匹配的位置(**首次**)

  ```js
  var str = "The full name of China is the People's Republic of China.";
  var pos = str.search("locate");
  ```

  - .search() 方法无法设置第二个开始位置参数
  - .indexOf() 方法无法使用正则表达式

- `.slice(*start*, *end*)`提取字符串的某个部分并在新字符串中返回被提取的部分

  - 如果省略第二个参数，则该 substr() 将裁剪字符串的剩余部分
  - 如果首个参数为负，则从字符串的结尾计算位置

  ```js
  var str = "Apple, Banana, Mango";
  var res = str.slice(-13,-7);  // Banana
  
  // 如果省略第二个参数，则该方法将裁剪字符串的剩余部分：
  var res = str.slice(7); //Banana, Mango
  
  // 从结尾计数：
  var res = str.slice(-13);
  ```

- `.substring(*start*, *end*)`类似于 slice(), 不同之处在于 substring() 无法接受负的索引

- `.substr(*start*, *length*)`类似于 slice()。不同之处在于第二个参数规定被提取部分的长度

  - 如果省略第二个参数，则该 substr() 将裁剪字符串的剩余部分。 
  - 如果首个参数为负，则从字符串的结尾计算位置。

  ```js
  var str = "Apple, Banana, Mango";
  var res = str.substr(7,6);  // Banana
  ```

- `.replace('oldStr',newStr')` 方法用另一个值替换在字符串中指定的值, 默认地，replace() *只替换首个匹配*, 且对大小写敏感

  ```js
  str = "Please visit Microsoft and Microsoft!";
  var n = str.replace("Microsoft", "W3School");
  ```

  - 如需执行大小写不敏感的替换，请使用正则表达式 /i（大小写不敏感）：请注意正则表达式不带引号。

    ```js
    str = "Please visit Microsoft!";
    var n = str.replace(/MICROSOFT/i, "W3School");
    ```

  - 如需替换所有匹配，请使用正则表达式的 g 标志（用于全局搜索）：

    ```js
    str = "Please visit Microsoft and Microsoft!";
    var n = str.replace(/Microsoft/g, "W3School");
    ```

- `.toUpperCase()` 全部转换为大写

- `.toLowerCase() `全部转换为小写

- `.concat()` 连接两个或多个字符串, 可用于代替加运算符

- `.trim() ` 方法删除字符串两端的空白符

- `.split()` 将字符串转换为数组,根据某字符串切割,如果改字符串不存在则返回一个长度为1的数组

  ```js
  let a = '1,2,3,4,5'
  b = a.split(',') //["1", "2", "3", "4", "5"]
  b = a.split('|') //["1,2,3,4,5"]
  b = a.split('')  // ["1", ",", "2", ",", "3", ",", "4", ",", "5"]
  ```



### 2.数字方法

```js
- JavaScript 只有一种数值类型,数值既可以带小数点，也可以不带
- 超大或超小的数可通过科学计数法来写
- JavaScript 数值始终是 64 位的浮点数
// 一个数和一个字符串相加 得到字符串
// 可以转化为数字的字符串 与数字相减 得到一个数字
```

- `.toString() ` 转化为字符串

  ```js
  var x = 123;
  x.toString();            // 从变量 x 返回 123
  (123).toString();        // 从文本 123 返回 123
  (100 + 23).toString();   // 从表达式 100 + 23 返回 123
  ```

- `Number() ` 将变量转换为数字

  ```js
  x = true;
  Number(x);        // 返回 1
  x = false;     
  Number(x);        // 返回 0
  x = new Date();
  Number(x);        // 返回 1404568027739
  x = "10"
  Number(x);        // 返回 10
  x = "10 20"
  Number(x);        // 返回 NaN   如果无法转换数字，则返回 NaN。
  
  Number(new Date("2019-04-15"));    // 返回 1506729600000
  ```

- `.parseInt()`将字符转换为数字

  ```js
  parseInt("10");         // 返回 10
  parseInt("10.33");      // 返回 10
  parseInt("10 20 30");   // 返回 10
  parseInt("10 years");   // 返回 10
  parseInt("years 10");   // 返回 NaN
  ```

  

- `.toFixed()`返回字符串值，它包含了指定位数小数的数字：

  ```js
  var x = 9.656;
  x.toFixed(0);           // 返回 10
  x.toFixed(2);           // 返回 9.66
  x.toFixed(4);           // 返回 9.6560
  x.toFixed(6);           // 返回 9.656000
  ```

- `.toPrecision()`返回字符串值，它包含了指定长度的数字：

  ```js
  var x = 9.656;
  x.toPrecision();        // 返回 9.656
  x.toPrecision(2);       // 返回 9.7
  x.toPrecision(4);       // 返回 9.656
  x.toPrecision(6);       // 返回 9.65600
  ```

### 3.数组方法

- `.length` 属性是数组的长度

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  fruits[fruits.length] = "Kiwi";          // 向 fruits 追加 "Kiwi"
  ```

- `Array.isArray(sourceList)` 判断是不是数组,返回bool值

- `.toString()` 把数组转换为数组值（逗号分隔）的字符串。

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  fruits.toString(); // Banana,Orange,Apple,Mango
  ```

- `.join(',')` 将所有数组元素结合为一个字符串

  ```js
  var fruits = ["Banana", "Orange","Apple", "Mango"];
  document.getElementById("demo").innerHTML = fruits.join(" * ");  
  // Banana * Orange * Apple * Mango
  ```

- `.pop()`方法从数组中删除最后一个元素,返回被删除的值：

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  fruits.pop();              // 从 fruits 删除最后一个元素（"Mango"）
  ```

- `.push()` 方法（在数组结尾处）向数组添加一个新的元素, 返回新数组的长度：

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  var x =  fruits.push("Kiwi");   //  x 的值是 5
  ```

- `.shift()` 方法删除首个数组元素，并把所有其他元素“位移”到更低的索引, 返回被“删除”的字符串：

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  fruits.shift();            // 从 fruits 删除第一个元素 "Banana"
  ```

- `.unshift()`（在开头）向数组添加新元素，并“反向位移”旧元素：

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  fruits.unshift("Lemon");    // 向 fruits 添加新元素 "Lemon"
  ```

- `.splice()` 方法可用于向数组添加新项：

  - 第一个参数（2）定义了应添加新元素的位置（拼接）。
  - 第二个参数（0）定义应删除多少元素。
  - 其余参数（“Lemon”，“Kiwi”）定义要添加的新元素。
  - splice() 方法返回一个**包含已删除项的数组**：

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  fruits.splice(2, 2, "Lemon", "Kiwi"); //  ["Apple", "Mango"]
   // fruits = ["Banana", "Orange", "Lemon", "Kiwi"]
  ```

  - **使用 splice() 来删除元素**

    ```js
    var fruits = ["Banana", "Orange", "Apple", "Mango"];
    fruits.splice(0, 2);        
    // 删除 fruits 中的前两个元素,返回被删除元素的数组
    ```

- `.concat()` 方法通过合并（连接）现有数组来创建一个新数组：

  - 不会更改现有数组。
  - 返回一个新数组。
  - 可以使用任意数量的**数组**参数.

  ```js
  var arr1 = ["Cecilie", "Lone"];
  var arr2 = ["Emil", "Tobias", "Linus"];
  var arr3 = ["Robin", "Morgan"];
  var myChildren = arr1.concat(arr2, arr3);   // 将arr1、arr2 与 arr3 连接在一起
  ```

- `.slice()` 数组切片,返回一个新数组, 不会改变原有数组.

  ```js
  var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
  var citrus = fruits.slice(1);   // ["Orange", "Lemon", "Apple", "Mango"];
  ```

  - slice() 可接受两个参数，比如 (1, 3),'顾头不顾尾'

    ```js
    var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
    var citrus = fruits.slice(1, 3);  // ["Orange", "Lemon"]
    ```

- `Math.max.apply(null, arr)` 求数组最大值
-  `Math.min.apply(null, arr)` 求数组最小值

- `.sort() `以**字母顺序**对数组进行排序

  ```js
  默认地，sort() 函数按照字符串顺序对值进行排序。
  该函数很适合字符串（"Apple" 会排在 "Banana" 之前）。
  不过，如果数字按照字符串来排序，则 "25" 大于 "100"，因为 "2" 大于 "1"。
  正因如此，sort() 方法在对数值排序时会产生不正确的结果。
  可以通过一个比值函数来修正此问题：
  
  var points = [40, 100, 1, 5, 25, 10];
  points.sort(function(a, b){return a - b}); // [1,5,10,25,40,100]
  ```

- `.reverse()` 反转数组中的元素

- `.indexOf() `方法在数组中搜索元素值并返回其位置,不存在返回-1

### 4.对象方法

- `Object.keys(obj)` 返回obj对象的key组成的数组
- `Object.values(obj)`  返回obj对象的value组成的数组

- `Object.getOwnPropertyNames(obj)` 返回一个数组，包含对象自身的所有属性（包含不可枚举属性）











