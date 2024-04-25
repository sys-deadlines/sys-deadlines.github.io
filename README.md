# Systems conference deadlines countdown

Based on [sec-deadlines](https://sec-deadlines.github.io) which is based on
[ai-deadlines](https://aideadlin.es) by @abshkdz.

## Adding/updating a conference

* Read the data format description below. **Note that the timezone format sign is inverted** (e.g., UTC+7 is written as `Etc/GMT-7`). It's [not a bug][0]. I hate this format too. I'd be happy to move to a different timezone JavaScript library that uses a friendlier format, but I don't have time for that.
* Update `_data/conferences.yml`. You can do that on GitHub or locally after forking the repo.
* Send a pull request

### Conference entry record

Example record:

```
- name: USENIX ATC
  description: USENIX Annual Technical Conference
  year: 2025
  link: https://www.usenix.org/conference/atc25
  comment: Collocated with OSDI 2025
  deadline: "2025-01-10 23:59"
  date: "July 7-9"
  timezone: Etc/UTC
  place: Boston, MA, USA
  tags: [CONF, SYSTEM, USENIX]
```

Descriptions of the fields:

| Field name    | Description                                                                             |
|---------------|-----------------------------------------------------------------------------------------|
| `name`\*      | Short conference name, without year                                                     |
| `year`\*      | Year the conference is happening                                                        |
| `description` | Description, or long name                                                               |
| `comment`     | Additional comments, e.g., co-located conference, rolling deadline                      |
| `link`\*      | URL to the conference home page                                                         |
| `deadline`\*  | A list of deadlines. [(Gory details below)][4]                                          |
| `timezone`    | [Timezone][5] in [tz][1] format. By default is UTC-12 ([AoE][2])                        |
| `date`        | When the conference is happening                                                        |
| `place`       | Where the conference is happening                                                       |
| `tags`        | One or multiple [tags][3]: `SEC`, `PRIV`, or `CRYPTO` (topic); `CONF` or `SHOP` (venue) |

Fields marked with asterisk (\*) are required.


### Deadline format

The *deadline* field can contain:

1. The simplest option: a date and time in ISO format. Example: `["2017-08-19 23:59"]` (Note that you need to wrap even a single deadline in a list).
2. If a deadline is rolling, you can use a template date, just substitute the
   year with `%y` and the year before the conference with `%Y`. Example:
   `["%y-01-15 23:59"]` means there is a deadline on the 15th January in the
   same year as the conference.
2. A list of (1) or (2). Example of two rolling deadlines, with one in the end
   of October in the year prior to the conference year, and the second in the
   end of February in the same year as the conference:
  ```
  - "%Y-10-31 23:59"
  - "%y-02-28 23:59"
  ```

On the page, all deadlines are displayed in viewer's local time (that's a feature).

*Note:* If the deadline hour is `{h}:00`, it will be automatically translated into `{h-1}:59:59` to avoid pain and confusion when it happens to be midnight in local time.

### Timezones

The timezone is specified in [tz format][1]. Unlike abbreviations (e.g. EST), these are un-ambiguous. Here are tz codes for some common timezones:

| Common name                   | tz                                                                 |
|-------------------------------|--------------------------------------------------------------------|
| UTC                           | `Etc/UTC`                                                          |
| America Pacific Time          | `America/Los_Angeles`                                              |
| Pacific Standard Time (UTC-8) | `Etc/GMT+8` (Yes, the sign is inverted for some weird reason)      |
| America Eastern Time          | `America/New_York`                                                 |
| Eastern Standard Time (UTC-5) | `Etc/GMT+5`                                                        |
| American Samoa Time (UTC-11)  | `Pacific/Samoa` or `Etc/GMT+11`. This timezone does not use DST.   |
| Aleutian Islands              | `America/Adak`                                                     |

[0]: https://momentjs.com/timezone/docs/#/zone-object/offset/
[1]: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
[2]: https://www.timeanddate.com/time/zones/aoe
[3]: _data/types.yml
[4]: #deadline-format
[5]: #timezones
