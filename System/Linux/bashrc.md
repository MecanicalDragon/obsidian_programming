`.bashrc` - configuration file used to set up the shell environment. It is run for **non-login interactive shells**. This file is read and executed by the shell when you log in or open a terminal. It is specific to the `bash` shell.

`.profile` is more general and can be used by various types of shells (`sh` or `dash`). It is run for **login shells** (when you log in to the system (on the console or over SSH), typically for the first time in a session). It is suitable for general environment configuration across different shells.

`.bashrc` is often used to define **shortcuts**, **aliases**, and **command customizations** that you want available in every terminal session.

`.profile` is used for **global environment settings** (e.g., `$PATH`, other environment variables) that should apply across all login sessions.

By default, changes in these files are applied not immediately but when these files run (e.g. on the session start). But using the `source` [[shell#^source|command]] it is feasible to apply changes without restart.

To add a value permanently to the environment you have to modify either the `/etc/profile` or `~/.profile` files.

`/etc/profile` allows you to modify system-wide environment variables for logged-in users.
`~/.profile` allows you to change variables for a specific user.

To extend the *PATH* variable (or any other variable) you can use the following command: `export PATH="/example/bin:$PATH"`. `:` here is separator for different paths.