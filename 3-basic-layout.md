# Simple layout
The layout of your Sagas should look something to the following:

.
├── settings
│   └── saga.ts
├── home
│   └── saga.ts
├── chat
│   └── saga.ts
└── rootSaga.ts

With each of the ducks of your application specifying a separate set of Sagas, all to be composed within the rootSaga. Then being attached during the construction of your store and the `run` method called on the Monitor.

It is now important to understand the composition of Sagas. Specifically, attached and detached Sagas.

---

## Attached Sagas
Attached Sagas have the following rules.

They will terminate when:
- It terminates its own body of instructions
- All attached forks are themselves terminated

Cancellation of a parent will cancel all attached forks. It will also trigger cancellation at the main task i.e. (the current effect that is being executed within the saga).

## Detached Sagas
Detached Sagas have the following rules.

They will terminate when:
- It terminates its own body of instructions

"Detached forks execute in their own context. A parent doesn't wait for detached forks to terminate. Uncaught errors from spawned tasks are not bubbled up to the parent. And cancelling a parent doesn't automatically cancel detached forks (you need to cancel them explicitly)."

---

## Structure of duck Saga

The structure of a duck saga looks commonly like the following:

```
import { takeEvery, call, put } from 'redux-saga/effects';

import { SUBMIT_LOGIN } from './actions'

function* handleLogin(action) {
  try {
    const user = yield call(fetchLogin)
    // Check user is valid
    yield put(loginSuccess(user))
  } catch (e) {
    put(loginFailure('Failed to login, please try again later'))
  }
}

function* authSagas() {
  yield all([
    takeLatest(SUBMIT_LOGIN, handleLogin),
  ])
}

export default authSagas;
```

Where you have a single root saga per duck, which composes the internal sagas. We call this the "watcher" saga.
