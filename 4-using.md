# Using

To use HookCase (once it's loaded into the kernel) you need to write
an "interpose library" (aka a "hook library") containing hook
functions for the methods you wish to hook.  The syntax is very
similar to that of `DYLD_INSERT_LIBRARIES` interpose libraries.
There's a annotated template in
[`HookLibraryTemplate`](HookLibraryTemplate/), and there are more
examples under [`Examples`](Examples/).

Once you have a hook library, you need to set environment variables to
load it into a new process and to determine how it behaves there.

Here are the environment variables that HookCase pays attention to:

* `HC_INSERT_LIBRARY` - Full path to hook library

* `HC_NOKIDS` - Don't effect child processes

By default, if HookCase sets hooks in a parent process, it will also
set them in all its child processes.  But if you set `HC_NOKIDS` (to
any value), HookCase only effects the parent process (not its
children).

You'll generally not want to set `HC_NOKIDS`.  These days many
applications use multiple processes, and it's particularly useful to
be able to log activity in child processes at the same time as you do
so in their parent.  But note that, even with `HC_NOKIDS` unset, this
really only works properly on OS X 10.11 (El Capitan) and up.  The
child processes of Apple applications (like Safari) are often XPC
services, which don't inherit their parent's environment.  HookCase
can keep track of "XPC children", but only on OS X 10.11 and up (not
on OS X 10.10 (Yosemite) or 10.9 (Mavericks)).
