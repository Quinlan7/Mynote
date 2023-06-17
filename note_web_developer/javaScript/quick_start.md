# JavaScript

*起因：在低代码开发平台进行的编程都是使用的 JS 所以学习一下*

2023.05.31

+ let teacherIdList = teacherList.map(item => item._id)

> 当你在 JavaScript 中使用数组的 `map` 方法时，它会对数组的每个元素应用一个回调函数，并返回一个新的数组，新数组的每个元素都是回调函数的返回值。
>
> 语法形式如下：
> ```
> const newArray = array.map(callback(element, index, array) {
>   // 操作逻辑，返回新的元素值
> });
> ```
>
> - `array`：要进行映射操作的原始数组。
> - `callback`：回调函数，它会接收三个参数：
>   - `element`：当前处理的数组元素。
>   - `index`（可选）：当前处理元素的索引。
>   - `array`（可选）：原始数组。
>
> 在给定的代码示例中，`teacherList` 是一个数组，`map` 方法被调用在它上面。回调函数使用箭头函数 `(item => item._id)` 的形式，表示它接收一个参数 `item`，代表数组中的每个元素。箭头函数的主体部分是 `item._id`，表示返回 `item` 对象的 `_id` 属性值。
>
> `map` 方法遍历 `teacherList` 数组的每个元素，将每个元素传递给回调函数，并将回调函数的返回值添加到一个新的数组中。最终，该方法返回一个包含了每个元素的 `_id` 属性值的新数组 `teacherIdList`。
>
> 举个例子，假设有以下 `teacherList` 数组：
> ```
> const teacherList = [
>   { _id: 1, name: 'John' },
>   { _id: 2, name: 'Alice' },
>   { _id: 3, name: 'Bob' }
> ];
> ```
>
> 使用给定的 `map` 方法，将会得到以下的 `teacherIdList` 数组：
> ```
> [1, 2, 3]
> ```
>
> 注意，原始数组 `teacherList` 不会受到影响，`map` 方法只会返回一个新的数组。





+ 在js中.push方法是什么类型的对象拥有的

> 在JavaScript中，`.push()` 方法是数组对象（Array）所拥有的方法。它用于向数组的末尾添加一个或多个元素，并返回修改后的数组的新长度。
>
> 使用语法为：`array.push(element1, element2, ..., elementN)`
>
> 这里的 `array` 是一个数组对象，`element1` 到 `elementN` 是要添加到数组末尾的元素。`.push()` 方法会按照传入的参数顺序将元素添加到数组中，并返回更新后的数组的新长度。
>
> 示例：
> ```javascript
> var fruits = ['apple', 'banana', 'orange'];
> console.log(fruits); // 输出: ['apple', 'banana', 'orange']
> 
> var newLength = fruits.push('kiwi');
> console.log(fruits); // 输出: ['apple', 'banana', 'orange', 'kiwi']
> console.log(newLength); // 输出: 4
> ```
>
> 在上述示例中，我们调用了 `fruits.push('kiwi')`，将 `'kiwi'` 添加到 `fruits` 数组的末尾。调用完 `.push()` 方法后，`fruits` 数组变成 `['apple', 'banana', 'orange', 'kiwi']`，并且返回值 `newLength` 等于 4，表示更新后的数组的长度为 4。





+ perList = [...perList,...ret],js中这段代码什么意思

> 这段代码使用了 JavaScript 中的扩展运算符（Spread Operator）。
>
> 在给定的代码中，`perList` 是一个数组，`ret` 也是一个数组。该代码的目的是将 `ret` 数组的元素添加到 `perList` 数组的末尾，生成一个新的数组，并将新数组赋值给 `perList`。
>
> 使用扩展运算符 `...`，可以将数组展开为逗号分隔的元素序列。所以，`[...perList,...ret]` 将 `perList` 和 `ret` 两个数组中的元素展开，并通过逗号连接在一起，形成一个新的数组。
>
> 例如，假设 `perList` 是 `[1, 2, 3]`，`ret` 是 `[4, 5, 6]`，那么运行该代码后，`perList` 的值将变为 `[1, 2, 3, 4, 5, 6]`。
>
> 这种用法可以方便地将两个数组合并为一个数组，而不需要使用循环或其他方法逐个添加元素。



+ js保留两位小数

> 在 JavaScript 中，你可以使用内置的 `toFixed()` 方法来将一个数字保留指定的小数位数。
>
> `toFixed()` 方法接受一个参数，表示要保留的小数位数。它返回一个字符串，表示指定小数位数的数字。
>
> 以下是一些示例：
>
> ```javascript
> var number = 3.1415926;
> var rounded = number.toFixed(2);
> console.log(rounded); // 输出: "3.14"
> 
> var anotherNumber = 9.87654321;
> var anotherRounded = anotherNumber.toFixed(2);
> console.log(anotherRounded); // 输出: "9.88"
> ```
>
> 在上述示例中，我们使用 `toFixed()` 方法将数字 `number` 和 `anotherNumber` 保留两位小数。结果是一个字符串，表示保留了两位小数的数字。
>
> 需要注意的是，`toFixed()` 方法将返回一个字符串，而不是一个数字。如果需要执行数值计算，你可能需要使用 `parseFloat()` 或 `parseInt()` 方法将结果转换为数字类型。
>
> ```javascript
> var roundedNumber = parseFloat(rounded);
> console.log(roundedNumber); // 输出: 3.14
> ```
>
> 这样，你就可以保留指定小数位数并进行数值操作。
