```javascript
// 数据校验 === 使用场景  解耦

{
  function validator(target, validator) {
    return new Proxy(target, {
      _validator: validator,
      // 代理控制
      set(target, key, value, proxy) {
        if (target.hasOwnProperty(key)) {
          var a = this._validator[key];
          if (!!a(value)) {
            return Reflect.set(target, key, value, proxy);
          } else {
            throw Error("不能设置${key}到${value}");
          }
        } else {
          throw Error(`${key} 不存在`);
        }
      },
    });
  }

  var person = {
    name(val) {
      return typeof val == "string";
    },
    age(val) {
      return typeof val == "number" && val > 18;
    },
  };

  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
      return validator(this,person);
    }
  }
  var pp = new Person("lihua", 30);
  console.log(pp); //Proxy {name: "lihua", age: 30}
}

```
