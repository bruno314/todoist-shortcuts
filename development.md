This is a file containing general notes about the development of
todoist-shortcuts.

# Contributing

Please ensure the following when opening a PR:

* Your change passes eslint (see `Running eslint` below).

* Your changes are written in a defensive style.  For example:

  - If it puts things in a state that should always be undone before the
action exits, then `try` / `finally` should be used.

  - It should check assumptions when possible.  For example, many parts of the
code find classes with the assumption that there is only one matching
element.  Rather than just using the first one, it ensures that there is
only one match.

* Your changes minimize use of mutable state between actions.

  - If possible, as much state should be queried from the DOM state.  This
makes it less likely that things will be out of sync.  In particular, the DOM
often gets refereshed, so storing references to nodes is often useless.

  - In cases where mutability is unavoidable, mark it with `MUTABLE` in a
comment.

For stuff to work on, take a look at [the issue
tracker](https://github.com/mgsloan/todoist-shortcuts/issues) or the list of
TODO items below.

# Running eslint

First, install eslint:

```
npm install eslint eslint-config-es5
```

Then, run `eslint.sh` to use it to do checking.

# TODO "soon"

* Is WHAT_CURSOR_APPLIES_TO = 'all' a good default?

* Have it fetch and cache which todoist versions are compatible.

* "bulk reschedule mode" - clears current selection, prompts for
rescheduling of item then moves cursor to the next.  Similar "bulk project
move" mode would also be nice.

* l should step out even if not focused on the collapser

* h should also move cursor down after opening

* Allow using ctrl+arrows while editing text of a task. Instead use
  shift+arrows to shift it around while editing.

* If user edits task and uses ctrl+arrow to indent / dedent, `cursorIndent`
doesn't update.  This can cause moveUp / moveDown to fail, among other
inconsistencies.

* Most functions have at least some documentation comments, but not all.
Should add these!

* Have `?` open up todoist-shortcuts README.md that is specific to the user's
  version.  Check if there is a new version?

* Add `update_url` field to `manifest.json`.

* Add a chrome extension icon.

# TODO "one day"

* Is it useful to be able to navigate to first / last task within a section?

* In agenda view, cursor should be able to focus empty days, for the purpose
of adding tasks.

* What is postpone?

* Enter day of month + move between months.

* Remember per project cursor locations.

* e for Archive and d for Done, # for Delete, are awfully close to
eachother.  Is archive vs delete useful?

* Use querySelectorAll to simplify code?  Would probably provide worse debug
diagnostics.

* Display of keybindings directly on elements suggests a library that mixes
querySelectorAll / mousetrap / the utilities here.  Design it also to be used
in normal applications?

* Consider using mnemonic navigation for project names, which would use the
first character of the project when possible.  Tricky to make this consistent
despite project folding.  Give priority based on lower indent levels?

* Handle more than (26 + 10 - 3 == 33) visible projects for navigation.

* Document usage with vimium.  Right now I have vimium disabled, but my most
recently accumulated set of key disables was `hjklxvtqaAuspr1234noO`

* The top-bar visibility hack requires knowing which task was last shift-
clicked.  However, it does not pay attention to user shift-clicks.  This means
if the last selection was done with the mouse, then some other stuff could get
randomly selected, which isn't very good.

* Youtube video?

* Blog post about the overall technique?

* Chrome webstore promotional tile images?

# Changes to todoist that would be very helpful

* Every button in menus should have a distinct, semantic class.  Right now for
some menus I am matching by english text, and this won't work for other
languages.  I could match it by index of the button, but this seems quite
unreliable if the order gets changed in the future.

* Other than things like today / tomorrow / next week / next month, there is
no good way to reschedule a task to a particular day.  I think it would
generally be a good improvement if the rescheduling calendar always had a
box to input textual descriptions of the date.  This would allow things that
you can't currently easily do, like bulk scheduling tasks with repeating
dates.

# Todoist issues

It would also be nice to get these fixed, but not really necessary.  Other
than the undo issue, I have solid workarounds.

* Undo will not work for multiple completions or deletions

* The behavior for shift-click is strange.  Between the last click location
and the new click location, selection statuses get toggled!?  See
`setSelections` and `shiftClickTask` functions in the code.

* In agenda mode, ids get duplicated

* In agenda mode, if you shift click a task that is in two spots, shift click
  should probably select both.

  - If you move the tasks so they are in the same day, they do not recombine!

* Sometimes when shift-clicking tasks, the top bar disappears.  I am currently
  doing the following workaround:

  - When it's detected that it has disappeared, double shift-click the last
    shift-clicked task.

  - Disable opacity animation to make this less visible.

# Automated upload of extensions

This is probably only useful to me for uploading the extension, but I figured
I'd document it anyway incase someone wondered what's up with the
`Gruntfile.js`.  It is used to automate extension upload.

Install grunt and https://github.com/c301/grunt-webstore-upload via

```
npm install grunt grunt-webstore-upload
```

Then, populate `etc/secrets.json` with a json object with `chrome_client_id` and
`chrome_client_secret` fields. Finally, you can run `./make-and-upload.sh` to
make the zip and upload it!


Firefox is not yet automated, for now I just visit
https://addons.mozilla.org/en-US/developers/addon/todoist-shortcuts/edit
