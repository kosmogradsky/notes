# Nobody understood Finnish notation

The name of Finnish notation relates to [Hungarian notaion](https://en.wikipedia.org/wiki/Hungarian_notation). Finnish notation is named after [André Staltz](https://twitter.com/andrestaltz), a large contributor to JavaScript reactive programming community who also happens to be Finnish.

Finnish notation is a convention is which names of Observable variables use dollar sign `$` instead of `s` to form plural:
```typescript
// instead of
const clicks: Observable<MouseEvent> = Observable.fromEvent(...)
// by convention we use
const click$: Observable<MouseEvent> = Observable.fromEvent(...)

// instead of
const users: Observable<User> = Observable.ajax(...)
// by convention we use
const user$: Observable<User> = Observable.fromEvent(...)
```
Pros of Finnish notation:
* Helps to distinguish Observables among other variables. This is useful when you don't have static typing in your app, you can instantly see on what you can call `.subscribe()`. This makes it easier to develop apps with heavy use of Observables (examples of such apps are ones using Angular or [Cycle.js](https://cycle.js.org/)).
* Avoids name collision with array and objects that may have the same name. In the same scope you can have an array of `users` and an Observable of `user$`. In such rare cases Finnish notation helps you to not bother.

Cons of Finnish notation:
* Not all English nouns form their plurals using `s`. In this cases you have to make up contrived forms: `people$` or `person$`, `mouse$` or `mice$`, `datum$` or `data$`.
* After some time you might also want to distinguish some other non-primitives like Promises and functions which will actually make code _harder_ to read.
* Nobody understood Finnish notation. Open-source repositories, articles and maybe even your project contain names like `fetchTable$`, `updateDictionary$` и `keyStream$`.