---
layout: post
title: JavaScript for React Developers
tags:
  - JavaScript
---


## 1. var, let, const

```javascript
function sayHello() { 
    for (var i = 0 ; i < 5 ; i++) {
        console.log(i);
    }
    console.log(i);
}
sayHello();
```
var는 let, const와 다른 scope를 갖고 있습니다. var는 function scope 이기 때문에 for 루프 안에서 0, 1, 2, 3, 4를 프린트한 뒤, for 루프 밖에서 마지막으로 증가된 i = 5를 프린트합니다.

만약 i를 var가 아니라 let으로 선언했다면, for 루프 안에서 0, 1, 2, 3, 4를 프린트한 뒤 for 루프를 빠져나가면 i는 정의되지 않은 상태(undefined)가 됩니다. let으로 선언된 i는 for loop의 {} 안에서만 유효하기 때문입니다.

const는 let과 동일하게 block 단위의 scope를 갖습니다. 다른 점은, const으로 선언된 변수는 재할당이 불가하다는 것입니다. 만약 위의 for 루프에서 i를 const type으로 선언하고 i를 1씩 증가시킨다면, 오류가 날 것입니다. 초기에 할당된 const i = 0의 값을 변경할 수 없기 때문이죠.

- var : function
- let : block
- const : block, read-only (재할당 불가)



## 2. objects & 3. this

```javascript
const person = {
    name: 'Mosh',
    // method
    talk() {},
    walk() {
        console.log(this); // 항상 현재 Object에 대한 reference를 리턴
    },
};
person.talk();

person.name = '';
const targetMember = 'name'; // input field of a form
person[targetMember.value] = 'John';

// function is an object
const walk = person.walk.bind(person);

// walk() changes to a reference to a object
// person.walk() != walk() : just walk() is a reference to a global object
```



## 4. arrow funcitons 

```javascript
const square_old = function(number) {
    return number * number;
}
const square_new = number => number * number;
const jobs = [
    { id: 1, isActive: true },
    { id: 2, isActive: true },
    { id: 3, isActive: false },
]

const activeJobs_old = jobs.filter(function(job) { return job.isActive; });
const activeJobs_new = jobs.filter(job => job.isActive);
```



## 5. call-back function

```javascript
const people = {
    talk() {
        // solution to call-back function
        var self = this;

        // execute f, after delay
        // call-back : not part of any object, so 'this' -> window, not this object
        setTimeout(function() {
            console.log('self', self);
        }, 1000); 

        // arrow functions : reference to this object, not rebind this keyword
        setTimeout(() => {
            console.log('this', this);
        }, 1000); 
    }
}
person.talk();
```



## 6. map

```javascript
const colors = ['red', 'green', 'blue'];
const items = colors.map(color => '<li>' + color + '</li>');

// template literal - render dynamically
const items_2 = colors.map(color => <li>${color}</li>); 
```



## 7. object destructor

```javascript
const address = {
    street: '',
    city: '',
    country: ''
};

const street = address.street;
const city = address.city;
const country = address.country;

// object destructor - resolve repetition
const { street, city, country } = address;
const { street: st } = address;

```



## 8. spread operator

```javascript
const first = [1, 2, 3];
const second = [4, 5, 6];
const combined = first.concat(second);

// spread operator - easily clone and insert anything
const combined = [...first, 'a', ...second, 'b'];
const clone = [...first];

const first = { name: 'Mosh' };
const second = { job: 'Instructor' };

const combined = [ 
    ...first, 
    ...second, 
    location: 'Australia',
];

const clone = { ...first };
```



## 9. class

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }
    walk() {
        console.log('walk');
    }
}
const person = new Person('Mosh');
person.walk();
```



## 10. inheritance

```javascript
class Teacher extends Person {
    constructor(name, degree) {
        super(name);
        this.degree = degree;
    }
    teach() {
        console.log("teach");
    }
}
const teacher = new Teacher('Mosh', 'MSc');
```

_extends_ 키워드를 사용해 원하는 클래스를 상속받을 수 있습니다.

위 코드에서 Teacher 클래스는 Person 클래스를 상속받았습니다. 부모 클래스인 Person 클래스에 이미 생성자가 존재하지만, Teacher 클래스에 새로운 생성자를 만들고 싶다면 주의해야할 점이 있습니다.

우선 부모 클래스의 생성자를 보면 "매개변수 name의 값은 이 Person 객체의 name이다." 라고 해서 `this.name = name;`이라고 작성되어 있습니다. 

현재 자식 클래스인 Teacher 클래스의 생성자에 degree라는 매개변수를 추가하고 싶다면, 우선 `super(name);`을 써주어 부모 클래스의 생성자를 호출해야 합니다. 그 뒤에 새로 추가되는 부분인 `this.degree = degree;`를 작성해줍니다.




## 11. module

```javascript
// person.js
export class Person {
    constructor(name) {
		this.name = name;
    }
    walk() {
        console.log('walk');
    }
}

// teacher.js
// private is default, so import and export are required
import { Person } from './person';
export class Teacher extends Person {
    constructor(name, degree) {
        super(name);
        this.degree = degree;
    }
    teach() {
        console.log("teach");
    }
}

// index.js
import { Teacher } from './teacher';

```



## 12. Named and default exports

```javascript
// named export - specific object is exported
// person.js
export class Person {
    constructor(name) {
        this.name = name;
    }
    walk() {
        console.log('walk');
    }
}

// teacher.js

// private is default, so import and export are required
import { Person } from './person';

export function promote() {}
export default class Teacher extends Person {
    constructor(name, degree) {
        super(name);
        this.degree = degree;
    }
    teach() {
        console.log("teach");
    }
}

// index.js
import Teacher, { promote } from './teacher';

// import React, { Component } from 'react';
```

- default : import ... from '';
- named : import { ... } from '';



출처 : https://youtu.be/NCwa_xi0Uuc

