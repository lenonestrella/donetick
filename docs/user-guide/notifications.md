# Notifications

Donetick reminds you about your tasks and tells you when someone needs something from you. You decide what you hear about, when, and where it arrives.

This guide covers:

- [What Donetick can notify you about](#what-donetick-can-notify-you-about)
- [Turning on notifications on your phone](#turning-on-notifications-on-your-phone)
- [Choosing a notification profile](#choosing-a-notification-profile)
- [Fine-tuning individual notifications](#fine-tuning-individual-notifications)
- [Daily and weekly digests](#daily-and-weekly-digests)
- [Setting reminders on a task](#setting-reminders-on-a-task)
- [Getting notifications in Telegram or Pushover](#getting-notifications-in-telegram-or-pushover)
- [Sending a nudge](#sending-a-nudge)
- [Free and Plus plans](#free-and-plus-plans)
- [Troubleshooting](#troubleshooting)

## What Donetick can notify you about

| Notification | When you get it |
| --- | --- |
| **On Assignment** | A new or existing task is assigned to you by someone else |
| **Approval Needed** | Someone completed a task and it is waiting for your approval (admins and managers only) |
| **Redemption Request** | Someone asked to redeem a reward (admins and managers only) |
| **Pre-Due Reminder** | Before a task is due, at the times you set on that task |
| **Due Reminder** | When a task becomes due |
| **Overdue Follow-up** | After a task's due date has passed, at the times you set on that task |
| **Daily Digest** | Once a day: a summary of what is due today and what is overdue |
| **Weekly Digest** | Every Monday: a preview of the week ahead |
| **Nudge** | Someone in your circle sends you a gentle reminder about a task |

You only hear about tasks you are allowed to see. Tasks in a private project are never announced to people outside that project.

## Turning on notifications on your phone

1. Open the Donetick app and go to **Settings → Notifications**.
2. Under **Where you get notified**, switch on **Enable Notifications**.
3. When your phone asks for permission, allow notifications.

That's it. Your phone now gets task reminders, plus assignments, approvals and nudges sent to your account.

A few things to know:

- **This switch is per device.** Turn it on on each phone or tablet you want notified. You can register up to 5 devices.
- **Task reminders are scheduled on the phone itself.** They work even without an internet connection. When you add or change a task on another device, your phone picks up the change the next time you open the app.
- **On the web**, browser notifications aren't available. To get reminders without the mobile app, connect Telegram or Pushover (see below).

## Choosing a notification profile

A profile is a ready-made mix of notifications. Pick the one that fits how you like to work under **Settings → Notifications → Notification Preferences**.

| | Balanced | Planner | Just in Time | Persistent | Quiet |
| --- | :---: | :---: | :---: | :---: | :---: |
| On Assignment | ✓ | ✓ | ✓ | ✓ | |
| Approval Needed | ✓ | ✓ | ✓ | ✓ | |
| Redemption Request | ✓ | ✓ | ✓ | ✓ | |
| Pre-Due Reminder | ✓ | | | | |
| Due Reminder | ✓ | ✓ | ✓ | ✓ | |
| Overdue Follow-up | ✓ | | | ✓ | |
| Daily Digest | ✓ | ✓ | | ✓ | |
| Weekly Digest | | ✓ | | | |

- **Balanced** — a bit of everything. The default for Plus members.
- **Planner** — fewer interruptions during the day, with daily and weekly summaries to plan ahead.
- **Just in Time** — only what needs you right now. The default on the free plan.
- **Persistent** — keeps reminding you until overdue tasks are done.
- **Quiet** — no notifications at all.
- **Custom** — you choose every notification yourself (see the next section).

Your profile is personal. It applies across every circle you belong to, and it doesn't change anyone else's notifications.

Below the picker, Donetick sums up your choice in a sentence, such as "You'll hear about new assignments, tasks becoming due, with summaries at 8:00 AM."

## Fine-tuning individual notifications

Want a profile but with one or two changes? Open **Fine-tune what you hear about** and switch individual notifications on or off.

- Your changes are kept on top of the profile you picked, and Donetick shows how many you've made.
- To undo them all, tap **Reset to** *profile name*.
- Changes to your profile apply right away, including reminders that are already scheduled.

## Daily and weekly digests

Digests are short summaries sent to your phone instead of one message per task.

- **Daily Digest** lists what's due today and what's overdue, for example "3 tasks today — Due today: Dishes, Laundry · Overdue: Groceries".
- **Weekly Digest** arrives on Monday and previews the week, for example "4 tasks this week, starting with Groceries (Mon)".

Choose when they arrive under **Digest Delivery Time**. The time is in the timezone of your Donetick profile. If there is nothing to report, no digest is sent.

Digests are sent to phones with the Donetick app and notifications turned on. They are not sent to Telegram or Pushover.

## Setting reminders on a task

Each task can have its own reminder schedule. When creating or editing a task:

1. Find the **Notifications** section and switch on **Notify for this task**.
2. Under **Notification Schedule**, add up to 5 reminders. Each one is:
   - **Before** — a reminder some minutes, hours or days before the task is due (for example, 1 day before)
   - **Due** — an alert at the moment the task is due (only one per task)
   - **After** — a follow-up some time after the due date, if the task still isn't done (for example, 3 days after)
3. Save the task.

Tap **Remember for Future Tasks** to use the same schedule as the starting point for new tasks. This is remembered on the device or browser where you tapped it.

**Who gets the reminders?** The person the task is assigned to. If a task is assigned to **Anyone**, everyone in the circle who can see it is reminded.

**Reminding a Telegram group.** Under **Who to Notify**, choose **Specific Group** and enter a **Telegram Group ID**. Donetick then also posts the reminders to that group. This isn't available for tasks in a private project.

Reminders can't be set on trigger-based or always-available tasks, since they have no due date.

Your profile still applies to task reminders. For example, if you turn off **Pre-Due Reminder**, the "before" reminders on your tasks are skipped for you.

## Getting notifications in Telegram or Pushover

You can also receive notifications outside the Donetick app. Go to **Settings → Notification integrations** and switch on **Custom Notification**, then choose a **Service**.

**Telegram**

1. Start a chat with the Donetick bot first, using the link on the settings screen. The bot can't message you until you do.
2. Enter your **Chat ID**. If you don't know it, start a chat with *userinfobot* in Telegram and it will tell you.
3. Save.

**Pushover**

1. Enter your Pushover **User key**.
2. Save.

**Webhooks** sends your task notifications through your circle's webhook connection instead. A circle admin sets this up under **Settings → Integrations → Circle webhooks**.

You can connect one external service at a time. Your notification profile applies to these channels too.

## Sending a nudge

Is someone forgetting a task? Send them a nudge.

1. Open the task, or open its menu, and choose **Send nudge**.
2. Optionally add a **Custom Message**.
3. Switch on **Notify All Assignees** to reach everyone who can see the task. Otherwise only the assigned person gets it.
4. Send.

The nudge arrives as a "Gentle Nudge" notification on their phone. You can't nudge yourself, and the other person needs the mobile app with notifications turned on.

## Free and Plus plans

| Feature | Free | Plus |
| --- | :---: | :---: |
| Due Reminder | ✓ | ✓ |
| On Assignment, Approval Needed, Redemption Request | ✓ | ✓ |
| Nudges | ✓ | ✓ |
| Telegram and Pushover | ✓ | ✓ |
| Pre-Due Reminder | | ✓ |
| Overdue Follow-up | | ✓ |
| Daily and Weekly Digest | | ✓ |
| Profiles | Just in Time, Quiet | All |

If your Plus subscription ends, the Plus-only notifications stop automatically. Everything else keeps working, and your settings are kept.

## Troubleshooting

**I'm not getting any notifications on my phone.**

- Check that **Enable Notifications** is on in **Settings → Notifications** on that phone.
- Check that notifications for Donetick are allowed in your phone's own settings.
- Make sure your profile isn't **Quiet**, and the notification you expect is switched on.
- Make sure the task has **Notify for this task** switched on and at least one reminder.

**A reminder arrived a few minutes late.**
Reminders sent to Telegram, Pushover and push notifications are checked every few minutes, so they can arrive up to about 3 minutes after the set time.

**My phone reminded me about an old due date.**
Phone reminders refresh when you open the app. Open Donetick once after changing tasks on another device.

**I don't get digests.**
Digests need Plus, the Donetick mobile app with notifications turned on, and **Daily Digest** or **Weekly Digest** switched on. Also check that **Digest Delivery Time** and your profile timezone are what you expect.

**Telegram stopped working.**
Make sure you've started a chat with the Donetick bot and that your Chat ID is correct. If you blocked the bot, unblock it.

**I can't register another device.**
You can have up to 5 devices. Remove an old one under **Settings → Notifications** and try again.

**I use a self-hosted Donetick server.**
Push notifications, nudges and digests depend on how your server is set up. Ask whoever runs it. Task reminders scheduled on your phone and Telegram or Pushover work either way, if the server has them configured.
