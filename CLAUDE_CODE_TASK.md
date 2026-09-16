# Task: NYCC Artist Alley + Exhibitors — Redbubble/TeePublic/Dashery presence check

## Objective
For each name in `remaining_artist_alley.txt`, determine whether that artist has a
confirmed personal storefront on:
- redbubble.com
- teepublic.com
- dashery.com

Then repeat the process for NYCC's Exhibitors list (companies/small presses), which
still needs to be scraped/pasted separately — see "Exhibitors" section below.

## Critical methodology lesson (already learned the hard way — don't relearn it)
Plain "[name] + redbubble" or "[name] + teepublic" web searches are UNRELIABLE on their own:

1. **False positives**: Redbubble and TeePublic auto-generate a "[Name] Merch & Gifts
   for Sale" landing page for almost any searchable term, populated with OTHER
   independent sellers' fan art of that person's characters — not a shop run by the
   artist themselves. Do not count these as a hit.
2. **False negatives**: a real personal shop that isn't SEO-prominent may not surface
   in a generic search at all.

**The reliable signal**: find the artist's own linked bio (personal website, Linktree,
carrd, Twitter/Instagram/Tumblr bio, "shop" or "prints" link list) and check whether
THEY link out to a redbubble.com/people/…, teepublic.com/user/…,
teepublic.com/stores/…, or dashery.com storefront under their own name/handle.
A username/handle match to the artist's known online identity is strong corroborating
evidence (e.g. "Femmmeow" → redbubble.com/people/femmmeow/shop).

## Confidence labels to use
- ✅ Confirmed — found via the artist's own linked bio/site, or an exact username/handle
  match to their known identity plus content that matches their actual work.
- ？ Likely — plausible username/handle match but not confirmed via their own linked bio.
- ❌ No evidence — nothing found after a reasonable search; note where they DO sell
  (own site, BigCartel, Shopify, Patreon, Threadless, Society6, INPRNT, Etsy, etc.) if found.

## Output format
Append rows to `/mnt/user-data/outputs/nycc_artist_alley_check.md` (the existing tracker —
do not recreate it from scratch; read it first and continue its exact table format):

| Artist | Redbubble | TeePublic | Dashery | Notes |
|---|---|---|---|---|

Update the "## Status" section's checked-count and confirmed-hits list as you go.
Work through `remaining_artist_alley.txt` in order, batching your own tool calls as
efficiently as possible (you do not need to check in with the user between batches —
this is the whole point of running unattended). Periodically save progress (e.g. every
25–50 names) so partial results survive if interrupted.

## Already confirmed hits (for context — don't re-check these, they're done)
- Acorviart — RB ✅ + TP ✅ (own bio: "PRINT SHOPS: inPRNT, teepublic, redbubble")
- Agnes Garbowska — RB ✅ (redbubble.com/people/agnesgarbowska/shop)
- Arielle Jovellanos — RB ✅ (former Redbubble Artist-in-Residence, 2017)
- Ayu Yamane — RB ✅ (own site: "order Ayu Yamane original goods on redbubble.com")
- Danny Haas — TP ✅ (teepublic.com/stores/the-art-of-danny-haas)
- Femmmeow — RB ✅ (redbubble.com/people/femmmeow/shop)
- HAZMATEN — RB ✅ (own linktree lists "Redbubble")
- INKPULP (Shawn Crystal) — TP ✅ (teepublic.com/user/inkpulp)
- Karen Hallion Illustration — RB ✅ + TP ✅ (own bio links both)
- Brianna Garcia Illustration — RB ？likely (redbubble.com/people/cherrygarcia/shop,
  matches her "Cherry Garcia" handle — worth a final confirm if easy)

## Input file
`remaining_artist_alley.txt` — 465 names, alphabetically ordered, already deduplicated
against names checked so far.

## Exhibitors list (second phase, after Artist Alley is done)
The user still needs to supply the Exhibitors list content (from
https://www.newyorkcomiccon.com/en-us/exhibitors-and-artists/exhibitors.html — the page
is JS-rendered and not fetchable directly; the user will need to paste the rendered
list, same as they did for Artist Alley). Once supplied, run the identical process and
create a second tracker file, `nycc_exhibitors_check.md`. Exhibitors are more likely to
be companies/small presses, some of which may already be legitimate wholesale/POD
sellers, so the same false-positive caution applies (a company name matching a generic
Redbubble collection page is not evidence of an actual account).

## Context on why this matters
The user works at Articore (Redbubble/TeePublic's parent company) and is attending
NYCC in person; the goal is a practical list of which exhibiting artists already sell
on Articore-owned platforms, to inform who to prioritize connecting with in person.
