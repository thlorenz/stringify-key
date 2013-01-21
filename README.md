# stringify-key [![build status](https://secure.travis-ci.org/thlorenz/stringify-key.png)](http://next.travis-ci.org/thlorenz/stringify-key)

Stringifies key objects emitted nodejs [readline](http://nodejs.org/api/readline.html).

## Installation

    npm i stringify-key

## Usage

```js
  var stringifyKey = require('stringify-key');
  var key =  {
    name: c,
    ctrl: true,
    meta: false,
    shift: false
  };
  console.log(stringifyKey(key)); // ctrl-c
```

## Limitations

Although the algorithm works for all keys, the readline module doesn't work consistent in all terminals. For example, on
a `Mac Lion xTerm` the `meta` key is never registered and pressing the `alt` key changes the key name, but does not
cause `alt` to be set to `true`.

The `ctrl` key seems to work properly, except for the following:

- `ctrl-i` interpreted as `tab`
- `ctrl-h` interpreted as `backspace`
- `ctrl-j` and `ctrl-m`, both interpreted as `enter`
- `ctrl-[;',/]` are also not interpreted correctly

The `shift-letter` is correctly registered as well, however pressing `ctrl-shift` and a letter together registers `ctrl`
only. 
Here is the most likely [cause for the latter](https://github.com/joyent/node/blob/master/lib/readline.js#L920), since letters are never
uppercased when `ctrl` is pressed.

I'm not sure if the other problems are caused by the underlying terminal or an incorrect implemention in `readline`.
I suggest to investigate the [readline source](https://github.com/joyent/node/blob/master/lib/readline.js) in order to
get more information and/or fix problems.

However, the moral is to expect unexpected results (npi) when using `stringify-key` for keys emitted by `readline`.
