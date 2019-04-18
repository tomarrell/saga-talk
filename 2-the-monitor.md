# The Saga Monitor
The Saga Monitor is the execution arbiter of the generator functions that you write, aka: Sagas. It begins by taking a root Saga as input, and attempts to run this to completion. 

However it wouldn't be too useful if your Saga was run to completion at the beginning of your program. Hence the need for control flow statements. These control flow statements come in the form of "effects."

Effects are arguably the most important part of Sagas that the developer will frequently interact with. Effects are yielded to the Saga Monitor, and give it instructions on what to do.

Redux-Saga provides a very extensive selection of effects. These can be easily found in their reference at: https://redux-saga.js.org/docs/api/ 

---

## Effects

Some useful effects used on a daily basis are:
- takeLatest
- call
- fork
- select
- put

When an effect is called, it simply returns a plain object. This plain object can be thought of as a *description about what should occur*.

This description will then almost always be preceded by a yield statement, returning the desription back to the Saga monitor for execution.

This pattern makes Sagas incredibly easy to test, as a simple assertion is all that is needed to test that the functionality is in order. This prevents you from having to spend time on expensive mocking infrastructure.

---

##
