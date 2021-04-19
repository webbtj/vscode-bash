# vscode-bash

Example setting up custom bash commands explicitly for VS Code Terminal on a
project specific basis.

The is just an example setup for creating some custom terminal commands (I'm
using Bash, you don't have to) without cluttering up your personal user's
profile or your whole system with a bunch of project specific tools.

Why? Say you have a project with multiple docker containers. In those containers
are tools you need to interact with from your Terminal. Sometimes those tools
require you to pass alot of the same arguments every time you use them. For
example, in this sample project we want to be able to run PHP CodeSniffer.
CodeSniffer is installed and configured within the container, so we need to run
it from the container. We could type out
`docker exec -ti ss2-web phpcs --standard=Drupal /var/www/html/web/modules/custom`
when we want to run `phpcs`. Or with this custom setup we can run `ss2 cs`.
That's certainly a lot cleaner and easier to remember.

What's "SS2"? Doesn't matter, it's project specific, which is the whole point.
`ss2` is not a command in my user's profile or in `PATH` or anything like that.
It only exists when VS Code opens a Terminal for _this_ project.

## How Does It Work

Step 1 is the `settings.json` file. In there we've added a value for
`terminal.integrated.shellArgs.osx` telling VS Code to run
`./.vscode/bash/.bashrc-vscode"` file when starting a Terminal. And because this
goes in the project's `.vscode` directory, it only applies for _this_ project.

Step 2 is the bashrc file itself, `.vscode/bash/.bashrc-vscode`. In this file
we've simply added our `.vscode/bash` path to our `PATH` exposing all of our
custom bash scripts.

Step 3, we have a bunch of bash scripts, which are all executable(`ss2cbf`,
`ss2composer`, `ss2cs`, `ss2drush`, `ss2shell`). Check out each of these
scripts, they're pretty basic and are just calling our longer, more complex
commands. Note that scripts like `ss2composer` and `ss2drush` pass along any
extra arguments using `$@`. This way we can run `ss2composer install` and it
will run `composer install` within our container.

Step 4, finally we have an `ss2` script. This is just to make things a bit
cleaner and easier to work with without having to remember every command. All of
the aforementioned commands can also be run using the `ss2` command. For example
`ss2 composer install`. It's just a bit of value add and also gives us a little
bit of help if we don't pass a valid option. Running `ss2` without any extra
arguments will give us a simple help screen explaining the available commands.
