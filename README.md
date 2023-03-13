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
| 14   | [ Implement a general memoization function memo()](#implement-a-general-memoization-function-memo)                                                                                                        | 
| 15   | [ implement a simple DOM wrapper to support method chaining like jQuery](#implement-a-simple-dom-wrapper-to-support-method-chaining-like-jquery)                                                                                                        |
| 16   | [ create an Event Emitter](#create-an-event-emitter)                               
| 17   | [ Create a simple store for DOM element](#create-a-simple-store-for-dom-element)          
| 18   | [ Improve a function](#improve-a-function)      
| 20   | [ Detect data type in JavaScript](#detect-data-type-in-javascript)                                                                                                        | 
| 21   | [ implement JSON.stringify()](#implement-jsonstringify)                                                                                                        | 
| 22   | [ implement JSON.parse()](#implement-jsonparse)                                                        
| 23   | [ create a sum()](#create-a-sum)                                                        
| 24   | [ create a Priority Queue in JavaScript](#create-a-priority-queue-in-javascript)                                                       
1. ###  implement curry()
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
2. ###  implement curry() with placeholder support
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
3. ###  implement Array.prototype.flat()
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
4. ###  implement basic throttle()
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
5. ###  implement throttle() with leading and trailing option
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
6. ###  implement basic debounce()
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
10. ###  first bad version
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
14. ###  Implement a general memoization function memo()
    Memoization is a common technique to boost performance. If you use React, you definitely have used React.memo before.

    Memoization is also commonly used in algorithm problem, when you have a recursion solution, in most cases, you can improve it by memoization, and then you might be able to get a Dynamic Programming approach.

    So could you implement a general memo() function, which caches the result once called, so when same arguments are passed in, the result will be returned right away.
       ```javascript
      const func = (arg1, arg2) => {
        return arg1 + arg2
      }

      const memoed = memo(func)

      memoed(1, 2) 
      // 3, func is called

      memoed(1, 2) 
      // 3 is returned right away without calling func

      memoed(1, 3)
      // 4, new arguments, so func is called
      ```
     The arguments are arbitrary, so memo should accept an extra resolver parameter, which is used to generate the cache key, like what _.memoize() does.
      ```javascript
      const memoed = memo(func, () => 'samekey')

      memoed(1, 2) 
      // 3, func is called, 3 is cached with key 'samekey'

      memoed(1, 2) 
      // 3, since key is the same, 3 is returned without calling func

      memoed(1, 3) 
      // 3, since key is the same, 3 is returned without calling func
      ```
     Default cache key could be just Array.from(arguments).join('_')

     note：

      It is a trade-off of space for time, so if you use this in an interview, please do analyze how much space it might cost.
     
      **solution:**
      ```javascript
      function memo(func, resolver ) {
          const cache = new Map();
          return function(...args) {
            const cacheKey = resolver?resolver(...args):args.join('_');
            if (cache.has(cacheKey)) {
              return cache.get(cacheKey);
            }
            const value = func.apply(this, args);
            cache.set(cacheKey, value);
            return value;
          }
      }

      ```
15. ###  implement a simple DOM wrapper to support method chaining like jQuery
     I believe you've used jQuery before, we often chain the jQuery methods together to accomplish our goals.

      For example, below chained call turns button into a black button with white text.

      ```javascript
      $('#button')
      .css('color', '#fff')
      .css('backgroundColor', '#000')
      .css('fontWeight', 'bold')
      ```
      The chaining makes the code simple to read, could you create a simple wrapper $ to make above code work as expected?

      The wrapper only needs to have css(propertyName: string, value: any)

      **solution:**
      ```javascript
      function $(el) {
        
        return {
          css: function(property, value){
            el.style[property] = value
            return this
          }
        }
      }
      ```
16. ###  create an Event Emitter
    There is Event Emitter in Node.js, Facebook once had its own implementation but now it is archived.

    You are asked to create an Event Emitter Class
       ```javascript
      const emitter = new Emitter()
      ```
      It should support event subscribing
       ```javascript
      const sub1  = emitter.subscribe('event1', callback1)
      const sub2 = emitter.subscribe('event2', callback2)

      // same callback could subscribe 
      // on same event multiple times
      const sub3 = emitter.subscribe('event1', callback1)
      ```
      emit(eventName, ...args) is used to trigger the callbacks, with args relayed
      ```javascript
      emitter.emit('event1', 1, 2);
      // callback1 will be called twice
      ```
      Subscription returned by subscribe() has a release() method that could be used to unsubscribe
      ```javascript
      sub1.release()
      sub3.release()
      // now even if we emit 'event1' again, 
      // callback1 is not called anymore
      ```

      **solution:**
      ```javascript
      class EventEmitter {
        subscriptions = new Map()

        subscribe(eventName, callback) {
            if(!(this.subscriptions.has(eventName))){
              this.subscriptions.set(eventName, new Set())
            }
            const subscriptions = this.subscriptions.get(eventName)
            const callbackObj = { callback }
            subscriptions.add(callbackObj)
            
            return {
              release: ()=>{
                subscriptions.delete(callbackObj)
                if(subscriptions.size === 0 ){
                  delete this.subscriptions.eventName
                }
              }
            }
        }
        
        emit(eventName, ...args) {
          const subscriptions = this.subscriptions.get(eventName)
          if(subscriptions){
            subscriptions.forEach(cbObj => {
              cbObj.callback.apply(this, args)
            })
          }
        }
      }
      ```
17. ###  Create a simple store for DOM element
      We have Map in es6, so we could use any data as key, such as DOM element.

      ```javascript
      const map = new Map()
      map.set(domNode, somedata)
      ```
      What if we need to support old JavaScript env like es5, how would you create your own Node Store as above?

      You are asked to implement a Node Store, which supports DOM element as key.
      ```javascript
      class NodeStore {
        set(node, value) {
        }
        get(node) {
        }
        has(node) {
        }
      }
      ```

      **solution:**
      ```javascript
      class NodeStore {
        constructor() {
          this.nodes = {};
        }
        set(node, value) {
           node.__nodeStoreKey__ = Symbol();
           this.nodes[node.__nodeStoreKey__] = value;
        }
        get(node) {
          return this.nodes[node.__nodeStoreKey__];
        }
        has(node) {
            return !!this.nodes[node.__nodeStoreKey__];
        }
      }
      ```
18. ###  Improve a function
      ```javascript
        // Given an input of array, 
        // which is made of items with >= 3 properties

        let items = [
          {color: 'red', type: 'tv', age: 18}, 
          {color: 'silver', type: 'phone', age: 20},
          {color: 'blue', type: 'book', age: 17}
        ] 

        // an exclude array made of key value pair
        const excludes = [ 
          {k: 'color', v: 'silver'}, 
          {k: 'type', v: 'tv'}, 
          ...
        ] 

        function excludeItems(items, excludes) { 
          excludes.forEach( pair => { 
            items = items.filter(item => item[pair.k] === item[pair.v])
          })
        
          return items
        } 
      ```
      What does this function excludeItems do?

      Is this function working as expected ?

      What is the time complexity of this function?

      How would you optimize it ?

      note

      we only judge by the result, not the time cost. please submit the best approach you can.

      **solution:**
      ```javascript
      function excludeItems(items, excludes) {
        // m k n
        // n * m
        
        // change exclude to Map<key, Set<value>>
        // m * k * 1
        
        // preprocess excludes array
        // avoid multiple for loop on items
        
        const excludeMap = new Map()
        for (let {k, v} of excludes) {
          if (!excludeMap.has(k)) {
            excludeMap.set(k, new Set())
          }
          excludeMap.get(k).add(v)
        }
        
        return items.filter(item => {
          return Object.keys(item).every(
            key => !excludeMap.has(key) || !excludeMap.get(key).has(item[key])
          )
        })
      }

      ```
20. ###  Detect data type in JavaScript
     This is an easy problem.

      For all the basic data types in JavaScript, how could you write a function to detect the type of arbitrary data?

      Besides basic types, you need to also handle also commonly used complex data type including Array, ArrayBuffer, Map, Set, Date and Function

      The goal is not to list up all the data types but to show us how to solve the problem when we need to.

      The type should be lowercase

      ```javascript
      detectType(1) // 'number'
      detectType(new Map()) // 'map'
      detectType([]) // 'array'
      detectType(null) // 'null'

      // more in judging step
      ```
      **solution:**
      ```javascript
      function detectType(data) {
        if(data instanceof FileReader) return 'object';
        return Object.prototype.toString.call(data).slice(1, -1).split(' ')[1].toLowerCase();
      }
      ```
21. ###  implement JSON.stringify()
      medium  1286 accepted / 11950 tried

      I believe you've used JSON.stringify() before, do you know the details of how it handles arbitrary data?

      Please have a guess on the details and then take a look at the explanation on MDN, it is actually pretty complex.

      In this problem, you are asked to implement your own version of JSON.stringify().

      In a real interview, you are not expected to cover all the cases, just decide the scope with interviewer. But for a goal of practicing, your code here will be tested against a lot of data types. Please try to cover as much as you can.

      note:

      JSON.stringify() support two more parameters which is not required here.

      **solution:**
      ```javascript
      function stringify(data) {
        if(typeof data === 'bigint') {
          throw new Error('Do not know how to serialize a BigInt at JSON.stringify');
        } 
        if(typeof data === 'string') {
          return `"${data}"`;
        } 
        if(typeof data === 'function') {
          return undefined;
        }
        if(data !== data) {
          return 'null';
        }
        if(data === Infinity) {
          return 'null';
        }
        if(data === -Infinity) {
          return 'null';
        }
        if(typeof data === 'number') {
        return `${data}`;
        }
        if(typeof data === 'boolean') {
          return `${data}`;
        }
        if(data === null) {
          return 'null';
        }
        if(data === undefined) {
          return 'null';
        }
        if(typeof data === 'symbol') {
          return 'null';
        }
        if(data instanceof Date) {
          return `"${data.toISOString()}"`;
        }
        if(Array.isArray(data)) {
          const arr = data.map((el) => stringify(el));
          return `[${arr.join(',')}]`;
        }
        if(typeof data === 'object') {
          const arr = Object.entries(data).reduce((acc, [key, value]) => {
            if(value === undefined) {
              return acc;
            }
            acc.push(`"${key}":${stringify(value)}`);
            return acc;
          }, [])
          return `{${arr.join(',')}}`;
        }
      }
      ```
22. ###  implement JSON.parse()
      This is a follow-up on 21. implement JSON.stringify().

      Believe you are already familiar with JSON.parse(), could you implement your own version?

      In case you are not sure about the spec, MDN here might help.

      JSON.parse() support a second parameter reviver, you can ignore that.

      note:

      JSON.parse() support two more parameters which is not required here.

      **solution:**
      ```javascript
      function parse(str) {
        if(str === '') {
          throw Error();
        }
        if(str[0] === "'") {
          throw Error();
        }
        if(str === 'null') {
          return null;
        }
        if(str === '{}') {
          return {};
        }
        if(str === '[]') {
          return [];
        }
        if(str === 'true') {
          return true;
        }
        if(str === 'false') {
          return false;
        }
        if(str[0] === '"') {
          return str.slice(1, -1);
        }
        if(+str === +str) {
          return Number(str);
        }
        if(str[0] === '{') {
          return str.slice(1, -1).split(',').reduce((acc, item) => {
            const index = item.indexOf(':');
            const key = item.slice(0, index)
            const value = item.slice(index + 1);
            acc[parse(key)] = parse(value);
            return acc;
          }, {});
        }
        if(str[0] === '[') {
          return str.slice(1, -1).split(',').map((value) => parse(value));
        }
      }
      ```
23. ###  create a sum()
      Create a sum(), which makes following possible
      ```javascript
      const sum1 = sum(1)
      sum1(2) == 3 // true
      sum1(3) == 4 // true
      sum(1)(2)(3) == 6 // true
      sum(5)(-1)(2) == 6 // true
      ```
      **solution:**
      ```javascript
      function sum(num) {
        const func = function(num2) { // #4
          return num2 ? sum(num+num2) : num; // #3
        }
        
        func.valueOf = () => num; // #2
        return func; // #1
      }

      ```
24. ###  create a Priority Queue in JavaScript
      Priority Queue is a commonly used data structure in algorithm problem. Especially useful for Top K problem with a huge amount of input data, since it could avoid sorting the whole but keep a fixed-length sorted portion of it.

      Since there is no built-in Priority Queue in JavaScript, in a real interview, you should tell interview saying that "Suppose we already have a Priority Queue Class I can use", there is no time for you to write a Priority Queue from scratch.

      But it is a good coding problem to practice, so please implement a Priority Queue with following interface
      ```javascript
      class PriorityQueue {
        // compare is a function defines the priority, which is the order
        // the elements closer to first element is sooner to be removed.
        constructor(compare) {
        
        }
        
        // add a new element to the queue
        // you need to put it in the right order
        add(element) {

        }

        // remove the head element and return
        poll() {
        
        }

        // get the head element
        peek() {

        }

        // get the amount of items in the queue
        size() {

        }
      }
      ```
      Here is an example to make it clearer
      ```javascript
      const pq = new PriorityQueue((a, b) => a - b)
        // (a, b) => a - b means
        // smaller numbers are closer to index:0
        // which means smaller number are to be removed sooner

        pq.add(5)
        // now 5 is the only element

        pq.add(2)
        // 2 added

        pq.add(1)
        // 1 added

        pq.peek()
        // since smaller number are sooner to be removed
        // so this gives us 1

        pq.poll()
        // 1 
        // 1 is removed, 2 and 5 are left

        pq.peek()
        // 2 is the smallest now, this returns 2

        pq.poll()
        // 2 
        // 2 is removed, only 5 is left
      ```
      **solution:**
      ```javascript
      class PriorityQueue {
        constructor(compare) {
          this.compare = compare;
          this.arr=[]
        }

        /**
        * return {number} amount of items
        */
        size() {
          return this.arr.length//returning size of arr
        }

        /**
        * returns the head element
        */
        peek() {
          return this.arr[0] //returning first element in array
        }

        /**
        * @param {any} element - new element to add
        */
        add(element) {
        this.arr.push(element); //pushing an element into array
        this.arr.sort(this.compare) //using compare to sort array in ascending or descending order 
        }

        /**
        * remove the head element
        * @return {any} the head element
        */
        poll() {
          let a = this.arr.shift() //removing the first element in arr to return
          return a
        }
      }

      ```
