# iproute2 Coding Style

iproute2 follows Linux kernel coding style with some exceptions specific to
userspace code. Reference: https://www.kernel.org/doc/html/latest/process/coding-style.html

## Indentation

- Use tabs for indentation (tabs are 8 characters)
- Do not use spaces for indentation (except in continuation lines)
- Switch and case labels are aligned in the same column:

```c
switch (suffix) {
case 'G':
case 'g':
	mem <<= 30;
	break;
case 'M':
case 'm':
	mem <<= 20;
	break;
default:
	break;
}
```

## Line Length

- Preferred limit: 100 columns
- Longer lines are acceptable if breaking them reduces readability
- Never break user-visible strings (e.g., error messages) - they must be grep-able

## Braces

**Functions**: Opening brace on a new line:

```c
int function(int x)
{
	body of function
}
```

**Control structures** (if, switch, for, while, do): Opening brace on same line:

```c
if (x is true) {
	we do y
}
```

**Single statements**: Do not use braces where a single statement will do:

```c
if (condition)
	action();
```

**Consistency rule**: If one branch needs braces, use braces on all branches:

```c
if (condition) {
	do_this();
	do_that();
} else {
	otherwise();
}
```

## Spaces

Use a space after these keywords:

	if, switch, case, for, do, while

Do NOT use a space after:

	sizeof, typeof, alignof, __attribute__

Examples:
```c
s = sizeof(struct file);      /* correct */
s = sizeof( struct file );    /* WRONG */
```

Use one space around binary and ternary operators:
```c
=  +  -  <  >  *  /  %  |  &  ^  <=  >=  ==  !=  ?  :
```

No space after unary operators:
```c
&  *  +  -  ~  !  sizeof  typeof  alignof  __attribute__
```

No space around `.` and `->` structure member operators.

## Pointer Declarations

The `*` is adjacent to the variable name, not the type:

```c
char *linux_banner;                          /* correct */
char* linux_banner;                          /* WRONG */
unsigned long long memparse(char *ptr, char **retptr);
```

## Naming

- Use lowercase with underscores for names: `count_active_users`
- Do NOT use CamelCase or Hungarian notation
- Global variables and functions need descriptive names
- Local variables should be short: `i` for loop counter, `tmp` for temporary
- Avoid `master/slave` and `blacklist/whitelist` terminology

## File Headers

Every source file must start with an SPDX license identifier:

```c
/* SPDX-License-Identifier: GPL-2.0-or-later */
/*
 * filename.c		Brief description
 *
 * Authors:	Name <email@example.org>
 */
```

## Comments

Multi-line comments use this format:

```c
/*
 * This is the preferred style for multi-line
 * comments in iproute2 source code.
 *
 * Description: A column of asterisks on the left side,
 * with beginning and ending almost-blank lines.
 */
```

Comments should explain WHAT the code does, not HOW. Avoid comments inside
function bodies - if needed, the function may be too complex.

## Variable Declarations

**Note**: Unlike the Linux kernel, iproute2 does NOT require "Christmas tree"
(reverse Christmas tree / reverse fir tree) style for variable declarations.
Variables do not need to be ordered by decreasing line length.

Acceptable:
```c
int ret;
struct nlmsghdr *answer;
const char *filter_dev = NULL;
__u32 filt_mask = IFLA_STATS_FILTER_BIT(IFLA_STATS_AF_SPEC);
```

Use one declaration per line to allow for comments on each item.

## Structure Initialization

Use designated initializers for structures:

```c
struct iplink_req req = {
	.n.nlmsg_len = NLMSG_LENGTH(sizeof(struct ifinfomsg)),
	.n.nlmsg_flags = NLM_F_REQUEST,
	.i.ifi_family = preferred_family,
};
```

## Functions

- Functions should be short and do one thing
- Limit local variables to 5-10 per function
- Separate functions with one blank line
- Use goto for centralized cleanup (see below)

## Centralized Exit with goto

Use goto for cleanup when a function has multiple exit points:

```c
int fun(int a)
{
	int result = 0;
	char *buffer;

	buffer = malloc(SIZE);
	if (!buffer)
		return -ENOMEM;

	if (condition1) {
		while (loop1) {
			...
		}
		result = 1;
		goto out_free_buffer;
	}
	...
out_free_buffer:
	free(buffer);
	return result;
}
```

Use descriptive label names like `out_free_buffer:` not `err1:`.

## Macros

- Macro names defining constants should be CAPITALIZED
- Enums are preferred over #define for related constants
- Prefer inline functions over function-like macros
- Multi-statement macros must use do-while:

```c
#define MACROFUN(a, b, c)		\
	do {				\
		if (a == 5)		\
			do_this(b, c);	\
	} while (0)
```

## Error Messages

Use `fprintf(stderr, ...)` for error messages. This is critical to avoid
corrupting JSON output on stdout:

```c
fprintf(stderr, "Error: argument of \"%s\" must be \"on\" or \"off\", not \"%s\"\n",
	msg, realval);
```

Do NOT use `printf()` or `fprintf(stdout, ...)` for error messages.
