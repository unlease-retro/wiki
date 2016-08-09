# Testing

Unlease uses the Tape test framework - [here's why](https://medium.com/javascript-scene/why-i-use-tape-instead-of-mocha-so-should-you-6aa105d8eaf4).

Tests should:
- Only test one thing
- Use simple equality checks `tape.equal`, `tape.deepEqual`, `tape.ok(Immutable.is)`
- Follow this template:

```js
import test from 'tape'

// For each unit test you write,
// answer these questions:
test('What component aspect are you testing?', t => {

  const actual = 'What is the actual output?'
  const expected = 'What is the expected output?'

  t.equal(actual, expected,
    'What should the feature do?')

  t.end()

})
```
