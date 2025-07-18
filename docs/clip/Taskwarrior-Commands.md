---
title: "Taskwarrior - What's next? - Taskwarrior"
source: "https://taskwarrior.org/docs/commands/"
date: 2025-06-15
description:
dg-publish: true
---
# Taskwarrior Commands

## Commands

Here are the commands alphabetically. Version-specific features are labelled with the version in which they were first available.

- [`add` - Add a new task](https://taskwarrior.org/docs/commands/add/)
- `annotate` - Add an annotation to a task
- [`append` - Append words to a task description](https://taskwarrior.org/docs/commands/append/)
- [`calc` 2.4.0 - Expression calculator](https://taskwarrior.org/docs/commands/calc/)
- `config` - Modify configuration settings
- `context` - Manage contexts
- [`count` - Count the tasks matching a filter](https://taskwarrior.org/docs/commands/count/)
- `delete` - Mark a task as deleted
- `denotate` - Remove an annotation from a task
- [`done` - Complete a task](https://taskwarrior.org/docs/commands/done/)
- `duplicate` - Clone an existing task
- `edit` - Launch your text editor to modify a task
- `execute` - Execute an external command
- [`export` - Export tasks in JSON format](https://taskwarrior.org/docs/commands/export/)
- `help` - Show high-level help, a cheat-sheet
- `import` - Import tasks in JSON form
- [`log` - Record an already-completed task](https://taskwarrior.org/docs/commands/log/)
- `logo` - Show the Taskwarrior logo
- [`modify` - Modify one or more tasks](https://taskwarrior.org/docs/commands/modify/)
- [`prepend` - Prepend words to a task description](https://taskwarrior.org/docs/commands/prepend/)
- `purge` 2.6.0 - Completely removes tasks, rather than change status to `deleted`
- `start` - Start working on a task, make active
- `stop` - Stop working on a task, no longer active
- [`synchronize` - Syncs tasks with other instances](https://taskwarrior.org/docs/commands/synchronize/)
- `undo` - Revert last change
- `version` - Version details and copyright

## Customizable Reports

[Customizable reports](https://taskwarrior.org/docs/report/) can be modified to suit your needs. They all work in same manner, and mostly differ by the [columns](https://taskwarrior.org/docs/commands/columns/) shown, the [filter](https://taskwarrior.org/docs/filter/) applied, and the sorting performed.

Because all the customizable reports work in the same way, [the `list` report](https://taskwarrior.org/docs/commands/list/) is the only report discussed.

- `active` - Started tasks
- `all` - Pending, completed and deleted tasks
- `blocked` - Tasks that are blocked by other tasks
- `blocking` - Tasks that block other tasks
- `completed` - Tasks that have been completed
- [`list` - Pending tasks](https://taskwarrior.org/docs/commands/list/)
- `long` - Pending tasks, long form
- `ls` - Pending tasks, short form
- `minimal` - Pending tasks, minimal form
- `newest` - Most recent pending tasks
- `next` - Most urgent tasks
- `oldest` - Oldest pending tasks
- `overdue` - Overdue tasks
- `ready` - Pending, unblocked, scheduled tasks
- `recurring` - Pending recurring tasks
- `unblocked` - Tasks that are not blocked
- `waiting` - Hidden, waiting tasks

## Fixed Reports

The fixed reports have minimal customization, typically only color.

- [`burndown.daily` - Burndown chart, by day](https://taskwarrior.org/docs/commands/burndown/)
- [`burndown.monthly` - Burndown chart, by month](https://taskwarrior.org/docs/commands/burndown/)
- [`burndown.weekly` - Burndown chart, by week](https://taskwarrior.org/docs/commands/burndown/)
- `calendar` - Calendar and holidays
- `colors` - Demonstrates all supported colors
- [`columns` - List of report columns and supported formats](https://taskwarrior.org/docs/commands/columns/)
- `commands` 2.5.0 - List of commands, with their behaviors
- `diagnostics` - Show diagnostics, for troubleshooting
- `ghistory.annual` - History graph, by year
- `ghistory.monthly` - History graph, by month
- `ghistory.weekly` 2.6.0 - History graph, by week
- `ghistory.daily` 2.6.0 - History graph, by day
- `history.annual` - History report, by year
- `history.monthly` 2.6.0 - History report, by month
- `history.weekly` 2.6.0 - History report, by week
- `history.daily` - History report, by day
- `ids` - Filtered list of task IDs
- [`information` - All attributes shown](https://taskwarrior.org/docs/commands/info/)
- `projects` - Filtered list of projects, with task counts
- `reports` - List of available reports
- `show` - Filtered list of configuration settings
- `stats` - Filtered statistics
- `summary` - Filtered project summary
- `tags` - Filtered list of tags
- `timesheet` - Weekly timesheet report
- `udas` - Details of all defined UDAs
- `uuids` - Filtered list of UUIDs

## Helper Commands

Helper commands all begin with an underscore, and they exist to provide support for third party add-ons, such as shell completion scripts.

Helper commands generate no extraneous output, and are therefore easy to parse. Helper commands are not intended for regular use, but there is no reason for that to stop you.

- `_aliases` - List of active aliases
- `_columns` - List of supported columns
- `_commands` - List of supported commands
- `_config` - List of configuration setting names
- `_context` - List of defined context names
- [`_get` - DOM accessor](https://taskwarrior.org/docs/commands/_get/)
- `_ids` - Filtered list of task IDs
- `_projects` - Filtered list of project names
- `_show` - List of `name=value` configuration settings
- `_tags` - Filtered list of tags in use
- `_udas` - List of configured UDA names
- [`_unique` - 2.5.0 List of unique values for the specified attribute](https://taskwarrior.org/docs/commands/_unique/)
- `_urgency` - Filtered list of task urgencies
- `_uuids` - Filtered list of pending UUIDs
- `_version` - Task version (and optional git commit)
- `_zshattributes` - Zsh formatted task attribute list
- `_zshcommands` - Zsh formatted command list
- `_zshids` - Zsh formatted ID list
- `_zshuuids` - Zsh formatted UUID list

## Usage Examples

Here are some Taskwarrior command line examples that cover a variety of topics. While this is not a reference for the features and command line, perhaps you will learn something new…

Some of the examples are deliberately chosen because there is more than one solution, in which case all are presented, for comparison.

## 30 Second Tutorial

```shell
$ task add Read Taskwarrior documents later
$ task add priority:H Pay bills
$ task next
$ task 2 done
$ task
$ task 1 delete
```

## Creating Tasks

Creating tasks is straightforward, but here are some tips:

Create a task with due date.

```shell
$ task add Pay the rent due:eom
```

Create a task, then add a due date later.

```shell
$ task add Pay the rent Created task 12
$ task 12 modify due:eom
```

Remove a due date from a task.

```shell
$ task 12 modify due:
```

Create a task with a multi-line description.

```shell
$ task add "Five syllables here
> Seven more syllables there
> Are you happy now?"
```

## Filters

A filter is how you restrict the tasks to just those that you want to see or modify.

Show tasks I added in the last 4 days.

```shell
$ task entry.after:today-4days list
```

Show tasks I added yesterday.

```shell
$ task entry:yesterday list
```

Show tasks I added in the last hour.

```shell
$ task entry.after:now-1hour list
```

Show tasks I completed between a date range.

```ruby
$ task end.after:2015-05-0 and end.before:2015-05-31 completed
```

Show tasks I completed in the last week.

```ruby
$ task end.after:today-1wk completed
```

Show tasks in `This` project or `That` project.

```php
$ task project:This or project:That list
```

More complex algebraic filters.

```php
$ task project:This and \( priority:H or priority:M \) list
```

Search for `pattern` in description and annotations:

```shell
$ task /pattern/ list
$ task rc.seach.case.sensitive:yes /pattern/ list
$ task rc.seach.case.sensitive:no  /pattern/ list
```

Search for tasks with no project.

```shell
$ task project: list
```

## Reports

Reports are simply a collection of configuration settings that specify display attributes, sorting, filter and a name.

Temporarily changing the sorting of a report.

```ruby
$ task rc.report.next.sort=due-,urgency- next
```

## Projects

A single project may be assigned to a task, and that project may be multiple words.

Assign a long project name.

```shell
$ task add Rake the leaves project:'Home & Garden'
```

Moving all tasks to a new project.

```shell
$ task project:OLDNAME modify project:NEWNAME
```

Moving all pending tasks to a new project.

```ruby
$ task project:OLDNAME and status:pending modify project:NEWNAME
```

Using a project hierarchy.

```shell
$ task add project:Home.Kitchen Clean the floor
$ task add project:Home.Kitchen Replace broken light bulb
$ task add project:Home.Garden Plant the bulbs
$ task project:Home.Kitchen count
2
$ task project:Home.Garden count
1
$ task project:Home count
3
```

What projects are currently used?

```shell
$ task projects
$ task _projects
```

What are all the projects I have ever used?

```shell
$ task rc.list.all.projects=1 projects
$ task rc.list.all.projects=1 _projects
$ task _unique project
```

Tags are simply one-word alphanumeric labels, and a task may have any number of them.

List tasks that have a specific tag.

```shell
$ task +home list
```

List tasks that do not have a specific tag.

```shell
$ task -home list
```

List tasks that have any tags.

```shell
$ task tags.any: list
$ task +TAGGED list
```

List tasks that have no tags.

```shell
$ task tags.none: list
```

List tasks that have two specific tags.

```cpp
$ task +this +that list
$ task +this and +that list
```

List tasks that have either of two specific tags.

```cpp
$ task +this or +that list
```

List tasks that have just one of two specific tags.

```php
$ task +this xor +that list
```

What tags am I currently using?

```shell
$ task tags
```

What are all the tags I have ever used?

```shell
$ task rc.list.all.tags=1 tags
$ task _tags
```

## Special Tags

A special tag is one with a specific name, that can influence behavior.

Modify a task to boost its urgency, and probably cause it to show up on the `next` report.

```ruby
$ task 1 modify +next
```

## Virtual Tags

A virtual tag is a tag that doesn’t actually exist, but the tag filter syntax is used to simulate the tag, providing a very convenient shortcut. After all, composing a filter to match the tasks due today is not straightforward.

List tasks due today.

```shell
$ task due.after:yesterday and due.before:tomorrow list
$ task +DUETODAY list
```

List tasks that are due, but not today.

```shell
$ task +DUE -DUETODAY list
```

List tasks that are due this week.

```shell
$ task +WEEK list
```

List tasks that are overdue.

```shell
$ task +OVERDUE list
```

What virtual tags are present for this task?

```shell
$ task 1 info
```

## Recurring Tasks

Recurring tasks are tasks that you need to do again and again.

I want to make a task that is due at 9:00am every Monday, starting this coming Monday.

```shell
$ task add Do the thing due:2015-06-08T09:00 recur:weekly
```

I want a reminder to pay the rent every month, but only for a year.

```shell
$ task add Pay rent due:28th recur:monthly until:now+1yr
```

## Priority

Priority is now a [User Defined Attribute](https://taskwarrior.org/docs/udas/) since version 2.4.3, and as such can be configured.

Make priority `L` sort lower than no priority.

```shell
$ task config uda.priority.values H,M,,L
```

Note that the above configuration only changes the sorting order. The `L` priority will still be considered higher than no priority in urgency calculations. To change the urgency calculation, you also need to adjust the urgency coefficients of the values.

I need more priority values for my workflow.

```shell
$ task config uda.priority.values OMG,DoIt,Meh,Phfh,Nope,
```

How do I remove the priority from a task?

```shell
$ task 1 modify priority:
```

## Color

I’m using a color theme, but I don’t see any colors. This is usually because your tasks do not contain due dates, priorities etc. Prove color is working with these commands.

```shell
$ task color
$ task color legend
```

When task output goes to a file or pipe, all color is lost. Force color with:

```coffeescript
$ task rc._forcecolor:on rc.defaultwidth:120 rc.detection:off ...
```

## DOM

The Taskwarrior DOM is an addressing mechanism to provide access to all stored data. It can be used in your scripts, or when manipulating tasks at the command line.

Get just the description for task 12.

```shell
$ task _get 12.description
Rake the leaves
```

Show the creation timestamp, and last modification date for task 12.

```shell
$ task _get 12.entry 12.modified
2000-04-04T01:02:31 2014-08-24T13:31:43
```

Get the dimensions of my terminal window.

```shell
$ task _get tw.width tw.height
```

Add a task, and set the wait date to 4 days before the due date.

```shell
$ task add Pay the rent due:eom wait:due-4days
```

Add a task, and use the same due date as task 12.

```shell
$ task add Buy wine for the party due:12.due
```

Get the week number on which task 12 due.

```shell
$ task _get 12.due.week
```

Get the columns used in the next report.

```ruby
$ task _get rc.report.next.columns
$ task show report.next.columns
```

## Configuration

Although you can interactively edit your `.taskrc` file using any text editor, there is also a command for doing this. Furthermore, you can temporarily override these settings on the command line.

Set the `default.command` to a different report.

```shell
$ task config default.command long
```

Restore the `default.command` to its original setting.

```shell
$ task config default.command
```

Set the `default.command` to a blank value.

```shell
$ task config default.command ''
```

Temporarily override default.command.

```shell
$ task rc.default.command:long
```

Show sort order of all reports.

```perl
$ task show | grep report | grep sort
```

## Miscellaneous

Scriptwriters often need access to assorted data, and it can all be obtained, but sometimes through odd mechanisms…

What is the most recent task ID?

```shell
$ task newest rc.verbose=nothing limit:1 | cut -f1 -d' '
$ task rc.verbose=nothing rc.report.foo.columns:id rc.report.foo.sort:id- foo limit:1
```

What is the minimum necessary data for a task?

```shell
$ echo '{"description":"A new task"}' | task import -
$ task add A new task
```

## Troubleshooting

Taskwarrior has some built-in functionality to help you get past obstacles.

Show the runtime diagnostics, to see if anything is wrong.

```shell
$ task diagnostics
```

Determine which version of GnuTLS is used by Taskwarrior.

```shell
$ task diagnostics | grep gnutls
```

Just let me brute-force change a task. Using Vim.

```shell
$ task 12 edit
$ EDITOR=vim task 12 edit
```

I can’t list my completed tasks, because the `list` report has a filter that shows only pending tasks.

```shell
$ task rc.report.list.filter: list
$ task all
```

Do I have the latest released version?

```shell
$ curl -L https://gothenburgbitfactory.org/task/latest
3.4.1
$ task --version 
3.4.1
```