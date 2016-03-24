

# Formatting #

  * Please do not use tabs, only spaces (4 spaces per tab):
    * Vim:

```
se expandtab
se ts=4
se sw=4
```

  * Avoid lines with more than 78 characters.
    * Vim:

```
se textwidth=78
```

  * Spaces:
    * No space at the end of line (for instance after ';' ending an instruction)
    * '(' and ')'
      * No space before '(' excepts after a statement (e.g. if, for, ...)
      * No space after ')' excepts if followed by '{'
    * ',' and ';'
      * No space before ',', one space after.
    * ':'
      * Labels: no space before, no space afetr
      * Ternary statement:
    * '{' and '}': see "indentation" paragraph.

```
min = x < y ? x : y;
```

# Types #

Booleans:
  * Use "bool" type instead of "int" type.

Integers:
  * Sizes should be of type "size`_`t".
  * Do not forget to precise the "unsigned" keyword if the integer can't be less than 0.
  * Do not use "int" or "unsigned int" if the size of the integer is known, and prefer "uint8`_`t, uint16`_`t, ..."

Pointers:
  * Generic pointer may be declared "void `*`" or "uint8`_`t `*`" depending on the context
  * Do not forget to precise whether a pointer is "const" or not in the function synopsis.

Scope:
  * No global variable.
  * static variables should be used only if they store shared information between the different algorithm instances using libparistraceroute (for example to perform caching or to store "global" information)

# #include #

  * Use <...> and "..." as follows:

```
#include <standard_header.h>
#include "path/to/header/of/libparistraceroute.h"
```

  * Please include first "config.h", then "use.h", then standard headers and at least remaining needed libparistraceroute headers. Example:

```
#include "config.h"
#include "use.h"

#include <stdlib.h>
#include <stdio.h>

#include "probe.h"
```

# Naming #

## #define ##

  * Macro and constants must be written using caps.
  * Constants should be prefixed with the module they are related to. For instance, constants declared in ipv4.c should be prefixed with "IPV4`_`".

## Labels ##

  * Labels should be written using only caps and "`_`".
  * Labels related to an error should be prefixed "ERR`_``*`" and "`*`" should be as most as possible the name of the function triggering the error.
  * Example:

```
void * p;

if (!(p = malloc(10))) {
  goto ERR_MALLOC;
}
```

## Variables and types ##

  * Do not use caps to name variables and types. Structures and enumerations are suffixed as follows:
    * Structure (typedef), enum : ...`_`t
    * Structure (only after struct keyword and only if needed): ...`_`s

```
#define MY_CONSTANT

typedef struct {
  //...
} my_struct_t;

int my_function(int param1, int param2);

typedef enum {
    MY_VALUE_1,
    MY_VALUE_2
} my_enum_t
```


## Functions ##

  * Functions which are not static must be prefixed with the filename related to. For example the function "set\_buffer" in "probe.c" should be named "probe`_`set`_`buffer".
  * Each "foo" "class" of libparistraceroute should propose a "foo\_create" (constructor), "foo`_`free" (destructor), and "foo`_`dump" (print to standard output) function. "foo\_free" and "foo\_dump" should respect the prototype defined in "common.h".
  * A "class" may provide getters (foo`_`get`_`...) and setters (foo\_set`_`...) which must not allocate nor release memory. If your function retrieve a value and must allocate a "bar" instance, you should name it "foo`_`create`_`bar`_`...".

# Indentation #

## Declarations ##

  * Declare variables at the beginning of the function.
  * Pass a line between the variable declarations and the rest of the function.
  * Try to align variables names and their initialization if this improve the readability.
  * Exemple:

```
void foo() {
  const char * s       = "hello";
  char         my_char = 'c';
  uint32_t     uint    = 1;

  printf("%s %c %d\n", s, my_char, my_uint);
}
```

## Algorithm ##

  * As a base rule, the left curly brace goes on the same line as the start of the statement. Example:

```
void my_function() {
    int i;
    for (i = 0; i < 10; ++i) {
         printf("%d\n", i);
    }
}
```

  * Use curly braces even when the body of a conditional statement contains only one line. Or write the body in the same line of the conditional statement.

```
// OK
if (i > 0) printf("%d\n", i);

// OK
if (i > 0) {
    printf("%d\n", i);
}

// Not OK
if (i > 0)
    printf("%d\n", i);
```

# Code style #


  * Lock headers

```
#ifndef FOO_H
#define FOO_H

#include <stdint.h>

// Contents of foo.h
typedef struct {
    field1_t * field1;
    field2_t * field2;
} foo_t;

#endif
```

  * Control memory allocations

```
foo_t * foo_create()
{
    foo_t * foo;

    if (!(foo         = malloc(sizeof(foo_t))))    return ERR_FOO;
    if (!(foo->field1 = malloc(sizeof(field1_t)))) return ERR_FIELD1;
    if (!(foo->field2 = malloc(sizeof(field2_t)))) return ERR_FIELD2;

    return foo;

ERR_FIELD2:
    free(foo->field2);
ERR_FIELD1:
    free(foo->field1);
ERR_FOO:
    free(foo);
    return NULL;
}

void foo_free(foo_t * foo) {
    if (foo) {
        if(foo->field1) field1_free(foo->field1);
        if(foo->field2) field2_free(foo->field2);
        free(foo);
    }
}
```



# Documentation #

Please comment your code in the headers:

```
typedef struct {
  int my_field1; /**< Comment about my_field1 */
  int my_field2; /**< Comment about my_field2 */
} my_struct_t;

/**
 * \brief Aim of my_function
 * \param param1 Meaning of param1
 * \param param2 Meaning of param2
 * \return Meaning of the return value
 */

int my_function(int param1, int param2);

```