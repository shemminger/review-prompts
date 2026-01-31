# Command-Line Argument Parsing

## Legacy Code vs New Code

**Critical distinction for argument parsing:**

- **Legacy code**: Uses `matches()` function which allows abbreviations
- **New code**: Must use `strcmp()` for exact string comparison only

The `matches()` function has caused problems with abbreviations in the past.
For all new code, use only full string comparison:

```c
/* CORRECT - New code should use strcmp() */
if (strcmp(*argv, "dev") == 0) {
	NEXT_ARG();
	/* ... */
}

/* LEGACY - matches() allows abbreviations, DO NOT use in new code */
if (matches(*argv, "broadcast") == 0) {  /* matches "b", "br", "bro", etc. */
	/* ... */
}
```

## Argument Processing Macros

Use the standard macros for argument iteration:

```c
NEXT_ARG();           /* Move to next argument, exit with error if none */
NEXT_ARG_OK();        /* Check if next argument exists */
PREV_ARG();           /* Move back one argument */
```

## Common Patterns

```c
while (argc > 0) {
	if (strcmp(*argv, "dev") == 0) {
		NEXT_ARG();
		dev = *argv;
	} else if (strcmp(*argv, "mtu") == 0) {
		NEXT_ARG();
		if (get_unsigned(&mtu, *argv, 0))
			invarg("Invalid \"mtu\" value\n", *argv);
	} else if (strcmp(*argv, "help") == 0) {
		usage();
	} else {
		fprintf(stderr, "Unknown argument: %s\n", *argv);
		exit(-1);
	}
	argc--; argv++;
}
```

## Standard Error Helpers

```c
invarg("invalid value", *argv);     /* Invalid argument value */
duparg("device", *argv);            /* Duplicate argument */
duparg2("dev", *argv);              /* Duplicate argument variant */
missarg("required argument");       /* Missing required argument */
nodev(devname);                     /* Device not found */
```

## Documentation

### Usage Functions

Each command should have a `usage()` function that displays help:

```c
static void usage(void) __attribute__((noreturn));

static void usage(void)
{
	fprintf(stderr,
		"Usage: ip address {add|change|replace} IFADDR dev IFNAME\n"
		"       ip address del IFADDR dev IFNAME\n"
		"       ip address show [ dev IFNAME ]\n"
		...);
	exit(-1);
}
```

## No Kernel Docbook Format

iproute2 does **not** use the kernel's docbook documentation format.
Function documentation should use simple C comments:

```c
/*
 * Brief description of what the function does.
 *
 * Longer description if needed.
 * Returns 0 on success, negative on failure.
 */
static int my_function(int argc, char **argv)
```
