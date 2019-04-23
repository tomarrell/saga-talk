# Lesser known effects

As you can probably already tell, there are a large number of different effects that the Saga Monitor can handle.

Some of these effects however prevent you from needing to build more common logic flows yourself.

## takeMaybe
Useful for intercepting an END action which would automatically terminate sagas which are blocked by take, and instead deal with it manually. This is applicable in server side rendering in order to break out of while(true) loops.

## delay
Useful to block the execution of a saga for a specific amount of time. This may be necessary for a timeout of some sort to prevent some action from occurring too frequently.

## throttle
Is used to spawn a saga based on an incoming action, while also holding a buffer of the most recent additional action call. 

## debounce
Pretty self explanatory, takes a time, a saga and an action to match. Will spawn the saga once it hasn't received a particular action for a period of time after receiving the most recent. Prevent spamming your API if your action is called frequently. E.g. search after typing.

## retry
Takes a number of retries, a delay, and a function with arguments to attempt execution. The Saga Monitor will then attempt to execute the function up to the maximum number of times, after which it will throw an exception which can be handled.
