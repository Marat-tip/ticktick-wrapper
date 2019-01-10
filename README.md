# TickTick Wrapper for Node

This package is meant to be a wrapper that allows you to automate your actions on TickTick.

It calls the **undocumented API**, used by the web app, that can change at any time, use with caution.

## Getting started

Install the package by executing:

```
npm install -s ticktick-wrapper
```

### Example

```
require('dotenv').config();
const ticktick = require('ticktick-wrapper');

const main = async () => {
  await ticktick.login({
    email: {
      username: process.env.EMAIL_LOGIN_USERNAME,
      password: process.env.EMAIL_LOGIN_PASSWORD,
    },
  });

  const title = 'Automated task';
  const description = 'This is a task added programatically via ticktick-wrapper';
  await ticktick.Inbox.addSimpleTask(title, description);
};

main();
```

## API

### `TickTick`

#### `TickTick.login(options)`

Authenticate with TickTick and store the session info for future calls. This method must be called *before* any other.

- `options`: Object - Contains the credenials to authenticate.
  - `options.email`: Object - Must be populated if the preferred login method is by e-mail
    - `options.email.username`: String - Your account username, the e-mail
    - `options.email.password`: String - Your password

Logins via Google, Facebook or Twitter are not implemented yet.

#### `TickTick.user`

Object that contains relevant information about the authenticated user

- `user.id`: String - Id of the user
- `user.username`: String - Username of the user
- `user.isPro`: Bool - Whether the user has a pro account

#### `TickTick.createReminder(quantity, unit)`

Time before start date to have a reminder of the task.

- `quantity`: Number - Quantity of \<unit\> to set the reminder before start date
- `unit`: String - Unit of quantity. Allowed values are:
  - `'seconds', 'minutes', 'hours', 'days'`
  - `'s', 'm', 'h', 'd'`

#### `TickTick.Inbox`

[`List`](#list) object that references the inbox of your account.

### `List`

#### `List.addSimpleTask(title, content, date, isAllDay, reminder)`

Creates a task that has only a title and a description (content) and instantly adds it in your account, inside the list referenced by the object in which you are calling the method. It is also possible to add a date and reminders for it.

- `title`: String - Title of the task
- `content`: String - Description of the task
- `date`: Date - Date to set in the task
- `isAllDay`: Boolean - Whether the task has a specific time in the day
- `reminders`: [[Reminder](#reminder)] - Reminders to set in the task, create them using [`TickTick.createReminder`](#ticktickcreatereminderquantityunit)