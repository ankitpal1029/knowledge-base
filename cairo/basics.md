# Basic Cairo

Calculate sum of elements of an array:

```rust
# This is a comment
func array_sum(arr: felt*, size) -> (sum: felt):
    if size == 0:
        return (sum = 0)
    end

    let (sum_of_rest) = array_sum(arr=arr+1, size=size-1)
    return (sum=[arr] + sum_of_rest)
end
```

- felt implies field element
- [...] is the dereference operator, returns address of the variable
- can't really use loops in cairo, memory is immutable
- loops possible but limited, can't call functions within loop

## Assert

- Verify that two values are the same
- Also assign value to memory

```rust
assert [ptr] = 0
```

- Sets value of the memory cell `[ptr]` to 0, if not set before
- Because of immutability of cairo's memory, if ptr was set before then assert checks if value matches otherwise  
  sets the pointer to the given value

## Output, Imports and Main Function

```rust
%builtins output

from starkware.cairo.common.serialize import serialize_word

func main{output_ptr : felt*}():
    serialize_word(1234)
    serialize_word(4321)
    return ()
end
```

- builtins output is like cout in cpp
- `func main{output_ptr : felt*}():` is an implicit arguement set because of builtin output, adds `return value` and corresponding `arguement`
- `output_ptr` points to beginning of memory to which output to be written

### Serialize_word

- it lets you output some value `x`, takes `output_ptr` as implicit arguement
- writes `x` to memory cell of `output_ptr`, [output_ptr] that is and returns `output_ptr + 1`

### Imports

- compiles imported file and makes available the mentioned value

```rust
from starkware.cairo.common.serialize import serialize_word as foo
```

This is also valid

- `%builtins output` isn't available for --layout plain so make sure to run the below to execute cairo  
  programs

```bash
cairo-compile array_sum.cairo --output array_sum_compiled.json

cairo-run --program=array_sum_compiled.json --print_output --layout=small
```

## Field Element

- when you don't specify type this is default
- number between -P/2 < < P/2, P is very large prime number 252 bit/76 digit, if there is overflow modulo P is taken

### Division in Field elements

- when numerator is multiple of denominator all fine (6/3 = 2)
- but when it's not in that case you get back a value (7/3 not 2.333 some 120...96) in the -P/2 to P/2 range such that if you multiply by 3 it will overflow and the result when mod P will give 7

## Making Arrays and Populating

```rust
%builtins output

from starkware.cairo.common.alloc import alloc
from starkware.cairo.common.serialize import serialize_word

func array_sum(arr, size) -> (sum):
    # ...
end

func main{output_ptr : felt*}():
    const ARRAY_SIZE = 3

    # Allocate an array.
    let (ptr) = alloc()

    # Populate some values in the array.
    assert [ptr] = 9
    assert [ptr + 1] = 16
    assert [ptr + 2] = 25

    # Call array_sum to compute the sum of the elements.
    let (sum) = array_sum(arr=ptr, size=ARRAY_SIZE)

    # Write the sum to the program output.
    serialize_word(sum)

    return ()
end
```

- First allocating space using `alloc()`
- Populate values in the array using `assert`
