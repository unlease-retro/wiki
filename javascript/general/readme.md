# General

### Semi-colons

> Never use them!

```
# good!
import x from 'x'
const a = b

# bad
import x from 'x';
const a = b;
```

### Comma spacing

> Always space comma-separated values

```
# good!
(a, b) => a

# bad
(a,b) => a
```

### Equality operator

> Always use triple equals

```
# good!
a === b || a !== b

# bad
a == b || a != b
```

### Indent

> 2 space indent

```
# good!
a => {
  return a
}

# bad
a => {
    return a
}
```

### Quotes

> Always use single quotes

```
# good!
const a = 'a'

# bad
const a = "a"
```

### Keyword spacing

> Always space keywords

```
# good!
function (a) {

  if (a) {

    return a

  }

}

# bad
function(a) {

  if(a) {

    return a

  }

}
```

### Pad blocks

> Always pad blocks

```
# good!
function () {

  return

}

# bad
function () {
  return
}
```
