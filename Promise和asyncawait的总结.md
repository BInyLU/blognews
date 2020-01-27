## Promise和async/await的总结

>  第一次开始注意到ES6的语法，就是因为`Promise`，感觉比回调好用，后来又接触到`async/await`，使用后大大的减少了代码的层次结构，因此觉得有必要总结一下

`Promise`是ES6的语法，`async/await`是ES7的语法

### Promise

Promise是异步编程的一种解决方案，它有三种状态，分别是：

- pending：进行中
- resolved：已完成
- rejected：已失败

**之前的方法回调：**

```javascript
function func1(cbk){
  ...
  cbk && cbk(参数1，参数2，参数3，...);
  ...
}

//方法调用
func1((参数1，参数2，参数3，...)=>{
  ...
})
```

**改用Promise后：**

```javascript
function func1(){
  ...
  reurn new Promise((resolve,reject)=>{
      try{
          //如果成功
          resolve(参数);
      }catch(e){
        //如果失败
        reject(e);
      }
  });
}

//方法调用
//func1();

//func1().catch(()=>{});

//var p = func1();
//p.then(res=>{ ... });

func1().then((参数)=>{
  //调用成功
}).catch(err=>{
  //调用失败
  err && console.error(err);
});
```

改用`Promise`后，调用更加灵活了，回调可以处理，也可以不处理，同时可以处理异步调用的异常，更具有通用性

已网络接口调用为例（这里采用ajax）：

```javascript
function ajaxPromise(  param ) {
  return new Promise((resovle, reject) => {
    $.ajax({
      "type":param.type || "get",
      "async":param.async || true,
      "url":param.url,
      "data":param.data || "",
      "success": res => { resovle(res); },
      "error": err => { reject(err); }
    })
  })
}

//调用
var p =ajaxPromise({
  url: "接口"
});
p.then(res=>{
  ...
});
```

**Promise.all**

```javascript
function p1(){
  ...
  return new Promise((resolve, reject) => {
    resolve('success1');
  })
}

function p1(){
  ...
  return new Promise((resolve, reject) => {
    resolve('success2');
  })
}

function p3(){
  ...
  return new Promise((resolve, reject) => {
    reject('error');
  })
}

function func1(flag){
  var ps = [];
  ps.push(p1());
  ps.push(p2());
  flag && ps.push(p3());
  return new Promise.all(ps);
}

//方法调用
func1().then(res=>{
  console.log(res);
}).catch(err=>{
  consolo.error(err);// ['error']
});

func1(1).then(res=>{
  console.log(res);
}).catch(err=>{
  consolo.error(err);// ['success1','success2']
});
```

- `Promise.all`: 只要有一个失败了，就会抛出异常
- `Promise.race`: 返回执行最快的那个，无论异常或者失败

### async/await

`await`必须使用在`async`修饰的方法内部

```javascript
function func1(){
  ...
  reurn new Promise((resolve,reject)=>{
      try{
          //如果成功
          resolve(参数);
      }catch(e){
        //如果失败
        reject(e);
      }
  });
}

function async func2(){
  var res = await func1().catch(err=>{ err && console.error(err); });
  console.log(res);
  ...
}
```

使用`async/await`可以减少代码的嵌套，使代码更加清晰，代码中的`func1`使用`await`修饰后，可以直接拿到`then`方法中的结果，同时可以如果不使用`Promise.catch`方法，则会抛出异常，这时候可以配合’try/catch’使用：

```javascript
function async func2(){
  try{
    var res = await func1();
    console.log(res);
    ...
  }catch(err){
     err && console.error(err); 
  }
}
```

对应的，`func1`也可以采用`asyn`实现：

```javascript
function asyn func1(){
  ...
  if(成功){
    return 参数;
  }else{
    throw '我错了...';
 } 
}

//调用
func1().then(res=>{
  ...
}).catch(err=>{
  err && console.error(err);
});
```