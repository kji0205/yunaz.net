<template>
<div>
  <h1>객체</h1>
  <p>모든 객체는 어떤 다른 객체, 즉 프로토타입과 연관되어 있다.</p>
  <br>
  <h3>prototype, getPrototypeOf, __proto__의 차이점을 이해하자.</h3>
  <p>C.prototype은 new C()로 <mark>생성된 객체의 프로토타입을 만드는 데 사용</mark>된다.</p>
  <p>Object.getPropertyOf(obj)는 obj의 프로토타입 객체를 가져오기 위한 <mark>표준 ES5 메커니즘</mark>이다.</p>
  <p>obj.__proto__는 obj의 프로토타입 객체를 가져오는 비표준 메커니즘이다.</p>
  <br>
  <p>자바스크립트에서 클래스는 기본적으로 생성자 함수(User)와 클래스의 인스턴스 간에 메서드를 공유하기 위해 사용되는 프로토타입 객체(User.prototype)의 조합이다.</p>
  <p>클래스는 생성자 함수와 연관된 프로토타입으로 구성된 설계 패턴이다.</p>
  <br>
  <h3>__proto__를 절대 수정하지 마라</h3>
</div>
</template>

<script>
export default {
  name: "js2"
}

function User(name, passwordHash) {
  this.name=name;
  this.passwordHash=passwordHash
}
function hash(p){
  return p;
}

User.prototype.toString=function () {
  return "[User "+this.name+"]"
}
User.prototype.checkPassword=function (password) {
  return hash(password)===this.passwordHash
}
let u=new User("sfalken", "0ef33ae79sdfasdfwsdfsdfsd")
console.log(User.prototype)
console.log(Object.getPrototypeOf(u))
console.log(u.__proto__)
</script>

<style scoped>

</style>
