```javascript
var obj = {
  time: "2020-02-02",
  name: "net",
  _r: 123,
};
console.log("get", Reflect.get(obj, "time")); //get 2020-02-02

Reflect.set(obj, "name", "xxxxx");
console.log(obj); //{time: "2020-02-02", name: "xxxxx", _r: 123}
console.log(Reflect.has(obj, "name")); // true
```
