# JustOneTask

A web app that shows the user's next most urgent task.

## MVP - Single user

For each task, the user should be able to define how frequently they'd like to
do it, and how frequently it actually becomes urgent.

For example, maybe they want to change their bed sheets every week, but they
don't think it's urgent until it's been two weeks.

The app should calculate the urgency of the task by comparing how long it's been
since last completed to the period of time between how often they'd like to do
it, and how often it's urgent.

So, for the bed sheets, if it hasn't yet been a week, it's urgency should be
null, since it's not even a candidate for showing up as the most urgent task.
At one week, the urgency should be 0, but by the time it's been two weeks, the
urgency should be 1. At 9 days the urgency shuld be between 0 and 1 -- it
should be 2/7. In other words, each day should add 1/7 to the urgency value, or
1 divided by the number of days between "would like to do" and "now it's
urgent". After 2 weeks, the app should keep adding 1/7 for every day.

If there are multiple tasks with the same urgency, there should be a visual
indicator of this, and the user should be able to choose which one they want
displayed.

That is the basic functionality.

Note that if the user is always behind on tasks (like me), tasks will rarely if
ever even show up as the most urgent task on the "would like to do" date, when
urgency is set to 0. This is indeed the intended behavior.

### Immediately urgent tasks

The user should be able to define tasks that must be done on a certain date or
at a specific frequency with no wiggle room. These tasks should not have an
urgency value -- there should be another way to flag them.

These tasks should always be shown as the next task on the day they are due,
until they are done. If there are multiple tasks due today, or overdue tasks,
the app should have the same behavior as when there are multiple tasks with the
same urgency.

### Single-occurrence tasks

Need to think about this.

### Users

Simple user-task relationship. Users should only be able to see their own tasks.

### Screens

#### Home

The main screen will only show your next most urgent task, as well as a visual
indicator if there are other tasks with the same value for urgency.

There should be something the user can click on that also shows the next 3
tasks, but they should be hidden by default. This is so the user can easily
evaluate if this task is configured incorrectly and should not be the most
urgent task.

There should be shortcuts for adjusting the top task's urgency, in case the
user decides it is incorrect. There should be a way to get to the the task edit
screen for this task from the main screen.

The main screen should optionally show a number indicating how many tasks have
an urgency greater than 1.

#### Task list

The task list screen will show all the tasks sorted by urgency descending, with
nulls last. The user should be able to manually adjust the urgency value for a
task by clicking up or down arrows which will increment or decrement the
urgency by, for example, 1/7 on the bed sheets task.

There should be another section on the task list screen that shows tasks
flagged as immediately urgent.

The task list should be searchable and filterable. Tasks should be able to have
tags which you can filter by as well.

Clicking on a task should take you to the task edit screen.

#### Task edit

The user should be able to edit any task property, including last completed
date. When a task is edited the urgency should be recalculated.

#### Options

The user should be able to configure what elements are visible on the main
screen besides the next most urgent task. For MVP this is just the number of
tasks with an urgency greater than 1.

The user should be able to configure their timezone, and what time of day the
urgency values should be recalculated. The default should be midnight in the
timezone they were in when they signed up.

The user should be able to change their password, and add or remove third-party
login methods.


## Development

The app should actually be two apps. One a backend Flask API using PeeWee to
talk to the database. One a frontend Svelte app that uses the API. The database
should be PostgreSQL.

Users should be able to login with a third-party, or email + password.

### Platform

Fly.io for both apps and the database


## Future

### V2 - Mobile

The user should be able to download apps for iOS and Android.

### V3 - Households

Users should be able to group themselves into households that share tasks.
Tasks would be owned by households instead of users. Tasks should be able to be
assigned to a specific user, but it should not be necessary.

If the most urgent task is assigned to a specific user, it should only show up
on that user's main screen as the next task, but should show up on all
household users task list screen. The next most urgent task should show up on
the main screen for other household users.

If the most urgent task is not assigned to a user, then it should show up as
the most urgent task for all users in the household. As such, tasks should be
able to be marked as "in progress" by a user, assigning it to that user for
this occurrence only. This would prevent more than one user from working on the
same task at the same time.

### V4 - Gamification

Tasks should be given an "effort" property. Users should get points for
completing tasks, based on the effort value. There should be a household
leaderboard screen. There should be some indication on the main screen of the
user's position on the leaderboard and how many points away they are from the
user ahead of them.

### V5 - Constrained tasks

The user should be able configure tasks to only show up as the top task on
certain days of the week, days of the month, or months of the year. In this
case, if the most urgent task should only be shown on Sunday, and it is not
Sunday, the next most urgent task should be displayed on the main screen.

Constrained tasks should still increment their urgency every day.
