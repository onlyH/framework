```javascript
// 供应商
var obj = {
  time: "2020-02-02",
  name: "net",
  _r: 123,
};
// 代理商
var monitor = new Proxy(obj, {
  // 拦截对象属性的读取
  get(target, key) {
    console.log(target, key, target[key]);
    return target[key].replace("2020", "2029");
  },
  set(target, key, value) {
    // 只允许修改name
    if (key == "name") {
      return (target[key] = value);
    } else {
      return target[key];
    }
  },
  //   拦截 key in obj操作
  has(target, key) {
    if (key == "name") {
      return target[key];
    } else {
      return false;
    }
  },
  // 拦截delete
  deleteProperty(target, key) {
    if (key.indexOf("_") > -1) {
      delete target[key];
    } else {
      return target[key];
    }
  },
  //   拦截Object.keys，Object.getOwnPropertySymbols,Object.getOwnPropertyNames
  ownKeys(target) {
    //   保护time
    return Object.keys(target).filter((item) => item != "time");
  },
});
console.log("set", monitor.time);
console.log("===========================");
console.log("has", "name" in monitor, "time" in monitor); // has true false
delete monitor.time;
console.log("delete", monitor); //{time: "2020-02-02", name: "net", _r: 123}
delete monitor._r;
console.log("delete", monitor); //{time: "2020-02-02", name: "net"}
console.log("ownKeys", Object.keys(monitor)); //ownKeys (2) ["name", "_r"]

```
