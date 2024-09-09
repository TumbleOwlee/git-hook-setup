# Easy Git Hook Setup

By default Git hooks are located in `.git/hooks` and thus won't be pushed to the repository. Thus, you can't share it this way with your team members. This repository contains the minimal scripts necessary to add and share hooks using the repository itself. Everything is written in `Bash` to make it compatible with every Unix based project.

## Manual

Since we have to move the hooks out of `.git/hooks` to be able to push them to the repository and share them with all developers, we move them into a new `.git-hooks` directory. First, just copy the `git-hooks` repository and move it into your repository as `.git-hooks`. Don't forget to set the executable flags for the scripts contained in the directory.

```bash
# Copy the directory into your repository
cp -r git-hooks /path/to/repo/.git-hooks

# Set the executable flag for the scripts
chmod +x /path/to/repo/.git-hooks/*
```

Now, set up the hook enviroment utilizing the `.git-hooks/hooks` script. Just execute the following command.

```bash
# Install all symbolic links for the hooks-wrapper
.git-hooks/hooks install

# You can revert it, too
.git-hooks/hooks uninstall
```

This will place a symbolic link for each hook in `.git/hooks` that points to `.git-hooks/hooks-wrapper`. If you already created a hook in `.git/hooks` it will **not** be overwridden. Instead your hook is first moved to `.git/hooks/<name>.local` before the symbolic link is created.

After setup the `.git-hooks/hooks-wrapper` script is the entry point for all hooks. Now you can create all supported git hooks in `.git-hooks` (e.g. `.git-hooks/pre-commit`). Please keep in mind that the wrapper script will **always** execute the moved hook in `.git/hooks` first, and then the associated script in `.git-hooks`. Thus, if you want to use a hook that you don't want to share, just create the hook in `.git/hooks` and append the extension `.local`. Please see `git-hooks/hooks` for a list of configured/supported hooks. If any hook is missing, just add it to the list and run `.git-hooks/hooks install` again.

Finally, simply add the `.git-hooks` directory and push it. Afterwards, your team members simply have to pull the repository and run `.git-hooks/hooks install` once.
