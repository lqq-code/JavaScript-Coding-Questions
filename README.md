# JavaScript-Coding-Questions
As a Front-End developer, JavaScript is the core skill of everything

### Table of Contents

| No. | Questions                                                                                                                                                         |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [ implement curry()](#implement-curry)                                         |
| 2   | [ implement curry() with placeholder support](#implement-curry-with-placeholder-support)                                                                                             |
| 3   | [ implement Array.prototype.flat()](#implement-arrayprototypeflat)                                                                                                                                   |
| 4   | [ implement basic throttle()](#implement-basic-throttle)                                                                                                                                   |
| 5   | [ implement throttle() with leading and trailing option](#implement-throttle-with-leading-and-trailing-option)                                                                                                        |
| 6   | [ implement basic debounce()](#implement-basic-debounce)                                                                                                        |
| 7   | [ implement debounce() with leading and trailing option](#implement-debounce-with-leading-and-trailing-option)                                                                                                        |
| 8   | [ can you shuffle() an array](#can-you-shuffle-an-array)                                                                                                        | 
| 9   | [ decode message](#decode-message)                                                                                                        | 
| 10   | [ first bad version](#first-bad-version)                                                                                                        | 
| 11   | [ what is Composition create a pipe()](#what-is-composition-create-a-pipe)                                                                                                        | 
| 12   | [ implement Immutability helper](#implement-immutability-helper)                                                                                                        | 
| 13   | [ Implement a Queue by using Stack](#implement-a-queue-by-using-stack)                                                                                                        | 
1. ### implement curry()
      Currying is a useful technique used in JavaScript applications.

      Please implement a curry() function, which accepts a function and return a curried one.

      Here is an example：

      ```javascript
      const join = (a, b, c) => {
         return `${a}_${b}_${c}`
      }

      const curriedJoin = curry(join)

      curriedJoin(1, 2, 3) // '1_2_3'

      curriedJoin(1)(2, 3) // '1_2_3'

      curriedJoin(1, 2)(3) // '1_2_3'
      ```
      **solution:**
      ```javascript
      function curry(fn) {
        return function curryInner(...args) {
          if (args.length >= fn.length) return fn(...args);
          return (...args2) => curryInner(...args, ...args2);
        };
      }
      ```
      
2. ### implement curry() with placeholder support
      This is a follow-up on 1. implement curry()

      please implement curry() which also supports placeholder.

      Here is an example

      ```javascript
     const  join = (a, b, c) => {
        return `${a}_${b}_${c}`
     }

     const curriedJoin = curry(join)
     const _ = curry.placeholder

     curriedJoin(1, 2, 3) // '1_2_3'

     curriedJoin(_, 2)(1, 3) // '1_2_3'

     curriedJoin(_, _, _)(1)(_, 3)(2) // '1_2_3'
      ```
      **solution:**
      ```javascript
      function curry(func) {
        const _ = curry.placeholder;
        const length = func.length;
        function curried(...argus) {
            if (argus.length >= length && !argus.slice(0, length).includes(_)) return func.call(this, ...argus);
            return (...newArgus) => {
                const formatArgus = argus.map((arg) => arg === _ && newArgus.length ? newArgus.shift() : arg);
                return curried(...formatArgus, ...newArgus);
            }
        }

    return curried;
    }

    curry.placeholder = Symbol()

      ```
      
3. ### implement Array.prototype.flat()
      There is already Array.prototype.flat() in JavaScript (ES2019), which reduces the nesting of Array.

      Could you manage to implement your own one?

      Here is an example to illustrate:

      ```javascript
     const arr = [1, [2], [3, [4]]];

     flat(arr)
     // [1, 2, 3, [4]]

     flat(arr, 1)
     // [1, 2, 3, [4]]
 
    flat(arr, 2)
     // [1, 2, 3, 4]
      ```
      **solution:**
      ```javascript
      function flat(arr, depth = 1) {
        const result = []
        const stack = [...arr.map(item => ([item, depth]))]
        
        while (stack.length > 0) {
          const [top, depth] = stack.pop()
          if (Array.isArray(top) && depth > 0) {
            stack.push(...top.map(item => ([item, depth - 1])))
          } else {
            result.push(top)
          }
        }
        
        return result.reverse()
      }
      ```
4. ### implement basic throttle()
     Throttling is a common technique used in Web Application, in most cases using lodash solution would be a good choice.

      could you implement your own version of basic throttle()?

      In case you forgot, throttle(func, delay) will return a throttled function, which will invoke the func at a max frequency no matter how throttled one is called.

      Here is an example.

      Before throttling we have a series of calling like

      ─A─B─C─ ─D─ ─ ─ ─ ─ ─ E─ ─F─G

      After throttling at wait time of 3 dashes

      ─A─ ─ ─C─ ─ ─D ─ ─ ─ ─ E─ ─ ─G

      Be aware that

      call A is triggered right way because not in waiting time
      function call B is swallowed because B, C is in the cooling time from A, and C is latter.
      notes

      please follow above spec. the behavior is not exactly the same as lodash.throttle()

      because window.setTimeout and window.clearTimeout are not accurate in browser environment, they are replaced to other implementation when judging your code. They still have the same interface, and internally keep track of the timing for testing purpose.

      ```javascript
      let currentTime = 0

      const run = (input) => {
        currentTime = 0
        const calls = []

        const func = (arg) => {
          calls.push(`${arg}@${currentTime}`)
        }

        const throttled = throttle(func, 3)
        input.forEach((call) => {
          const [arg, time] = call.split('@')
          setTimeout(() => throttled(arg), time)
        })
        return calls
      }

    expect(run(['A@0', 'B@2', 'C@3'])).toEqual(['A@0', 'C@3'])
      ```
      **solution:**
      ```javascript
      function throttle(func, wait) {
        let timer = null
        let stashed = null
        
        const startCooling=()=>{
          timer = window.setTimeout(check, wait)
        }

        const check=()=>{
          timer = null
          if(stashed != null){
            func.apply(stashed[0], stashed[1])
            stashed = null
            startCooling()
          }
        }

        return funcion(...args){
          if(timer !== null){
            stashed = [this, args]
          }else{
            func.apply(this, args)
            startCooling()
          }
        }
        
      }

      ```
5. ### implement throttle() with leading and trailing option
    This is a follow up on 4. implement basic throttle(), please refer to it for detailed explanation.

    In this problem, you are asked to implement a enhanced throttle() which accepts third parameter, option: {leading: boolean, trailing: boolean}

    leading: whether to invoke right away
    trailing: whether to invoke after the delay.
    4. implement basic throttle() is the default case with {leading: true, trailing: true}.

    Explanation:

    for the previous example of throttling by 3 dashes

    ─A─B─C─ ─D─ ─ ─ ─ ─ ─ E─ ─F─G

    with {leading: true, trailing: true}, we get as below

    ─A─ ─ ─C─ ─ ─D ─ ─ ─ ─ E─ ─ ─G

    with {leading: false, trailing: true}, A and E are swallowed.

    ─ ─ ─ ─C─ ─ ─D─ ─ ─ ─ ─ ─ ─G

    with {leading: true, trailing: false}, only A D E are kept

    ─A─ ─ ─ ─D─ ─ ─ ─ ─ ─ E

    with {leading: false, trailing: false}, of course, nothing happens.

    Something like below will be used to do the test.


      ```javascript
      let currentTime = 0

      const run = (input) => {
        currentTime = 0
        const calls = []

        const func = (arg) => {
          calls.push(`${arg}@${currentTime}`)
        }

        const throttled = throttle(func, 3)
        input.forEach((call) => {
          const [arg, time] = call.split('@')
          setTimeout(() => throttled(arg), time)
        })
        return calls
      }

      expect(run(['A@0', 'B@2', 'C@3'])).toEqual(['A@0', 'C@3'])
      ```
      **solution:**
      ```javascript
     function throttle(func, wait, option = {leading: true, trailing: true}) {

        let timer = null
        let stashed = null
        
        const startCooling = () => {
          timer = window.setTimeout(check, wait)
        }
        
        const check = () => {
          timer = null
          if (stashed !== null) {
            func.apply(stashed[0], stashed[1])
            stashed = null
            startCooling()
          }
        }
        
        return function(...args) {
          if (timer !== null) {
            // cooling, stash it
            if (option.trailing) {
              stashed = [this, args]
            }
            return
          } 
          
          if (option.leading) {
            func.apply(this, args)
            startCooling()
            return
          } 

          if (option.trailing) {
            stashed = [this, args]
            startCooling()
          }  
        }
    }

      ```
6. ### implement basic debounce()
     Debounce is a common technique used in Web Application, in most cases using lodash solution would be a good choice.

      could you implement your own version of basic debounce()?

      In case you forgot, debounce(func, delay) will returned a debounced function, which delays the invoke.

      Here is an example.

      Before debouncing we have a series of calling like

      ─A─B─C─ ─D─ ─ ─ ─ ─ ─E─ ─F─G

      After debouncing at wait time of 3 dashes

      ─ ─ ─ ─ ─ ─ ─ ─ D ─ ─ ─ ─ ─ ─ ─ ─ ─ G

      notes

      please follow above spec. the behavior might not be exactly the same as lodash.debounce()

      because window.setTimeout and window.clearTimeout are not accurate in browser environment, they are replaced to other implementation when judging your code. They still have the same interface, and internally keep track of the timing for testing purpose.

      Something like below will be used to do the test.

      ```javascript
     let currentTime = 0

      const run = (input) => {
        currentTime = 0
        const calls = []

        const func = (arg) => {
          calls.push(`${arg}@${currentTime}`)
        }

        const debounced = debounce(func, 3)
        input.forEach((call) => {
          const [arg, time] = call.split('@')
          setTimeout(() => debounced(arg), time)
        })
        return calls
      }

      expect(run(['A@0', 'B@2', 'C@3'])).toEqual(['C@5'])
      ```
      **solution:**
      ```javascript
     function debounce(func, wait) {
        let timer = null
        return function(...args) {
          window.clearTimeout(timer)
          timer = window.setTimeout(() => {
            func.call(this, ...args)
          }, wait)
        }
      }
      ```

7. ###  implement debounce() with leading and trailing option
     This is a follow up on 6. implement basic debounce(), please refer to it for detailed explanation.

      In this problem, you are asked to implement an enhanced debounce() which accepts third parameter, option: {leading: boolean, trailing: boolean}

      leading: whether to invoke right away
      trailing: whether to invoke after the delay.
      6. implement basic debounce() is the default case with {leading: false, trailing: true}.

      for the previous example of debouncing by 3 dashes

      ─A─B─C─ ─D─ ─ ─ ─ ─ ─ E─ ─F─G

      with {leading: false, trailing: true}, we get as below

      ─ ─ ─ ─ ─ ─ ─ ─D─ ─ ─ ─ ─ ─ ─ ─ ─ G

      with {leading: true, trailing: true}:

      ─A─ ─ ─ ─ ─ ─ ─D─ ─ ─E─ ─ ─ ─ ─ ─G

      with {leading: true, trailing: false}

      ─A─ ─ ─ ─ ─ ─ ─ ─ ─ ─E

      with {leading: false, trailing: false}, of course, nothing happens.

      notes

      please follow above spec. the behavior might not be exactly the same as lodash.debounce()

      because window.setTimeout and window.clearTimeout are not accurate in browser environment, they are replaced to other implementation when judging your code. They still have the same interface, and internally keep track of the timing for testing purpose.

      Something like below will be used to do the test.

      ```javascript
     let currentTime = 0

      const run = (input) => {
        currentTime = 0
        const calls = []

        const func = (arg) => {
          calls.push(`${arg}@${currentTime}`)
        }

        const debounced = debounce(func, 3)
        input.forEach((call) => {
          const [arg, time] = call.split('@')
          setTimeout(() => debounced(arg), time)
        })
        return calls
      }

      expect(run(['A@0', 'B@2', 'C@3'])).toEqual(['C@6'])
      ```
      **solution:**
      ```javascript
      function debounce(func, wait, option = {leading: false, trailing: true}) {
        let timer = null

        return function(...args) {

          let isInvoked = false
          // if not cooling down and leading is true, invoke it right away
          if (timer === null && option.leading) {
            func.call(this, ...args)
            isInvoked = true
          }

          // no matter what, timer needs to be reset
          window.clearTimeout(timer)
          timer = window.setTimeout(() => {
            if (option.trailing && !isInvoked) {
              func.call(this, ...args)
            }

            timer = null
          }, wait)
        }
      }
      ```
8. ###  can you shuffle() an array
    How would you implement a shuffle() ?

    When passed with an array, it should modify the array inline to generate a randomly picked permutation at the same probability.

    for an array like this:

      ```javascript
      const arr = [1, 2, 3, 4]
      ```
      there would be possibly 4! = 24 permutations
      ```javascript
      [1, 2, 3, 4]
      [1, 2, 4, 3]
      [1, 3, 2, 4]
      [1, 3, 4, 2]
      [1, 4, 2, 3]
      [1, 4, 3, 2]
      [2, 1, 3, 4]
      [2, 1, 4, 3]
      [2, 3, 1, 4]
      [2, 3, 4, 1]
      [2, 4, 1, 3]
      [2, 4, 3, 1]
      [3, 1, 2, 4]
      [3, 1, 4, 2]
      [3, 2, 1, 4]
      [3, 2, 4, 1]
      [3, 4, 1, 2]
      [3, 4, 2, 1]
      [4, 1, 2, 3]
      [4, 1, 3, 2]
      [4, 2, 1, 3]
      [4, 2, 3, 1]
      [4, 3, 1, 2]
      [4, 3, 2, 1]
      ```
      your shuffle() should transform the array in one of the above array, at the same 1/24 probability.

      **solution:**
      ```javascript
      function shuffle(arr) {
        for (let i = 0; i < arr.length; i++) {
          const j = i + Math.floor(Math.random() * (arr.length - i))
          ;[arr[i], arr[j]] = [arr[j], arr[i]]
        }
      }
      ```
9. ###  decode message
      Your are given a 2-D array of characters. There is a hidden message in it.

      ```javascript
      I B C A L K A
      D R F C A E A
      G H O E L A D   
      ```
      The way to collect the message is as follows

      - start at top left
      - move diagonally down right
      - when cannot move any more, try to switch to diagonally up right
      - when cannot move any more, try switch to diagonally down right, repeat 3
      -  stop when cannot neither move down right or up right. the character on the path is the message
      
      for the input above, IROCLED should be returned. 
      **solution:**
      ```javascript
      function decode(message) {
        let i = 0, j = 0;
        let ans = '';
        while (message[i] && message[i][j]) {
          ans += message[i][j];
          message[i + 1] ? i++ : i--
          j++;
        }
        return ans;
      }         
      ```
10. ### first bad version
    Say you have multiple versions of a program, write a program that will find and return the first bad revision given a isBad(version) function.

    Versions after first bad version are supposed to be all bad versions.

    notes：

      - Inputs are all non-negative integers

      - if none found, return -1

      **solution:**
      ```javascript
      function firstBadVersion(isBad) {
        const check = (v) => isBad(v)? check(v-1): v+1

        return (version) => {
          return isBad(version)? check(version) : -1
        }
      }
      ```
11. ###  what is Composition create a pipe()
    Here you are asked to create a pipe() function, which chains multiple functions together to create a new function.

    Suppose we have some simple functions like this
       ```javascript
      const times = (y) =>  (x) => x * y
      const plus = (y) => (x) => x + y
      const subtract = (y) =>  (x) => x - y
      const divide = (y) => (x) => x / y
      ```
    Your pipe() would be used to generate new functions
       ```javascript
      pipe([
        times(2),
        times(3)
      ])  
      // x * 2 * 3

      pipe([
        times(2),
        plus(3),
        times(4)
      ]) 
      // (x * 2 + 3) * 4

      pipe([
        times(2),
        subtract(3),
        divide(4)
      ]) 
      // (x * 2 - 3) / 4
      ```
      notes：

      to make things simple, functions passed to pipe() will all accept 1 argument
      **solution:**
      ```javascript
      function pipe(funcs) {
        return function(arg){
          return funcs.reduce((prev, curr) => curr.call(this, prev),arg)
        }
      }
      ```
12. ###  implement Immutability helper
    If you use React, you would meet the scenario to copy the state for a slight change.

    For example, for following state
       ```javascript
      const state = {
        a: {
          b: {
            c: 1
          }
        },
        d: 2
      }
      ```
      if we are to modify d to a new state, we could use _.cloneDeep, but it is not efficient because state.a is cloned while we don't need to change that.

      A better way is to do shallow copy like this
       ```javascript
      const newState = {
        ...state,
        d: 3
      }
      ```
      now is the problem, if we want to modify c, we would have to do something like
      ```javascript
      const newState = {
        ...state,
        a: {
          ...state.a,
          b: {
            ...state.b,
            c: 2
          }
        }
      }
      ```
      We can see that for simple data structure it would be enough to use spread operator, but for complex data structures, it is verbose.

      **solution:**
      ```javascript
      function update(data, command) {
        for (const [key, value] of Object.entries(command)) {
          switch (key) {
            case '$push':
              return [...data, ...value];
            case '$set':
              return value;
            case '$merge':
              if (!(data instanceof Object)) {
                throw Error("bad merge");
              }
              return {...data, ...value};
            case '$apply':
              return value(data);
            default:
              if (data instanceof Array) {
                const res = [...data];
                res[key] = update(data[key], value);
                return res;
              } else {
                return {
                  ...data,
                  [key]: update(data[key], value)
                }
              }
          }
        }
      }
      ```
13. ###  Implement a Queue by using Stack
    In JavaScript, we could use array to work as both a Stack or a queue.
       ```javascript
      const arr = [1, 2, 3, 4]

      arr.push(5) // now array is [1, 2, 3, 4, 5]
      arr.pop() // 5, now the array is [1, 2, 3, 4]
      ```
      Above code is a Stack, while below is a Queue
       ```javascript
      const arr = [1, 2, 3, 4]

      arr.push(5) // now the array is [1, 2, 3, 4, 5]
      arr.shift() // 1, now the array is [2, 3, 4, 5]
      ```
      now suppose you have a stack, which has only follow interface:
      ```javascript
      class Stack {
        push(element) { /* add element to stack */ }
        peek() { /* get the top element */ }
        pop() { /* remove the top element */}
        size() { /* count of elements */}
      }
      ```
      Could you implement a Queue by using only above Stack? A Queue must have following interface
      ```javascript
      class Queue {
        enqueue(element) { /* add element to queue, similar to Array.prototype.push */ }
        peek() { /* get the head element*/ }
        dequeue() { /* remove the head element, similar to Array.prototype.pop */ }
        size() { /* count of elements */ }
      }
      ```
      **solution:**
      ```javascript
      class Queue {
        constructor() {
          this.stack1 = new Stack();
          this.stack2 = new Stack();
        }
        enqueue(element) { 
          while(this.stack1.size()){
            this.stack2.push(this.stack1.pop())
          }
          
          this.stack1.push(element);
          
           while(this.stack2.size()){
            this.stack1.push(this.stack2.pop())
          }
          
        }
        peek() { 
          return this.stack1.peek();
        }
        size() { 
          return this.stack1.size();
        }
        dequeue() {
          return this.stack1.pop();
        }
      }
      ```
