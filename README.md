#### 1.typeof

1. 判断所有值类型：`Undefined`、`String`、`Number`、`Boolean`、`Symbol`
2. 判断函数：`function`
3. 识别引用类型：`object`(null)

#### 2.深拷贝

```js
const obj1 = {
  age: 20,
  name: 'xxx',
  address: {
    city: 'beijing',
  },
  arr: ['a', 'b', 'c'],
}

const obj2 = deepClone(obj1)
obj2.address.city = 'shanghai'
obj2.arr[0] = 'a1'
console.log(obj1.address.city) // beijing
console.log(obj1.arr[0]) // a

// 深拷贝
function deepClone(obj = {}) {
  if (typeof obj !== 'object' || obj === null) {
    // obj 是null，或者不是对象或数组，直接返回
    return obj
  }

  // 初始化返回结果
  let result
  if (obj instanceof Array) {
    result = []
  } else {
    result = {}
  }

  for (let key in obj) {
    // 保证key不是原型的属性
    if (obj.hasOwnProperty(key)) {
      // 递归调用
      result[key] = deepClone(obj[key])
    }
  }
  // 返回结果
  return result
}
```

#### 3.类型转换

1. 字符串拼接：`+` 号

2. `==` 运算符：除了 `a == null` 之外一律用 `===` 

   ```js
   const obj = { x: 100 }
   if (obj.a == null) {}
   // 相当于
   // if (obj.a === null || obj.a === undefined) {}
   ```

3. ```js
   truly变量：!!a === true的变量
   falsely变量：!!a === false的变量
   // 以下是falsely变量，除此之外都是truly变量
    !!0 === false
    !!NaN === false
    !!'' === false
    !!null === false
    !!undefined === false
    !!false === false
   ```

4. 逻辑运算：&&、||

5. `instanceof`

   ```js
   xialuo instanceof Student // true
   xialuo instanceof People // true
   xialuo instanceof Object // true
   
   [] instanceof Array // true
   [] instanceof Object // true
   
   {} instanceof Array // true
   ```


#### 4.class继承

   ```js
   class Person {
     constructor(name) {
     	this.name = name
     }
     eat() {
     	console.log(`${this.name} eat something`)
     }
   }
   
   // 子类
   class Student extends Person {
   	constructor(name, number) {
       super(name)
       this.number = number
   	}
     sayHi() {
     	console.log(`姓名 ${this.name} 学号 ${this.number}`)
     }
   }
   
   class Teacher extends Person {
     constructor(name, major) {
       super(name)
       this.major = major
   	}
     teach() {
     	console.log(`姓名 ${this.name} 教授 ${this.major}`)
     }
   }
   
   const xialuo = new Student('夏洛', 100)
   console.log(xialuo.name)
   console.log(xialuo.number)
   xialuo.sayHi()
   xialuo.eat()
   
   const wanglaoshi = new Teacher('王老师', '语文')
   console.log(wanglaoshi.name)
   console.log(wanglaoshi.major)
   wanglaoshi.teach()
   wanglaoshi.eat()
   ```

#### 5.作用域，闭包，自由变量

1. 作用域：

   - 全局作用域
   - 函数作用域
   - 块级作用域

2. 自由变量

   - 变量在当前作用域没有定义，但是被使用了
   - 向上级作用域，一层一层寻找。知道找到为止，找不到则报错

3. 闭包：作用域应用的特殊情况

   - 函数作为参数被传递
   - 函数作为返回值被返回

   > 所有自由变量的查找，是在函数定义的地方，向上级作用域查找
   >
   > 不是在函数执行的地方！

4. 闭包的应用

   - 隐藏数据：如做一个cache工具

     ```js
     // 闭包隐藏数据，只提供API
     function createCache() {
     	const data = {} // 闭包中的数据，被隐藏，不被外界访问
     	return {
     		set: function (key, val) {
     			data[key] = val
     		},
     		get: function (key) {
     			return data[key]
     		},
     	}
     }
     const c = createCache()
     c.set('a', 100)
     console.log(c.get('a')) // 100
     ```

     

#### 6.this

- this的指向是在函数执行的时候定义

  1. 作为普通函数调用，指向window

  2. 使用`call`、`apply`、`bind`

     ```js
     function fn1 () {
     	console.log(this)
     }
     fn1.call({ x : 100}） // { x: 100 }
     
     const fn2 = fn1.bind({ x: 200}) // bind是返回新的函数执行
     fn2() // { x: 200 }
     ```

     

  3. 作为对象方法调用，指向当前对象

  4. 在class方法中调用

  5. 箭头函数

     ```js
     const zhangsan = {
       name: '张三',
       sayHi() {
         // this 即当前对象
         console.log(this)
       },
       wait() {
         setTimeout(function () {
           // this 即window
           console.log(this)
       	})
       },
     }
     
     const zhangsan = {
       name: '张三',
       sayHi() {
         // this 即当前对象
         console.log(this)
       },
       wait() {
         setTimeout(() => {
           // this 即当前对象
           console.log(this)
       	})
       },
     }
     
     // class中
     class People {
       constructor(name) {
         this.name = name
         this.age = 20
       }
     
       sayHi() {
         console.log(this)
       }
     }
     const zhangsan = new People('张三')
     zhangsan.sayHi() // this 即zhangsan对象
     ```


#### 7.异步和同步

- 基于JS是单线程语言

- 异步不会阻塞代码执行

- 同步会阻塞代码执行

  - 异步应用场景
    1. 网络请求，如`ajax`图片加载
    2. 定时任务，如`setTimeout`

- Promise

  ```js
  const url1 =
        'https://img1.baidu.com/it/u=4127991555,3421789262&fm=253&fmt=auto&app=138&f=JPEG?w=680&h=454'
  const url2 =
        'https://img1.baidu.com/it/u=1407750889,3441968730&fm=253&fmt=auto&app=120&f=JPEG?w=1200&h=799'
  
  function loadImg(src) {
  	return new Promise((resolve, reject) => {
  		const img = document.createElement('img')
  		img.onload = () => resolve(img)
  		img.onerror = () => reject(new Error(`图片加载失败 ${src}`))
  
  		img.src = src
  	})
  }
  
  loadImg(url1)
  	.then((img1) => {
  		console.log(img1.width)
  		return img1 // 普通对象
  	})
  	.then((img1) => {
  		console.log(img1.height)
  		return loadImg(url2) // promise实例
  	})
  	.then((img2) => {
  		console.log(img2.width)
  		return img2
  	})
  	.then((img2) => {
  		console.log(img2.height)
  	})
  	.catch((err) => {
  		console.log(err)
  	})
  ```

  