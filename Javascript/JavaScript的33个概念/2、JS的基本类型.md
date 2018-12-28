# javaScript数据类型

## 原始数据类型

JavaScript中的原始类型包含以下几个：

* Null
* Undefined-------一个没有被赋值的变量会有个默认值 undefined
* String-----------字符串基本类型
* Number---------数值基本类型
* Boolean---------布尔基本类型
* Symbol----------字面量基本类型(ECMAScript 6 新定义)

1、null
null是表示缺少表示，指示变量未指向任何对象。可以作为尚未创建的对象。

null与undefined的不同点：
当检测 null 或 undefined 时，注意相等（==）与全等（===）的区别，相等（==）会执行类型转换：

    typeof null         // 'object'
    typeof undefined    // 'undefined'
    null == undefined   // true （都转换成布尔类型进行比较）
    null === undefined  // false
    null === null       // true
    null == null        // true
    !null               // true
    isNaN(1 + null)     // false
    isNaN(1 + undefined) // true

2、undefined  
一个没有被赋值的变量会有一个默认值undefined。undefined是全局对象的一个属性, 是全局作用域的一个变量。  
自ECMAscript5标准以来undefined是一个不能被配置，不能被重写的属性。
注意：undefined不是一个保留字，注意不要把undefined当做变量使用。

    // eg：1
    var x;
    x === undefined // true
    typeof x === 'undefined' // true

    // eg：2
    typeof y === 'undefined' // true
    y === undefined // ReferenceError: y is not defined
    使用typeof时，不会因为变量未声明而报错；相反，直接使用 y === undefined 就会报参数未定义错误。

undefined 和 void

    void 0 === undefined    // ture
    typeof void 0 === 'undefined'   // ture
    z === void 0    // 此时因为z未定义 所以会报 ReferenceError: z is not defined

    void 可以作为undefined的一种替代方案。

3、String

JavaScript的字符串类型用于表示文本数据。它是一组16位的无符号整数值的“元素”。在字符串中的每个元素占据了字符串的位置。第一个元素的索引为0，下一个是索引1，依此类推。字符串的长度是它的元素的数量。

4、Number

根据ECMAScript标准，JavaScript中只有一种数字类型：基于IEEE 754标准的双精度64位二进制格式的值（-（2^63 -1）到 2^63 -1）。没有特定的类型，除了可以表示浮点数、整数外，还有带符号的值 +Infinity 和 -Infinity、NaN(非数值)。

检测数值是否在安全范围内（大于 +Infinity 或 小于 -Infinity）：  
ES5: 使用常量 Number.MAX_VALUE和Number.MIN_VALUE
ES6: 可以通过Number.isSafeInteger()方法，通过Number.MAX_VALUE_INTEGER和Number.MIN_VALUE_INTEGER检测是否在双精度浮点数的取值范围内。

数字类型只有一个整数，它有两种表示方法： 0 可表示为 -0 和 +0（"0" 是 +0 的简写）。 在实践中，这也几乎没有影响。 例如 +0 === -0 为真。 但是，你可能要注意除以0的时候：

    2 / +0; // Infinity
    2 / -0; // -Infinity


5、Symbol

Symbol()函数会返回symbol类型的值，该类型具有静态属性和静态方法。它的静态属性会暴露几个内建的成员对象；它的静态方法会暴露全局的symbol注册，且类似于内建对象类，但作为构造函数来说它并不完整，因为它不支持语法："new Symbol()"。

    let a = new Symbol()    // TypeError

每个从Symbol()返回的symbol值都是唯一的。一个symbol值能作为对象属性的标识符；这是该数据类型仅有的目的。

    Symbol(1) === Symbol(1)     // false
    typeof Symbol(2)    // 'symbol'