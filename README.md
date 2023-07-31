# cutest-utilities

*Utility helpers for use with the "CuTest" Embedded C Unit-Testing Framework*

## Usage

* Add repo to your project as git submodule, or grab a copy of the files and import them manually

## Utility Description

### Lambda Expressions ([`lambda.h`](lambda.h))

This module provides some macro magic to implement "lambda-like" expressions, similar to C++ lambdas, by utilising [GCC *statement expressions*](https://gcc.gnu.org/onlinedocs/gcc/Statement-Exprs.html).

Since this will usually result in GCC complaining about trampoline functions, additional macros `BEGIN_` and `END_LAMBDA_SECTION()` are provided to temporarily disable those warnings. Trampoline function generation warnings can be globally disabled using the `-Wno-trampolines` compiler switch.

*Usage example:*
```C
// Some data structure
typedef struct {
    int id;
    const char* value;
} data_t;

// Some data array
data_t my_data_array[NUM_ITEMS] = { ... };

// Generic search using predicate callback to check an item for match
const data_t* find_if(const data_t* array, size_t n, bool (*predicate)(const data_t*));

// Function call using lambda as predicate
BEGIN_LAMBDA_SECTION();
const data_t* result = find_if(my_data_array, NUM_ITEMS, LAMBDA((const data_t* d), bool, {
    return (d->id == 10);
}));
END_LAMBDA_SECTION();
```

