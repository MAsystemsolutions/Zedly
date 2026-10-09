# SlotSync

**Share your real availability. Let people book a time. Invites go out automatically.**

A scheduling app like Calendly, built entirely on Google Workspace. Google Apps Script runs the app, Google Sheets stores settings and bookings, and Google Calendar is the real calendar: free times come from your actual schedule, and every booking lands on it with an invite sent to the guest.

### 👉 [Try the live demo](https://masystemsolutions.github.io/SlotSync/)

Pick a demo role (**Mia, owner** or **Jordan, team member**), or click **Book a meeting as a guest** to see what your clients see. No sign-up needed. Everything is sample data, emails are previewed instead of sent, and the demo resets every night.

<!-- Add screenshots here, e.g.
![Booking page](screenshots/booking.png)
![Calendar](screenshots/calendar.png)
![Meeting types](screenshots/types.png)
-->

---

## Features

| | |
|---|---|
| **Booking pages** | Each person gets a link like `?u=mia`, with meeting types such as *30 min with Mia*, a 15-minute intro call, or a 60-minute consultation. Guests pick a day and time shown in **their own timezone**. |
| **Real availability** | Open times come from weekly hours, days off and the busy times already on your Google Calendar, so you're never double-booked. |
| **Booking rules** | Buffers before and after meetings, minimum notice, how far ahead people can book, and a daily limit per meeting type. |
| **Calendar invites** | Every booking creates a real calendar event and sends the guest an invite, with a Google Meet link when you choose Meet. |
| **Share specific times** | Pick a few open slots and send them in a message, like Google's "Offer times". Each time is a one-click booking link. |
| **A full calendar** | Day, week and month views. Click an empty spot to create an event and invite people, just like Google Calendar. |
| **Reschedule & cancel** | Guests get a private link to move or cancel. Hosts can do the same from the Bookings page, and everyone is emailed. |
| **Reminders** | An email reminder goes out about 24 hours before each meeting. |
| **Small teams** | Add team members, each with their own booking page, hours and meeting types. Admins manage the team. |

## Security

- Salted, hashed passwords, expiring sessions, and lockout after repeated failed logins
- Guests only ever see open times, never what's on your calendar
- Bot trap and a per-email booking limit on the public form
- Every server request checks who's signed in and what they're allowed to change
- All text escaped (no XSS) and protected against spreadsheet formula injection
- Locking so two people can't grab the same slot, plus an activity log

## Tech stack

Google Apps Script · Google Sheets · Google Calendar (CalendarApp + Calendar API for Meet) · MailApp · HTML/CSS/JavaScript

## Project structure

```
apps-script/
├── Code.gs       # server: sign-in, availability engine, calendar, bookings, emails, demo data
└── index.html    # the whole front end: booking pages, calendar, dashboard
index.html        # GitHub Pages wrapper that loads the live app and passes booking links through
```

## Run your own copy

1. Create a **new, blank** Google Sheet, then open **Extensions → Apps Script**.
2. Paste `apps-script/Code.gs` into `Code.gs`. Add an HTML file named `index` and paste `apps-script/index.html`.
3. Select the `setup` function and click **Run**, then approve the permissions.
4. **Deploy → New deployment → Web app**, with *Execute as: Me* and *Who has access: Anyone*.
5. Paste the `/exec` link into the root `index.html` (the line marked ⬇️) for GitHub Pages.

### Real use (your own calendar)

1. Make a **second copy** with a new blank Sheet (keep the demo separate).
2. In `CONFIG`: `DEMO_MODE: false`, `CALENDAR_MODE: 'google'`, and fill in `OWNER_EMAIL`, `OWNER_NAME`, `OWNER_SLUG`, `OWNER_PASSWORD`. Set `PUBLIC_URL` to your GitHub Pages link so booking links use it.
3. For Google Meet links: **Services → + → Google Calendar API → Add**.
4. Run `setup`. It creates your admin account, a "30 min with you" meeting type and the hourly reminder trigger.
5. **Delete the password from `OWNER_PASSWORD`**, save, and deploy.
6. Team members share their Google Calendar with the script owner's account (*Make changes to events*). If one isn't shared, their bookings go on the owner's calendar and they're invited.

> No Google Calendar? Set `CALENDAR_MODE: 'sheet'`. The app keeps its own calendar in the Sheet and emails standard `.ics` invites that work with Google, Outlook and Apple Calendar.

---

Built by **MA System Solutions**.
