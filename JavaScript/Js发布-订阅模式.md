# 发布-订阅模式

## 1. 定义

发布-订阅模式其实是一种对象间一对多的依赖关系，当一个对象的状态发送改变时，所有依赖于它的对象都将得到状态改变的通知。

## 2. 实现思路

```javascript
class EventEmitter {
  constructor() {
    // key: 事件名
    // value: callback [] 回调数组
    this.events = {};
  }
  on(name, callback) {
    if (this.events[name]) {
      this.events[name].push(callback);
    } else {
      this.events[name] = [callback];
    }
  }
  // 订阅一次
  once(name, callback) {
    const _this = this;
    function onlyOnce() {
      _this.off(name, onlyOnce);
      callback(...arguments);
    }
    this.on(name, onlyOnce);
  }
  off(name, callback) {
    if (!this.events[name]) return;
    if (!callback) {
      // 如果没有callback,就删掉整个事件
      this.events[name] = undefined;
    }
    this.events[name] = this.events[name]?.filter((item) => {
      return item !== callback;
    });
  }
  emit(name, ...args) {
    if (!this.events[name]) return;
    this.events[name].forEach((cb) => cb(...args));
  }
}

const emitter = new EventEmitter();

const user1 = (content) => {
  console.log("用户1订阅了: ", content);
};
const user2 = (content) => {
  console.log("用户2订阅了: ", content);
};
const user3 = (content) => {
  console.log("用户3订阅了: ", content);
};
const user4 = (content) => {
  console.log("用户4订阅了: ", content);
};

// 订阅
emitter.on("article1", user1);
emitter.on("article1", user2);
emitter.on("article1", user3);

// 取消订阅
emitter.off("article1", user2);

emitter.once("article2", user4);

emitter.emit("article1", "发布了一篇新的文章");
emitter.emit("article1", "发布了一篇新的文章");
emitter.emit("article2", "Javascript");
emitter.emit("article2", "Javascript");

```
