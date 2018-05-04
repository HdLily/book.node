# book.node
# 函数

**函数对象**

---

JS中的函数就是对像，对象“名/值”对的集合并拥有一个连到原型对象的隐藏连接。每个函数对象在创建时也随配一个prototype属性，因为函数是对象，所以他们可以像任何其他的值一样被使用，所以函数可以拥有方法。
***
**函数字面量**

---

函数对象通过函数字面量来创建：

    var add = function (a,b) {
      return a+b;
      };
  

函数字面量包括4个部分。
第一个部分是保留字function。
第二个部分是函数名。
第三个部分是包围在圆括号中的一组参数。
第四是包围在圆括号中的一组参数。
函数字面量可以出现在任何允许表达式出现的地方。

---
**调用**

---
调用一个函数会暂停当前函数的执行，传递控制权和参数给新函数。除了声明时定义的形式参数，每个函数g还接受两个附加的参数：this和arguments。在Javascript中一共有4种调用模式：方法调用模式，函数调用模式，构造器和apply调用模式。

---
- 方法调用模式

---
当一个函数被保存为对象的一个属性时，我们称它为一个方法。
当一个方法被调用时，this被绑定到该对象。方法可以使用this访问自己所属的对象，所以他能从对象中取值或对对象进行修改。

***

- 函数调用模式

---
当一个函数并非一个对象的属性时，那么它就是被当作一个函数来调用的：

   

    //给myObject增加一个double方法。
    myObject.double = function (){
    var that = this; //解决方法
    var helper = function (){
     that.value = add(that.value,that.value);
     };
     helper(); //以函数的形式调用helper.
     };
     //以方法的形式调用double。
     myObject.double();
     document.writenln(myObject.value);//6
 

---
- 构造器调用模式

---
一个函数，如果创建的目的就是希望结合new前缀来调用，那它就被称为构造器函数，按照约定，它们保存在以大写格式命名的变量里，

---
- Apply调用模式

---
apply方法让我们构建一个参数数组传递给调用函数，它也允许我们选择this的值，apply方法接收俩个参数，第一个是要绑定给this的值，第二个就是一个参数数组。

    //构造一个包含两个数字的数组，并将他们相加
    var array = [3,4];
    var sum = add.apply(null,array);
    //构造一个包含status成员的对象。
    var statusObject = {
    status:'A-OK'
    };
    //statusObject并没有继承自Quo.prototype,但我们可以在statusOb//ject上调用get_status方法，尽管statusObject并没有一个名为get_st//atus的方法。
    var status = Quo.prototype.get_status.apply(statusObject);
    //status的值为'A-OK'。

---
**参数**

---
当函数被调用时，会得到一个“免费”配送的参数，那就是arguments数组。函数可以通过此参数访问所有它被调用时传递给它的参数列表，包括那些没有被分配给函数声明时定义的形式的多余参数，这使得编写一个无须指定参数个数函数成为可能。

---
**返回**

---
return语句可用来使函数提前返回，当return被执行时，函数立即返回而不再执行余下的语句。
一个函数总会返回一个值，如果没有指定返回值，则返回undefined.
如果函数调用时在前面加上了new前缀，且返回值不是一个对象，则返回hthis（该新对象）。

---
**异常**

---
异常是干扰程序的正常流程的不寻常（但并非完全是出乎意料的）的事故。当发生这样的事故时，你的程序应该抛出一个异常：

    var add = function(a,b){
    if(typeof a !=='number' || typeof b !== 'number'){
    throw{
    name:'TypeError',
    message:'add needs nembers'
    };
    }
    return a+b;
    }
throw语句中断函数的执行。它应该抛出一个exception对象，该对象包含一个用来识别异常类型的name属性和一个描述的message属性。你也可以添加其他的属性。
该exception对象将被传递到一个try语句的catch从句：

    //构造一个try_it函数，以不正确的方式调用之前的add函数。
    var try_it = function(){
        try{
              add("seven");
            }catch(e){
                document.writeln(e.name +':'+e.message);
            }
         }
         try_it();
     

如果try代码块内抛出了一个异常，控制权就会跳转到它的catch从句。

一个try语句只会有一个捕获所有异常的catch代码块。如果你的处理手段取决于异常的类型，那么异常处理器必须检查异常处理器必须检查异常对象的name属性来确定异常的类型。

---
**扩充类型的功能**

---
通过给基本类型增加方法，我们可以极大地提高语言的表现力，因为Javascript原型继承的动态本质，新的方法立刻被赋予到所有的对象实例上，哪怕对象实例是在方法被增加之前就创建好了。

基本类型的原型是公用结构，所以在类库混用时务必小心。一个保险的做法就是只在该方法时才添加它。

    //符合条件时才增加方法
    function.prototype.method = function (name,func){
       if(!this.prototype[name]){
           this.prototype[name] = func;
           }
           return this;
           };
       
另一个要注意的就是for in语句用在原型上时表现很糟糕。我们可以使用hasOwnproperty方法筛选出继承而来的属性，或者我们可以查找特定的类型。

---
**递归**

---
递归函数就是会直接或间接地调用自身的一种函数。递归是一种强大的编程技术，它把一个问题分解为一组相似的子问题，每一个都用一个寻常的解去解决。一般来说，一个递归函数调用自身去解决它的子问题。

递归函数可以非常高效的操作树形结构，比如浏览器的文档对象模型（DOM)。每次递归调用时处理指定的树的一小段。

    //定义walk_the_DOM函数，它从某个指定的节点开始，按HTML源码中    //的顺序访问该树的每个节点。
    //它会调用一个函数，并依次传递每个节点给它。walk_the_DOM调用    //自身去处理每一个子节点。
     var walk_the_DOM = function walk(node,func){
         func(node);
         node = node.firstChild;
         while(node){
            walk(node,func);
            node = node.nextSibling;
            }
        };
        //定义getElementsByAttribute函数。它以一个属性名称字符串
        //和一个可选的匹配值作为参数。
        //它调用walk_the_DOM,传递一个用来查找节点属性名的函数作         //为参数。匹配的节点会累加到一个结果数组中。
        var getElementsByAttribute = function (att,value){
           var result = [];
           walk_the_DOM(document.body,function(node){
             var actual = node.nodeType === 1&& node.getAttribute(att); 
             if(typeof actual === 'String'&&(actual === value ||typeof value !=='String')){
             results.push(node);
             }
            });
            return results;
            };
        
---
**作用域**

---
在编程语言中，*作用域*控制着变量与参数的可见性及生命周期。对程序员来说这是一项重要的服务，因为它减少了名称冲突，并且提供了自动内存管理。

---
**闭包**

---
作用域的好处是内部函数可以访问定义他们的外部函数的参数和变量（除了this和arguments)。

和以对象字面量形式去初始化myObject不同，我们通过调用一个函数的形式去初始化myObject，该j函数会返回一个对象字面量。函数里定义了一个value变量，该变量对increment和getValue方法总是可用的，但函数的作用域使得它对其他的程序来说是不可见的。

    var myObject = (function(){
        var value = 0;
        
        return{
           increment:function(inc){
           value +=typeof inc === 'number' ? inc : 1;
           }，
           getValue:function(){
              return value;
              }
            };
       }());
   

我们并没有把一个函赋值给myOject。我们是把调用该函数后返回的接过赋值给它。注意最后一行的()。该函数返回包含两个方法的对象，并且这些方法继续享有访问value变量的特权。

---
**回调**

---
发起异步请求，提供一个当服务器的响应到达时随即触发的回调函数。
异步函数立即返回，这样客户端就不会被阻塞。

    request = prepare_the_request();
    send_request_asynchronously(request,function (response){
          display(response);
          });
          
      

---
      
**模块**

---
模块是一个提供接口却隐藏状态与实现的函数或对象。通过函数产生模块，我们几乎可以完全摒弃全局变量的使用，从而缓解JavaScript的最为糟糕的特性之一所带来的影响。

模块模式的一般形式是：一个定义了私有变量和函数的函数；利用闭包创建可以访问私有变量和函数的特权函数；最后返回这个特权函数，或者把他们保存到一个可访问到的地方。

---
**级联**

---