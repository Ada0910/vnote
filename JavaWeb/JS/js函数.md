# 1. js常见说明：
```
(1)$(function() {}) 是$(document).ready(function())的简写
(2)JSON.stringify(value[, replacer[, space]]) 方法用于将 JavaScript 值转换为 JSON 字符串。
(3)JSON.parse(text[, reviver]) 方法用于将一个 JSON 字符串转换为对象。
```

# 2.  函数的写法
## 2.1. 常规写法
```
 function fnName(){
     console.log("常规写法");
 }
```
## 2.2. 匿名函数
```
var myfn = function(){
  console.log("匿名函数，函数保存到变量里");
  }
```
## 2.3. 如果有多个变量，可以用对象收编变量
### 2.3.1. 用json对象
```
var fnobject1={
    fn1:function(){
          console.log("第一个函数");
    },
    fn2:function(){
          console.log("第二个函数");
    },
    fn3:function(){
          console.log("第三个函数");
    }
}
```
### 2.3.2. 声明一个对象，然后给它添加方法
```
var fnobject2 = function(){};
fnobject2.fn1 = function(){
    console.log("第一个函数");
}
fnobject2.fn2 = function(){
    console.log("第二个函数");
}
fnobject2.fn3 = function(){
    console.log("第三个函数");
}
```
### 2.3.3. 可以把方法放在一个对象函数里
```
var fnobject3 = function(){
    return {
        fn1:function(){
            console.log("第一个函数");
            },
        fn2:function(){
             console.log("第二个函数");
         },
          fn3:function(){
            console.log("第三个函数");
         }    
    }    
};
```
## 2.4. 可用类来实现,注意类的第二种和第三种写法不能混用，否则一旦混用，如在后面为对象的原型对象赋值新对象时，那么他将会覆盖掉之前对prototype对象赋值的方法
```
(1)
var fnobject4 = function(){
    this.fn1 = function(){
        console.log("第一个函数");
    }
    this.fn2 = function(){
        console.log("第二个函数"); 
    }
    this.fn3 = function(){
        console.log("第三个函数");
    }
};


(2)
var fnobject5 = function(){};
fnobject5.prototype.fn1 = function(){
    console.log("第一个函数");
}
fnobject5.prototype.fn2 = function(){
    console.log("第二个函数");
}
fnobject5.prototype.fn3 = function(){
    console.log("第三个函数");
}


(3)
var fnobject6 = function(){};
fnobject6.prototype={
    fn1:function(){
        console.log("第一个函数");
    },
    fn2:function(){
        console.log("第二个函数");
    },
    fn3:function(){
        console.log("第三个函数");
    }
}


(4)
var fnobject7 = function(){};
fnobject7.prototype={
    fn1:function(){
        console.log("第一个函数");
        return this;
    },
        fn2:function(){
        console.log("第二个函数");
        return this;
    },
    fn3:function(){
        console.log("第三个函数");
        return this;
    }
}
```
## 2.5. 对Function对象类的扩展(下面三种只能用一种)
### 2.5.1. 第一种写法（对象）

```
Function.prototype.addMethod = function(name,fn){
    this[name] = fn;
 }
var methods=function(){};//var methods=new Function();
methods.addMethod('fn1',function(){
    console.log("第一个函数");
});
methods.addMethod('fn2',function(){
    console.log("第二个函数");
});
methods.addMethod('fn3',function(){
    console.log("第三个函数");
});
```
### 2.5.2. 链式添加（对象)
```
Function.prototype.addMethod = function(name,fn){
    this[name] = fn;
    return this;
 }
var methods=function(){};//var methods=new Function();
methods.addMethod('fn1',function(){
    console.log("第一个函数");
}).addMethod('fn2',function(){
    console.log("第二个函数");
}).addMethod('fn3',function(){
    console.log("第三个函数");
});
```
### 2.5.3. 链式添加（类）
```
Function.prototype.addMethod = function(name,fn){
    this.prototype[name] = fn;
    return this;
}
var Methods=function(){};//var methods=new Function();
methods.addMethod('fn1',function(){
    console.log("第一个函数");
}).addMethod('fn2',function(){
    console.log("第二个函数");
}).addMethod('fn3',function(){
    console.log("第三个函数");
});
```

