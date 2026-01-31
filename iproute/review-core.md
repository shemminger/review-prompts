# iproute2 Review Core

This is the core review prompt for iproute2 patches. It provides a checklist
and guidance for AI-assisted code review.

## Project Context

iproute2 is a collection of userspace utilities for controlling and monitoring
networking in the Linux kernel. It includes tools like `ip`, `tc`, `ss`, `bridge`,
and others.

**Critical**: iproute2 is userspace code, not kernel code. While the coding style
is similar to the Linux kernel, some kernel-specific conventions do not apply.

## Review Checklist

When reviewing patches, check for:

### 1. Coding Style
- [ ] Tabs for indentation (8 characters)
- [ ] Line length ≤100 columns (flexible for readability)
- [ ] User-visible strings not broken across lines
- [ ] Proper brace placement (K&R style for control, new line for functions)
- [ ] Pointer declarations with `*` adjacent to variable name
- [ ] SPDX license identifier present in new files

### 2. Argument Parsing
- [ ] New code uses `strcmp()`, not `matches()` for argument comparison
- [ ] Proper use of `NEXT_ARG()`, `NEXT_ARG_OK()`, `PREV_ARG()`
- [ ] Error helpers used correctly: `invarg()`, `duparg()`, `missarg()`

### 3. JSON Output
- [ ] All output uses `print_XXX()` helpers, not raw `fprintf(fp, ...)`
- [ ] Error messages go to stderr, not stdout
- [ ] `open_json_object()` / `close_json_object()` properly paired
- [ ] `open_json_array()` / `close_json_array()` properly paired
- [ ] Uses `PRINT_ANY` where possible, not separate JSON/text paths
- [ ] Rates/times use proper helpers for human-readable vs raw JSON values

### 4. Memory Safety
- [ ] All allocations checked for NULL
- [ ] No buffer overflows
- [ ] Memory freed on all error paths
- [ ] Use goto for centralized cleanup when appropriate

### 5. Netlink Handling
- [ ] Proper request structure initialization with designated initializers
- [ ] Correct use of `addattr_l()`, `addattr32()`, `addattr_nest()`
- [ ] Nested attributes properly closed with `addattr_nest_end()`
- [ ] Error return values checked

### 6. Kernel Compatibility
- [ ] uapi header updates in separate patch from functionality
- [ ] uapi changes reference upstream kernel commit
- [ ] Code handles older kernels gracefully
- [ ] Runtime feature detection where appropriate

### 7. Commit Message
- [ ] Subject line has correct prefix (`[PATCH iproute2]` or `[PATCH iproute2-next]`)
- [ ] Subject line ≤50 characters, lowercase after prefix
- [ ] Body wrapped at 72 characters
- [ ] Signed-off-by present

## Conditional Context Loading

Load additional context based on what the patch touches:

- **JSON output changes**: Load `json-output.md`
- **Argument parsing changes**: Load `argument-parsing.md`
- **New kernel feature support**: Load `kernel-compat.md`
- **New files or license questions**: Load `coding-style.md`
- **Patch submission issues**: Load `patch-submission.md`

## Common Issues to Watch For

1. **matches() in new code** - This is a frequent mistake. New code must use strcmp().

2. **Missing JSON object/array close** - Every open needs a close, or JSON output breaks.

3. **Error messages to stdout** - Corrupts JSON output. Must use stderr.

4. **Broken user-visible strings** - Strings users might grep for must not be split.

5. **Missing error checks** - Especially for malloc, netlink responses.

6. **uapi updates without kernel reference** - Must cite upstream commit.

## Output Format

When regressions are found, create a review in email format suitable for
sending to netdev@vger.kernel.org with specific line references and
explanations.

## Security Considerations

- Validate all user input
- Be careful with buffer sizes
- Check return values from system calls and library functions
- See SECURITY.md for vulnerability reporting procedures
