# std::Vec&lt;T&gt;

```san
import "vec"
```

This structure is a self-expanding list of elements. You'll not need to allocate memory yourself in order to add more values.

## Static methods

### new()

#### Description

Instantiate a new `std::Vec<T>`.

#### Prototype

```san
fn new() : std::Vec<T>
```

#### Return value

A new instance of `std::Vec<T>`.

#### Example

```san
let vec = std::Vec<i32>::new();
```


## Public properties

### length

#### Description

The current size of the vector. This represents how much values are actually inside the vector.

#### Type

`u64`

#### Example

```san
let vec = std::Vec<i32>::new();

vec.push(5);
vec.push(18);
vec.push(2);

vec.length; // 3
```


## Public methods

### push(const T&)

#### Description

Push a value inside the vector. The value must be of type `T` or convertible to `T`.

#### Prototype

```san
fn push(value: const T&) : u64
```

#### Return value

The updated size of the vector.

#### Example

```san
let vec = std::Vec<i32>::new();

vec.push(1);
vec.push(2);
vec.push(3);
```


### shift()

#### Description

Remove and return the first value of the vector.

#### Prototype

```san
fn shift() : T
```

#### Return value

The first value of the vector (which should be removed from the vector).

#### Example

```san
let vec = std::Vec<i32>::new();

vec.push(5);
vec.push(18);
vec.push(2);

vec.shift(); // 5
vec.shift(); // 18

vec.length; // 1
```


### pop()

#### Prototype

```san
fn pop() : T
```

#### Description

Remove and return the last value of the vector.

#### Return value

The last value of the vector (which should be removed from the vector).

#### Example

```san
let vec = std::Vec<i32>::new();

vec.push(5);
vec.push(18);
vec.push(2);

vec.pop(); // 2
vec.pop(); // 18

vec.length; // 1
```


### filter(cb: fn (T&): bool)

#### Prototype

```san
fn filter(cb: fn (T&): bool) : std::Vec<T>
```

#### Description

Create a new `std::Vec<T>` with only values that returns `true` when passing in the callback.

#### Return value

A vector consisting of filtered values of the same type as the original vector.

#### Examples

<!-- tabs:start -->

#### ** Filter odd values **

```san
let vec = std::Vec<i32>::new();

vec.push(3);
vec.push(4);
vec.push(5);

let odds = vec.filter(fn (value: i32&) : bool {
    return (value % 2) == 1;
});

odds.length; // 2
odds[0]; // 3
odds[1]; // 5
```

#### ** Get words starting by 'n' **

```san
let vec = std::Vec<i8*>::new();

vec.push("hi");
vec.push("no");
vec.push("nutshell");
vec.push("squirrel");
vec.push("nature");

let words = vec.filter(fn (value: i8*&) : bool {
    return value[0] == 'n';
});

words.length; // 3
words[0]; // no
words[1]; // nutshell
words[2]; // nature
```

<!-- tabs:end -->


### reduce&lt;U&gt;(fn (U&, u64, const T&) : U)

#### Prototype

```san
fn reduce<U>(cb: fn (acc: U&, value: const T&) : U, default: U) : U
```

#### Description

Accumulate all values into a single value.

#### Return value

A value of type `U`.

#### Examples

<!-- tabs:start -->

#### ** Sum of all values **

```san
let vec = std::Vec<i32>::new();

vec.push(1);
vec.push(2);
vec.push(3);

let sum = vec.reduce<i32>(fn (acc: i32&, value: const i32&) : i32 {
    return acc += value;
}, 0);

sum; // 6
```

#### ** Sum of odd values **

```san
let vec = std::Vec<i32>::new();

vec.push(1);
vec.push(2);
vec.push(3);

let sum = vec.reduce<i32>(fn (acc: i32&, value: const i32&) : i32 {
    if value % 2 == 1 {
        acc += value;
    }

    return acc;
}, 0);

sum; // 4
```

#### ** Check if all values are even **

```san
let vec = std::Vec<i32>::new();

vec.push(8);
vec.push(4);
vec.push(6);

let even = vec.reduce<bool>(fn (acc: bool&, value: const i32&) : bool {
    return acc && (value % 2 == 0);
}, true);

even; // true
```

#### ** Check if all values are even but as a separated function **

```san
fn is_even(acc: bool&, value: const i32&) : bool {
    return acc && (value % 2 == 0);
}

fn main() {
    let vec = std::Vec<i32>::new();

    vec.push(8);
    vec.push(4);
    vec.push(6);

    let even = vec.reduce<bool>(is_even, true); // true
}
```

<!-- tabs:end -->


### map&lt;U&gt;(fn (T, u64, const std::Vec&lt;T&gt; &) : U)

#### Prototype

```san
fn map<U>(cb: fn (value: T, index: u64, original: const std::Vec<T>&) : U) : std::Vec<U>
```

#### Description

Transform each values of a vector to the function returned values.
The function will pass each values of a vector one by one to the callback function.
It lets the original vector unmodified.

#### Return value

A new vector of type `U` which should be specified in the function's call.

#### Examples

<!-- tabs:start -->

#### ** Transform all numbers in string **

```san
import "string"

fn main() {
    let vec = std::Vec<i32>::new();

    vec.push(3);
    vec.push(4);
    vec.push(5);

    let strings = vec.map<i8*>(fn (value: i32, index: u64, original: const std::Vec<i32>&) : std::Vec<i8*> {
        return std::to_string(value);
    });

    strings[0]; // 3
    strings[1]; // 4
    strings[2]; // 5
}
```

#### ** Multiply all values by 2 **

```san
let vec = std::Vec<i32>::new();

vec.push(3);
vec.push(4);
vec.push(5);

let multiplied_by_2 = vec.map<i32>(fn (value: i32, index: u64, original: const std::Vec<i32>&) : std::Vec<i32> {
    return value * 2;
});

multiplied_by_2[0]; // 6
multiplied_by_2[1]; // 8
multiplied_by_2[2]; // 10
```

<!-- tabs:end -->


### get(const i64&)

#### Description

Get a value by its index.

#### Prototype

```san
fn get(index: const i64&) : T&
```

#### Return value

A non-const reference to the element being at the given index.

#### Example

```san
let vec = std::Vec<i32>::new();

vec.push(1);
vec.push(2);
vec.push(3);

vec.get(0); // 1
vec.get(2); // 3
```


### [const i64&]

#### Description

Operator overloading of `[]` that gets a value by its index.

#### Prototype

```san
fn [](value: const i64&) : T&
```

#### Return value

A non-const reference to the element being at the given index.

#### Example

```san
let vec = std::Vec<i32>::new();

vec.push(1);
vec.push(2);
vec.push(3);

vec[0]; // 1
vec[2]; // 3
```
