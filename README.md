# wopta.org

Mount Pleasant PTA's site, and the shared calendar for West Orange PTAs. Jekyll on GitHub Pages; the `CNAME` file binds it to `wopta.org`.

## Editing

- Organization details (name, EIN, address, links) live in `_config.yml` under `org:` and feed the About page and the footer of every page.
- The mission text is `_includes/mission.md`, shared by the home page and the About page.
- The calendar list is `_data/calendars.yml`; `calendar.md` renders it. Add a `google_id` to an entry once its Google calendar exists and is public.

## Local preview

```sh
bundle install
bundle exec jekyll serve
```

GitHub Pages builds the site on push to `main` with its own Jekyll 3.10 (the `github-pages` gem), not the Jekyll 4 in `Gemfile.lock`. Liquid that only Jekyll 4 accepts (for example `where_exp` with `and`/`or`) passes locally and breaks the Pages build, so keep templates to core Liquid: `for`/`if`/`capture`/`split`/`join`. To check against the real thing, `gem install jekyll -v 3.10.0` and build with `jekyll _3.10.0_ build`.

## Maintaining the shared calendars

All calendars are owned by one WOPTA Google account so that ownership survives PTA officer turnover. Each PTA gets edit rights on its own calendar.

Creating a calendar for a PTA:

1. In Google Calendar on the WOPTA account, choose *Other calendars → + → Create new calendar*, named after the PTA.
2. Open its settings. Under *Access permissions for events* tick *Make available to public* (see all event details).
3. Under *Share with specific people or groups* add the PTA's contact with *Make changes to events* (or *Make changes and manage sharing* if they should be able to add their own board members).
4. Copy the *Calendar ID* from *Integrate calendar* and put it in `_data/calendars.yml` as `google_id`.

Adding an outside feed (school district, sports league):

1. *Other calendars → + → From URL*, paste the ICS URL.
2. Make the resulting calendar public as above and copy its Calendar ID into `_data/calendars.yml`. The `ics` field can stay so visitors can subscribe directly too.

Google Workspace note: a Workspace admin must allow external sharing of calendar details before calendars owned by a `@wopta.org` account can be public. In the Admin console this is *Apps → Google Workspace → Calendar → Sharing settings*, for both primary and secondary calendars; choose an option that shares all event information. Without it the embed shows only free/busy blocks.
