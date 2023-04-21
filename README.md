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
| 19   | [ find corresponding node in two identical DOM tree](#find-corresponding-node-in-two-identical-dom-tree)      
| 20   | [ Detect data type in JavaScript](#detect-data-type-in-javascript)                                                                                                        | 
| 21   | [ implement JSON.stringify()](#implement-jsonstringify)                                                                                                        | 
| 22   | [ implement JSON.parse()](#implement-jsonparse)                                                        
| 23   | [ create a sum()](#create-a-sum)                                                        
| 24   | [ create a Priority Queue in JavaScript](#create-a-priority-queue-in-javascript)        
| 25   | [ Reorder array with new indexes](#reorder-array-with-new-indexes)        
| 26   | [ implement Object.assign()](#implement-objectassign)                                              
| 27   | [ implement completeAssign()](#implement-completeassign)   
| 28   | [ implement clearAllTimeout()](#implement-clearalltimeout) 
| 29   | [ implement async helper sequence()](#implement-async-helper-sequence)   
| 30   | [ implement async helper parallel()](#implement-async-helper-parallel)   
| 31   | [ implement async helper race()](#implement-async-helper-race)   
| 32   | [ implement Promise.all()](#implement-promiseall)   
| 33   | [ implement Promise.allSettled()](#implement-promiseallsettled)   
| 34  | [ implement Promise.any()](#implement-promiseany)   
| 35  | [ implement Promise.race()](#implement-promiserace)   
| 36  | [ create a fake timer setTimeout](#create-a-fake-timer-settimeout)  
| 37  | [ implement Binary Search](#implement-binary-search)  
| 38  | [ implement jest.spyOn()](#implement-jestspyon)  
| 39  | [ implement range()](#implement-range)  
| 40  | [ implement Bubble Sort](#implement-bubble-sort)  
| 41  | [ implement Merge Sort](#implement-merge-sort)  
| 42  | [ implement Insertion Sort](#implement-insertion-sort)  
| 43  | [ implement Quick Sort](#implement-quick-sort)  
| 44  | [ implement Selection Sort](#implement-selection-sort)  
| 45  | [ find the K-th largest element in an unsorted array](#find-the-k-th-largest-element-in-an-unsorted-array)  
| 46  | [ implement _.once()](#implement-_once)  
| 47  | [ reverse a linked list](#reverse-a-linked-list)  
| 48  | [ search first index with Binary Search](#search-first-index-with-binary-search)  
| 49  | [ search last index with Binary Search](#search-last-index-with-binary-search)  
| 50  | [ search element right before target with Binary Search](#search-element-right-before-target-with-binary-search)  
| 51  | [ search element right after target with Binary Search](#search-element-right-after-target-with-binary-search)  
| 52  | [ create a middleware system](#create-a-middleware-system) 

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
19. ###  find corresponding node in two identical DOM tree
      Given two same DOM tree A, B, and an Element a in A, find the corresponding Element b in B.

      By corresponding, we mean a and b have the same relative position to their DOM tree root.

      follow up

      This could a problem on general Tree structure with only children.

      Could you solve it recursively and iteratively?

      Could you solve this problem with special DOM api for better performance?

      What are the time cost for each solution?

      **solution:**
      ```javascript
      const findCorrespondingNode = (rootA, rootB, target) => {
        const stack = [[rootA, rootB]];
        while(stack.length > 0) {
          const [leftNode, rightNode] = stack.pop();
          if (leftNode === target) return rightNode;
          for (let i = 0; i < leftNode.children.length; i++) {
            stack.push([leftNode.children[i], rightNode.children[i]]);
          }
        }
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
25. ###  Reorder array with new indexes
     Suppose we have an array of items - A, and another array of indexes in numbers - B
      ```javascript
      const A = ['A', 'B', 'C', 'D', 'E', 'F']
      const B = [1,   5,   4,   3,   2,   0]
      ```
      You need to reorder A, so that the A[i] is put at index of B[i], which means B is the new index for each item of A.

      For above example A should be modified inline to following
      ```javascript
      ['F', 'A', 'E', 'D', 'C', 'B']
      ```
      **solution:**
      ```javascript
      function sort(items, newOrder) {
        return items.sort((a, b) => newOrder[items.indexOf(a)] - newOrder[items.indexOf(b)]);
      }

      ```
26. ###  implement Object.assign()
     The Object.assign() method copies all enumerable own properties from one or more source objects to a target object. It returns the target object. (source: MDN)

      It is widely used, Object Spread operator actually is internally the same as Object.assign() (source). Following 2 lines of code are totally the same.
      ```javascript
      let aClone = { ...a };
      let aClone = Object.assign({}, a);
      ```
      **solution:**
      ```javascript
      function objectAssign(target, ...sources) {
        if (target === null || target === undefined) {
          throw new Error('Not an object')  
        }
        
        if (typeof target !== `object`) {
          target = new target.__proto__.constructor(target)  
        }
        
        for (const source of sources) {
          if (source === null || source === undefined) {
            continue
          }
          
          Object.defineProperties(target, Object.getOwnPropertyDescriptors(source))
          
          for (const symbol of Object.getOwnPropertySymbols(source)) {
            target[symbol] = source[symbol]
          } 
        }
        return target
      }

      ```
27. ###  implement completeAssign()
      This is a follow-up on 26. implement Object.assign().

      Object.assign() assigns the enumerable properties, so getters are not copied, non-enumerable properties are ignored.

      Suppose we have following source object.

      It is widely used, Object Spread operator actually is internally the same as Object.assign() (source). Following 2 lines of code are totally the same.
      ```javascript
      const source = Object.create(
        {
          a: 3 // prototype
        },
        {
          b: {
            value: 4,
            enumerable: true // enumerable data descriptor
          },
          c: {
            value: 5, // non-enumerable data descriptor
          },
          d: { // non-enumerable accessor descriptor 
            get: function() {
              return this._d;
            },
            set: function(value) {
              this._d = value
            }
          },
          e: { // enumerable accessor descriptor 
            get: function() {
              return this._e;
            },
            set: function(value) {
              this._e = value
            },
            enumerable: true
          }
        }
      )
      ```
      If we call Object.assign() with source of above, we get:
       ```javascript
      Object.assign({}, source)

      // {b: 4, e: undefined}
      // e is undefined because `this._e` is undefined
      ```
      Rather than above result, could you implement a completeAssign() which have the same parameters as Object.assign() but fully copies the data descriptors and accessor descriptors?

      **solution:**

      ```javascript
      function completeAssign(target, ...sources) {
       if (target == null) throw Error('target cannot be null or undefined');
        target = Object(target);
        
        for(let source of sources) {
          if (source == null) continue;
          Object.defineProperties(target, Object.getOwnPropertyDescriptors(source))
          
          for (const symbol of Object.getOwnPropertySymbols(source)) {
            target[symbol] = source[symbol]
          } 
        }
        return target
      }

      ```
28. ###  implement clearAllTimeout()
      window.setTimeout() could be used to schedule some task in the future.

      Could you implement clearAllTimeout() to clear all the timers ? This might be useful when we want to clear things up before page transition.

      For example
      ```javascript
      
      setTimeout(func1, 10000)
      setTimeout(func2, 10000)
      setTimeout(func3, 10000)

      // all 3 functions are scheduled 10 seconds later
      clearAllTimeout()

      // all scheduled tasks are cancelled.
      ```
      
      note

      You need to keep the interface of window.setTimeout and window.clearTimeout the same, but you could replace them with new logic
      **solution:**

      ```javascript
      function clearAllTimeout() {
        // your code here
        let id = setTimeout(null, 0);
        while(id>=0){
          window.clearTimeout(id);
          id--;
        }
      }

      ```
29. ###  implement async helper sequence()
      This problem is similar to 11. what is Composition? create a pipe().

      You are asked to implement an async function helper, sequence() which chains up async functions, like what pipe() does.

      All async functions have following interface
      ```javascript
      type Callback = (error: Error, data: any) => void

      type AsyncFunc = (
        callback: Callback,
        data: any
      ) => void
      ```
      Your sequence() should accept AsyncFunc array, and chain them up by passing new data to the next AsyncFunc through data in Callback.

      Suppose we have an async func which just multiple a number by 2 
      ```javascript
        const asyncTimes2 = (callback, num) => {
          setTimeout(() => callback(null, num * 2), 100)
        }
      ```
      Your sequence() should be able to accomplish this
       ```javascript
        const asyncTimes4 = sequence(
          [
            asyncTimes2,
            asyncTimes2
          ]
        )

        asyncTimes4((error, data) => {
          console.log(data) // 4
        }, 1)
      ```
      Once an error occurs, it should trigger the last callback without triggering the uncalled functions.

      **solution:**

      ```javascript
      function sequence(funcs){
        const funcsQueue = [...funcs];
        
        return function run(finalCB, data) {
          if (funcsQueue.length === 0) {
            finalCB(undefined, data);
            return;
          }
          
          const currF = funcsQueue.shift();
          
          const cb = (error, num) => {
            if (error) {
              finalCB(error, num);
              return
            }
            
            run(finalCB, num);
          };
          
          currF(cb, data);
        };
      }
      ```
30. ###  implement async helper parallel()
     This problem is related to 29. implement async helper - sequence().

      You are asked to implement an async function helper, parallel() which works like Promise.all(). Different from sequence(), the async function doesn't wait for each other, rather they are all triggered together.

      All async functions have following interface
      ```javascript
      type Callback = (error: Error, data: any) => void

      type AsyncFunc = (
        callback: Callback,
        data: any
      ) => void
      ```
      Your parallel() should accept AsyncFunc array, and return a new function which triggers its own callback when all async functions are done or an error occurs.

      Suppose we have an 3 async functions
      ```javascript
      const async1 = (callback) => {
        callback(undefined, 1)
      }

      const async2 = (callback) => {
        callback(undefined, 2)
      }

      const async3 = (callback) => {
        callback(undefined, 3)
      }
      ```
      Your parallel() should be able to accomplish this
       ```javascript
        const all = parallel(
          [
            async1,
            async2,
            async3
          ]
        )

        all((error, data) => {
          console.log(data) // [1, 2, 3]
        }, 1)
      ```
      When error occurs, only first error is passed down to the last. Later errors or data are ignored.

      **solution:**

      ```javascript
      const promisify = fn => input => new Promise((res, rej) => {
        fn((err, output) => err ? rej(err) : res(output), input)
      })

      function parallel(fns) {
        return (cb, input) => {
          Promise
            .all(fns.map(fn => promisify(fn)(input)))
            .then(outputs => cb(undefined, outputs))
            .catch(err => cb(err, undefined))
        }
      }
      ```
31. ###  implement async helper race()
      This problem is related to 30. implement async helper - parallel().

      You are asked to implement an async function helper, race() which works like Promise.race(). Different from parallel() that waits for all functions to finish, race() will finish when any function is done or run into error.

      All async functions have following interface
      ```javascript
      type Callback = (error: Error, data: any) => void

      type AsyncFunc = (
        callback: Callback,
        data: any
      ) => void
      ```
      Your race() should accept AsyncFunc array, and return a new function which triggers its own callback when any async function is done or an error occurs.

      Suppose we have an 3 async functions
      ```javascript
      const async1 = (callback) => {
        setTimeout(() => callback(undefined, 1), 300)
      }

      const async2 = (callback) => {
          setTimeout(() => callback(undefined, 2), 100)
      }

      const async3 = (callback) => {
        setTimeout(() => callback(undefined, 3), 200)
      }
      ```
      Your parallel() should be able to accomplish this
       ```javascript
        const all = parallel(
          [
            async1,
            async2,
            async3
          ]
        )

        all((error, data) => {
          console.log(data) // [1, 2, 3]
        }, 1)
      ```
      When error occurs, only first error is passed down to the last. Later errors or data are ignored.

      **solution:**

      ```javascript
      function race(funcs){
        return function(cb) {
          let handled = false;
          funcs.forEach((func) => {
            func((e, v) => {
              if (!handled) {
                handled = true;
                cb(e, v)
              }
            })
          });
        };
      }
      ```
32. ###  implement Promise.all()
      The Promise.all() method takes an iterable of promises as an input, and returns a single Promise that resolves to an array of the results of the input promises

      Could you write your own all() ? which should works the same as Promise.all()

      note

      Do not use Promise.all() directly, it is not helping
     
      **solution:**
      ```javascript
      async function all(promises) {
        let result = []
        for(let promise of promises){
          result.push(await promise)
        }
        return result;
      }
      ```
33. ###  implement Promise.allSettled()
      The Promise.allSettled() method returns a promise that resolves after all of the given promises have either fulfilled or rejected, with an array of objects that each describes the outcome of each promise.

      Different from Promise.all() which rejects right away once an error occurs, Promise.allSettled() waits for all promises to settle.

      Now can you implement your own allSettled() ?

      note

      Do not use Promise.allSettled() directly, it helps nothing.
     
      **solution:**
      ```javascript
      function allSettled(promises) {
          return Promise.all(promises.map(p => Promise.resolve(p).then((value) => {
            return {
              status: 'fulfilled',
              value
            };
          }, (reason) => {
            return {
              status: 'rejected',
              reason
            };
          })));
        }
      ```
34. ###  implement Promise.any()
      Promise.any() takes an iterable of Promise objects and, as soon as one of the promises in the iterable fulfils, returns a single promise that resolves with the value from that promise

      Can you implement a any() to work the same as Promise.any()?

      note

      AggregateError is not supported in Chrome yet, but you can still use it in your code since we will add the Class into your code. Do something like following:

      ```javascript
      new AggregateError(
        'No Promise in Promise.any was resolved', 
        errors
      )
      ```
      **solution:**
      ```javascript
      function any(promises) {
        if(!promises.length)
          throw new AggregateError('No Promise passed')
        
        return new Promise((resolve, reject) => {
          let settledCount = 0, errors = [];
          promises.forEach((promise, index)=>
            promise
              .then(data => resolve(data))
              .catch(err => {
                errors[index] = err
                if(++settledCount === promises.length)
                  reject(new AggregateError(
                    'No Promise in Promise.any was resolved',
                    errors
                  ))
              })
          )
        })
      }
      ```
35. ###  implement Promise.race()
      The Promise.race() method returns a promise that fulfills or rejects as soon as one of the promises in an iterable fulfills or rejects, with the value or reason from that promise.

      Can you create a race() which works the same as Promise.race()?

      **solution:**
      ```javascript
      function race(promises) {
        return new Promise((resolve, reject)=>{
          promises.forEach((promise)=>{
            promise.then(resolve,reject);
          })
        })
      }
      ```
36. ###  create a fake timer setTimeout
      setTimeout adds task in to a task queue to be handled later, the time actually is no accurate. (Event Loop).

      This is OK in general web application, but might be problematic in test.

      For example, at 5. implement throttle() with leading & trailing option we need to test the timer with more accurate approach.

      Could you implement your own setTimeout() and clearTimeout() to be sync? so that they have accurate timing for test. This is what FakeTimes are for.

      By "accurate", it means suppose all functions cost no time, we start our function at time 0, then setTimeout(func1, 100) would schedule func1 exactly at 100.

      You need to replace Date.now() as well to provide the time.
      ```javascript
      class FakeTimer {
        install() {
          // setTimeout(), clearTimeout(), and Date.now() 
          // are replaced
        }

        uninstall() {
          // restore the original APIs
          // setTimeout(), clearTimeout() and Date.now()
        }

        tick() {
          // run all the schedule functions in order
        }
      }
      ```
      Your code is tested like 
      ```javascript
        const fakeTimer = new FakeTimer()
          fakeTimer.install()

          const logs = []
          const log = (arg) => {
            logs.push([Date.now(), arg])
          }

          setTimeout(() => log('A'), 100)
          // log 'A' at 100

          const b = setTimeout(() => log('B'), 110)
          clearTimeout(b)
          // b is set but cleared

          setTimeout(() => log('C'), 200)

          expect(logs).toEqual([[100, 'A'], [200, 'C']])

          fakeTimer.uninstall()
      ```
      **solution:**
      ```javascript
      class FakeTimer {
        constructor() {
          this.original = {
            setTimeout: window.setTimeout,
            clearTimeout: window.clearTimeout,
            dateNow: Date.now,
          }
          this.timerId = 1;
          this.currentTime = 0;
          this.queue = [];
        }
        install() {
          window.setTimeout = (cb, time, ...args) => {
            const id = this.timerId++;
            this.queue.push({
              id,
              cb,
              time: time + this.currentTime,
              args,
            });
            this.queue.sort((a, b) => a.time - b.time);
            return id;
          }
          window.clearTimeout = (removeId) => {
            this.queue = this.queue.filter(({ id }) => id !== removeId);
          }
          Date.now = () => {
            return this.currentTime;
          }
        }
        
        uninstall() {
          window.setTimeout = this.original.setTimeout;
          window.clearTimeout = this.original.clearTimeout;
          Date.now = this.original.dateNow;
        }
        
        tick() {
          while(this.queue.length) {
            const { cb, time, args } = this.queue.shift();
            this.currentTime = time;
            cb(...args);
          }
        }
      }
      ```
37. ###  implement Binary Search
      Even in Front-End review, basic algorithm technique like Binary Search are likely to be asked.

      You are given an unique & ascending array of integers, please search for its index with Binary Search.

      If not found, return -1

      note

      Please don't use Array.prototype.indexOf(), it is not our goal.

      **solution:**
      ```javascript
      function binarySearch(arr, target){
        let left = 0;
        let right = arr.length - 1;
        while (left <= right) {
          const mid = Math.floor((left + right)) / 2;

          if (arr[mid] === target) {
            return mid;
          }
          if (arr[mid] < target) {
            right = mid - 1;
          } else {
            left = mid + 1;
          }
        }

        return -1;
      }
      ```
38. ###  implement jest.spyOn()
      If you did unit test before, you must be familiar with Spy.

      You are asked to create a spyOn(object, methodName), which works the same as jest.spyOn().

      To make it simple, here are the 2 requirements of spyOn

      original method should be called when spied one is called
      spy should have a calls array, which holds all the arguments in each call.
      Code to explain everything.

      ```javascript
      const obj = {
        data: 1, 
        increment(num) {
            this.data += num
        }
      }

      const spy = spyOn(obj, 'increment')

      obj.increment(1)

      console.log(obj.data) // 2

      obj.increment(2)

      console.log(obj.data) // 4

      console.log(spy.calls)
      // [ [1], [2] ]
      ```
      **solution:**
      ```javascript
      function spyOn(obj, methodName) {
        const calls = []

        const originMethod = obj[methodName]
        
        if (typeof originMethod !== 'function') {
          throw new Error('not function')
        }
        
        obj[methodName] = function(...args) {
          calls.push(args)
          return originMethod.apply(this, args)
        }
        
        return {
          calls
        }
      }
      ```
39. ###  implement range()
      Can you create a range(from, to) which makes following work?
      ```javascript
      for (let num of range(1, 4)) {
        console.log(num)  
      }
      // 1
      // 2
      // 3
      // 4
      ```
      **solution:**
      ```javascript
      function range(from, to) {
          let result = []
          while(from <= to)
            result.push(from++)
          return result
      }
            
      ```
40. ###  implement Bubble Sort
      Even for Front-End Engineer, it is a must to understand how basic sorting algorithms work.

      Now you are asked to implement Bubble Sort, which sorts an integer array in ascending order.

      Do it in-place, no need to return anything.

      Follow-up

      What is time cost for average / worst case ? Is it stable?

      **solution:**
      ```javascript
      function bubbleSort(arr) {
        const len = arr.length;

        for (let i = 0; i < len; i++) {
          for (let j = 0; j < len; j++) {
            if (arr[j] > arr[j + 1]){
              [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
          }
        }

        return arr;
      }
      ```
41. ###  implement Merge Sort
      Even for Front-End Engineer, it is a must to understand how basic sorting algorithms work.

      Now you are asked to implement Merge Sort, which sorts an integer array in ascending order.

      Do it in-place, no need to return anything.

      Follow-up

      What is time cost for average / worst case ? Is it stable?

      **solution:**
      ```javascript
      function mergeSort(arr) {
        if (arr.length < 2) return;
        let mid = Math.floor(arr.length/2);
        let left = arr.slice(0, mid);
        let right = arr.slice(mid);
        mergeSort(left);
        mergeSort(right);
        let l = 0, r = 0;
        while (l < left.length || r < right.length) {
          if (r == right.length || (l < left.length && left[l] <= right[r]))
            arr[l + r] = left[l++];
          else
            arr[l + r] = right[r++];
        }
      }
      ```
42. ###  implement Insertion Sort
      Even for Front-End Engineer, it is a must to understand how basic sorting algorithms work.

      Now you are asked to implement Insertion Sort, which sorts an integer array in ascending order.

      Do it in-place, no need to return anything.

      Follow-up

      What is time cost for average / worst case ? Is it stable?

      **solution:**
      ```javascript
      function insertionSort(arr) {
        for(let i = 1; i < arr.length; i++) {
          for(let j = 0; j < i; j++) {
            const insert = arr[i];
            const curr = arr[j];
            if(curr > insert) {
              [arr[i], arr[j]] = [arr[j], arr[i]];
            }
          }
        }
      }

      ```
43. ###  implement Quick Sort
      Even for Front-End Engineer, it is a must to understand how basic sorting algorithms work.

      Now you are asked to implement Quick Sort, which sorts an integer array in ascending order.

      Do it in-place, no need to return anything.

      Follow-up

      What is time cost for average / worst case ? Is it stable?

      **solution:**
      ```javascript
      function quickSort(arr, l=0, r=arr.length-1) {
        if (l>r) return;
        const pIndex=partition(arr,l,r);
        quickSort(arr,l,pIndex-1);
        quickSort(arr,pIndex+1,r);
      }

      function partition(arr,l,r) {
        const pivot=l++;
        while (l<=r) {
          if (arr[l]<=arr[pivot])
            l++;
          else {
            [arr[l], arr[r]]=[arr[r], arr[l]];
            r--;
          }
        }
        [arr[pivot], arr[r]]=[arr[r], arr[pivot]];
        return r;
      }
      ```
44. ###  implement Selection Sort
      Even for Front-End Engineer, it is a must to understand how basic sorting algorithms work.

      Now you are asked to implement Selection sort, which sorts an integer array in ascending order.

      Do it in-place, no need to return anything.

      Follow-up

      What is time cost for average / worst case ? Is it stable?

      **solution:**
      ```javascript
      function selectionSort(arr) {
        for (let i = 0; i < arr.length; i++) {
          let min = i;
          for (let j = i; j < arr.length; j++) {
            if (arr[j] < arr[min]) {
              min = j;
            }
          }
          [arr[i], arr[min]] = [arr[min], arr[i]];
        }
      }
      ```
45. ###  find the K-th largest element in an unsorted array
      You are given an unsorted array of numbers, which might have duplicates, find the K-th largest element.

      The naive approach would be sort it first, but it costs O(nlogn), could you find a better approach?

      Maybe you can recall what is happening in Quick Sort or Priority Queue
      **solution:**
      ```javascript
      function findKThLargest(arr, k) {
        // your code here
        arr = arr.sort((a,b)=>{return a-b})
        return arr[arr.length-k]
      }
      ```
46. ###  implement `_.once()`
      _.once(func) is used to force a function to be called only once, later calls only returns the result of first call.

      Can you implement your own once()?
       ```javascript
      function func(num) {
        return num
      }
      const onced = once(func)
      onced(1) 
      // 1, func called with 1
      onced(2)
      // 1, even 2 is passed, previous result is returned 
      ```

      **solution:**
      ```javascript
      function once(func) {
        let result= null;
        let isCalled = false;
        return function(...args){
          if(!isCalled){
            result = func.call(this, ...args);
            isCalled = true;
          }
          return result;
        }
      }
      ```
47. ###  reverse a linked list
      Another basic algorithm even for Front End developers.

      You are asked to reverse a linked list.

      Suppose we have Node interface like this
       ```javascript
      class Node {
        new(val: number, next: Node);
        val: number
        next: Node
      }
      ```
      We can then chain nodes together to create a linked list.
       ```javascript
      const Three = new Node(3, null)
      const Two = new Node(2, Three)
      const One = new Node(1, Two)

      //now we have  a linked list
      // 1 → 2 → 3
      ```
      Now how can you reverse it to 3 → 2 → 1 ? you can modify the next property of each node, but not the val.
      **solution:**
      ```javascript
      const reverseLinkedList = (list) => {
        let node = list, prev = null;
        while (node !== null) {
          [node.next, node, prev] = [prev, node.next, node];
        }
        return prev;
      }
      ```
48. ###  search first index with Binary Search
      This is a variation of 37. implement Binary Search (unique).

      Your are given a sorted ascending array of number, but might have duplicates, you are asked to return the first index of a target number.

      If not found return -1.
      **solution:**
      ```javascript
      function firstIndex(arr, target){
        // your code here
        let start = 0, end = arr.length - 1;
        while(start <= end) {
          const mid = start + Math.floor((end - start) / 2);

          if(arr[mid] === target) {
            if(arr[mid - 1] !== target) return mid;
            end = mid - 1;
          }else if(arr[mid] < target) start = mid + 1;
          else end = mid - 1;
        }

        return -1;
      }
      ```
49. ###  search last index with Binary Search
      This is a variation of 37. implement Binary Search (unique).

      Your are given a sorted ascending array of number, but might have duplicates, you are asked to return the last index of a target number.

      If not found return -1.
      **solution:**
      ```javascript
      function lastIndex(arr, target){
        let left = 0, right = arr.length - 1
        while(left < right){
          const mid = (left + right +1) >> 1
          if(arr[mid] <= target){
            left = mid
          }else{
            right = mid - 1
          }
        }
        return arr[left] === target? left : -1
      }
      ```
50. ###  search element right before target with Binary Search
    This is a variation of 37. implement Binary Search (unique).

    Your are given a sorted ascending array of number, but might have duplicates, you are asked to return the element right before first appearance of a target number.

    If not found return undefined.
      **solution:**
      ```javascript
      function elementBefore(arr, target){
        // your code here
        let start = 0, end = arr.length - 1;
        while(start <= end){
          const mid = start + Math.floor((end - start) / 2);
          if(arr[mid] === target){
            if(arr[mid - 1] !== target) return arr[mid - 1];
            end = mid - 1;
          }else if(arr[mid] < target) start = mid + 1;
          else end = mid - 1;
        }
      }
      ```
51. ###  search element right after target with Binary Search
      This is a variation of 37. implement Binary Search (unique).

      Your are given a sorted ascending array of number, but might have duplicates, you are asked to return the element right after last appearance of a target number.

      If not found return undefined.
      **solution:**
      ```javascript
      function elementAfter(arr, target){
        // your code here
        let start = 0, end = arr.length - 1;
        while(start < end){
          const mid = start + Math.ceil((end - start) / 2);
          if(arr[mid] <= target){
            start = mid
          } 
          else end = mid - 1;
        }
        return arr[start] ===target? arr[start+1] : undefined
      }
      ```
51. ###  create a middleware system
      Have you ever used Express Middleware before?

      Middleware functions are functions with fixed interface that could be chained up like following two functions.
      **solution:**
      ```javascript
      class Middleware {
        /**
        * @param {MiddlewareFunc} func
        */
        constructor() {
          this.callbacks = [];
          this.errHandlers = [];
        }

        use(func) {
          if(func.length === 2) {
            this.callbacks.push(func);
          }
          if(func.length === 3) {
            this.errHandlers.push(func);
          }
        }

        /**
        * @param {Request} req
        */
        start(req) {
          let idx = 0;
          let errIdx = 0;
          let self = this;
          function next(nextError) {
            let args = [req, next];
            let func;
            if (nextError) {
              func = self.errHandlers[errIdx++];
              args.unshift(nextError);
            } else {
              func = self.callbacks[idx++];
            }
            try {
              func && func(...args);
            } catch(error) {
              next(error);
            }
          }
          next();
        }
      }
      ```









