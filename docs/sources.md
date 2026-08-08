# Data sources

**37 polled feeds and 15 streaming accounts.** Every source carries a weight
that decides how far up the queue its events go when the model's attention is
the scarce resource.

| Weight | Count | What sits there |
|---|---|---|
| 1.00 | 7 | primary sources — government releases, the White House presidential-actions feed, SEC filings |
| 0.95 | 7 | wire services |
| 0.90 | 4 | major outlets |
| 0.85 | 3 | financial press |
| 0.80 | 8 | general news |
| 0.75 | 3 | sector press |
| 0.70 | 3 | aggregators that reprint the wires |
| 0.40–0.45 | 2 | multireddits — a source of early noise, not of facts |

## The streaming connection

One source is not polled at all. Reuters and AP closed their public RSS feeds,
which meant the two largest news agencies in the world reached us only through
whoever reprinted them. On Bluesky they publish first-hand, with no
authentication, no rate limits and no legal grey area.

A long-lived process subscribes to 15 accounts by DID — the handle changes,
the DID does not. **Measured: 16 and 22 seconds** from publication to our
database, against a 12-minute median for polled feeds. The first post that ever
arrived went the whole way — matched to 5 contracts, judged by the model,
refused — in four seconds.

Deduplication is shared with the polled path, so a Reuters post and a Guardian
article about the same event collapse into one event.

## Things that had to be learned the hard way

**A feed can return 200 OK and still be dead.** `feeds.a.dj.com` served valid
XML with twenty real headlines, the freshest of them eighteen months old. It
returned zero new events on 222 consecutive runs, and by every counter we had
that looked identical to "a quiet week". We now record the newest item *in the
feed*, before any freshness filter — the only way to tell a frozen feed from a
quiet one.

**403 and 429 are statements about us, not about the feed.** Ordinary retry
backoff turned them into knocking on a closed door every twenty minutes
forever. A source that refuses access is now switched off for an hour, then
twice as long, up to a day. A successful poll clears it; the counter of
shut-offs remains.

**A release schedule is better than a rule that derives it.** Jobs-report dates
used to come from "first Friday of the month". The rule is correct for 2026 —
and that is exactly what makes it dangerous, because BLS occasionally moves a
release and the rule would miss silently, on the one day the spike matters
most. It is now an explicit list. PPI dates could not be obtained at all, and
an empty list is more honest than an invented rule.

## Usefulness is measured, weekly

A weekly job follows each source through the whole funnel: events → first to
report → matches → verdicts → trades. Candidates for removal are named by
**verdicts**, not by event count: a feed producing two hundred headlines a day
that miss every contract is worse than one producing three that land. A source
younger than the reporting window is not judged.

---

[← All documentation](https://fluxrbot.com/docs/) · [How it works](https://fluxrbot.com/#how) · [fluxrbot.com](https://fluxrbot.com)
