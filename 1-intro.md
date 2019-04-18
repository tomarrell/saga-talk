# Sagas are Generators, but special
Sagas are generator functions. generator functions are functions which hold state. They do not run until completion. Sagas are incredibly useful for describing business logic and complicated control flows throughout our applications. They handle both synchronous and asynchronous logic easily and intuitively, once you are familiar with their mechanisms.

A descriptive snippet from the Redux-Saga readme: "The mental model is that a saga is like a separate thread in your application that's solely responsible for side effects. redux-saga is a redux middleware, which means this thread can be started, paused and cancelled from the main application with normal redux actions, it has access to the *full redux application state and it can dispatch redux actions as well*."

I like to point out that Sagas have access to the state, and also dispatch methods on the store. This allows them to tie in closely with actions coming from elsewhere in your application, and also make decisions based on data present in the Redux store.

Let us look at the signature of generator functions. This will help us understand the control flow of Sagas later on.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator

Notice the methods of:
- Generator.prototype.next()
- Generator.prototype.return()
- Generator.prototype.throw()

These are important, as they are used by the Saga monitor to control a series of generator functions, which we will now refer to Sagas when they are controlled by the Saga Monitor.

We will take a brief look at the signatures of the above methods.

---

## next()

```
next<A, B>(value: A) -> { done: boolean, value?: B }
```

The next method takes a value of type any, begins execution of the generator until it reaches the next yield statement, then returns an object representing the current state of the generator. It is important to understand that the value argument becomes the so called "return value" of the yield statement when execution begins again.

The returned object includes a boolean to signify whether the generator has completed its execution, if this is the case, calling next repeatedly will do nothing and simply return the same object. It also may include a value key, whos value can take any type.

---

## return()

```
return<T>(value: T) -> { done: true, value?: T }
```

The return method is similar to the next method, however it ends execution of the generator and will put the generator into the completed state. Any further calls on the generator will respect this state.

If the value argument is passed, it will be set to the value key in the return object. This method is not commonly called manually. The Saga monitor will do this for us.

---

## throw()

```
throw<T>(exception: T) -> { done: boolean, value?: T)
  where T: instanceof Error
```

The throw method is useful when you would like an error to originate within a generator. This takes a value, which is useful to be an instance of Error and will cause an exception containing this value to be throw at the point of execution within the saga.

The return value of this method is the same as what the next() method would have returned in order to reach the current point of execution within the generator.

Again, this method is not frequently called manually, however is used frequently by the Saga monitor in order to hand exceptions in external execution back to the Saga that triggered them.


---

We have now covered all there is to know about generators. Let's have a look at some more Redux-Saga specific functionality.
