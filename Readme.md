# æmitter

An asynchronous event emitter for node.js and the browser. Heavily inspired by [component/emitter](https://github.com/component/emitter).

## Installation

Using component:

    component install matthewmueller/aemitter

Using node.js:

    npm install aemitter

## Example

```js
var Emitter = require('aemitter');
var user = { name: 'matt' };
Emitter(user);

user.on('save', function(data, next) {
  // save data
  next()
});

user.emit('save', data, function(err) {
  // called after all `save` callbacks have completed
});
```

## API

### Emitter#on(event, fn)

  Register an `event` handler `fn`. The signature of `fn` will be all the arguments emitted + the `next(err)` function. If this function is not specified, `Emitter#on` will immediately return.

```js
user.on('save', function(next) {
  // asynchronous, will wait for `next` to be called
});

user.on('save', function() {
  // immediately returns
})

user.emit('save');
```

It's important to note that `next` will be passed as the last argument. If `Emitter#emit` passes four arguments, `next` will be it's fifth.

### Emitter#once(event, fn)

  Register a single-shot `event` handler `fn`,
  removed immediately after it is invoked the
  first time.

### Emitter#off(event, fn)

  Remove `event` handler `fn`, or pass only the `event`
  name to remove all handlers for `event`.

### Emitter#emit(event, ..., [fn])

  Emit an `event` with variable option args, and a final, optional callback `fn`. This `fn` will be called when all the event callbacks have finished.

  **Caveat:** You will need to be consistent with the `emit` signature, otherwise `Emitter#on()` callbacks will be inconsistent and likely fail. For example:

```js
user.emit('save', data, message, fn); // main signature
user.emit('save', data, fn); // will NOT work as expected
user.emit('save', data, '', fn); // will work as expected
user.emit('save', data, ''); // will also work as expected
```

### Emitter#listeners(event)

  Return an array of callbacks, or an empty array.

### Emitter#hasListeners(event)

  Check if this emitter has `event` handlers.


## Test

    npm install .
    mocha test

## License

MIT
