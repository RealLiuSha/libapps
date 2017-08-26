
# Getting the source

The official copy of this repository is located on chromium.googlesource.com.
Use `git clone https://chromium.googlesource.com/apps/libapps` to create a
local copy.

# Coding Style

We follow the [Chromium style guide] (which in turn follows the
[Google style guide]).  You might find code in the tree that doesn't follow
those, but it's most likely due to lack of tools automatically checking and
enforcing rather than being done on purpose.

[Chromium style guide]: https://chromium.googlesource.com/chromium/src/+/master/styleguide/web/web.md#JavaScript
[Google style guide]: https://google.github.io/styleguide/jsguide.html

* Do not one-line if statements.
* When wrapping function arguments, wrap them all, or keep them aligned.

## Automatic Formatting

If you want to see suggestions for how to format your code, you can use the
`clang-format` tool.  Note that clang-format can sometimes reformat code that
is acceptable according to the style guide, and it cannot catch everything!
Make sure to use your best judgment when going through its proposed changes.

```
# Write the formatted output to stdout.
$ clang-format -style=file foo.js

# Update the file in-place.
$ clang-format -i -style=file foo.js
```

## Linting

You can use [eslint](https://eslint.org/) to quickly check code.

## Console Logging

The `console.log` functions have a variety of function signatures.  We like to
stick to a few forms though.

* A single string when we expect everything to be strings:

      console.log('The field "' + name + '" is invalid: ' + val);

* A string followed by objects when dealing with more than strings, and we want
  to be able to easily explore the object in the debugging console:

      console.log('The field "' + name + '" is invalid:', val);

* Or you can use ES6 template literals/strings:

      console.log(`The field "${name}" is invalid: ${val}`);
      console.log(`The field "${name}" is invalid:`, val);

# Submitting patches

This repository only accepts commits that are submitted through "Gerrit", the
code-review software.  In order to submit a patch through Gerrit, you'll need
to do a one-time setup to get things ready.

1. Create an account on https://chromium-review.googlesource.com/.  (You can use
   OAuth for this, no need for yet-another-password.)

2. From the root of your libapps/ repo, run the command:

        $ curl -Lo `git rev-parse --git-dir`/hooks/commit-msg https://gerrit-review.googlesource.com/tools/hooks/commit-msg ; chmod +x `git rev-parse --git-dir`/hooks/commit-msg

   This will copy a commit-msg hook necessary for Gerrit.  The hook annotates
   your commit messages with Change-Id's.  It's important to leave these intact,
   as it's how commits are mapped to code reviews.

Now you're free to start working.  Create a branch to hold your changes...

    $ git checkout -b hterm_stuff

Then start hacking and commit your changes.  When you're ready to submit, push
with...

    $ git push origin HEAD:refs/for/master

This will push the current branch to Gerrit for review.  Once the change has
passed review it will be cherry-picked onto the master branch of the official
repository.

The output of this command should include a url to Gerrit web page for the
review.  Add one or more reviewers using the "Add Reviewer" button on that web
page.

If the official repository changes, you can fetch the new commits using...

    # This command only affects your local repository files, you can run it
    # regardless of which branch you're currently on.
    $ git fetch

And then re-base any branches with work-in-progress.

    $ git checkout hterm_stuff
    $ git rebase origin/master

Sometimes this rebase will fail due to merge conflicts which will have to be
resolved by hand.
