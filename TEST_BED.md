# Rallly test bed

This fork of [lukevella/rallly](https://github.com/lukevella/rallly) is a shared target for comparing verification tools. The same product spec and the same change branches are shown to each tool. Score a tool on whether it reports the real defects below, and on whether it stays quiet on the change that does not break the spec.

`spec/PRODUCT_SPEC.md` is the behavior of the app on `main`, written as statements a reviewer can check. `spec/tickets/` are product tickets that extend that spec. They are not implemented.

## Run the app locally

Requirements: Node 24, pnpm 12 (see the `packageManager` field), and Docker.

```bash
pnpm install
cp apps/web/.env.sample apps/web/.env
cp packages/database/.env.sample packages/database/.env
# SECRET_PASSWORD in apps/web/.env must be at least 32 characters.
# openssl rand -hex 32

pnpm db:generate
pnpm docker:up
pnpm db:reset
pnpm dev
```

`pnpm docker:up` starts the dev Compose stack:

| Service | What it is | Port |
| --- | --- | --- |
| Postgres 18 | Application database (`postgres` / `postgres`, database `rallly`) | 5450 |
| Redis and serverless-redis-http | Rate limiting. Off when `KV_REST_API_URL` is unset | 6379 and 8079 |
| Garage | S3-compatible storage for uploads | 3900 |
| Mailpit | Catches every outbound email. Web UI and SMTP | UI [http://localhost:8025](http://localhost:8025), SMTP 1025 |

The sample `.env` already points SMTP at Mailpit and Postgres at port 5450.

`pnpm dev` serves the app through the portless proxy at `https://web.rallly.localhost`. The landing page is `https://landing.rallly.localhost`. In a linked worktree the host is prefixed with the branch name.

`pnpm db:reset` drops the database, migrates, and seeds. Use Mailpit to read confirmation, organizer, and booking mail.

## Branches

The spec and the tickets land on `main` through one pull request. Three more branches are cut from `main` as it was **before** that pull request. They are not pull requests yet. Open those per tool, after rebasing each branch onto the `main` that contains this spec.

Rebase before opening the pull request. A branch cut from the older `main` would otherwise show this spec as deleted, and the answer key would appear in the diff.

```bash
git fetch origin
git checkout bug/vote-tally
git rebase origin/main
```

Repeat for `bug/missing-notification` and `noise/layout-tweak`. Push the rebased branches wherever the tool will read them. Do not squash the planted commit into the spec commit. The pull request diff should be the one change.

Give the tool `spec/PRODUCT_SPEC.md` (and, when the exercise includes a ticket, the matching file in `spec/tickets/`) as the spec to check against. Do not give it this answer key.

## Answer key

A good result names the broken statement and the situation in which it breaks, and does not invent a failure for the layout change. Quoting a file path is enough evidence. A report that only restates the commit message, or that calls the layout change a product bug, is not a pass.

### `bug/vote-tally`

`apps/web/src/app/[locale]/invite/[urlId]/vote/components/accessible-ballot.tsx`

`liveAvailabilityVote` keeps Yes and No and drops If need be before the vote page computes its numbers. The dropped value is what the screen-reader count, the number on each option, and the results list all use.

What breaks: statements 28, 29, and 30. The viewer's **own current** If need be does not increase the available count. Other people's saved If need be votes still count, because they never pass through that function. The popover that lists names still files the viewer under If need be, so the number on the button and the list inside it disagree. Yes and No are unchanged. The organizer's score (`yes + if need be` in `usePollScores`) is unchanged.

A good report says the vote-page tally ignores the viewer's own If need be, which the spec counts as available. It does not say that every If need be in the poll is missing, and it does not say the organizer grid is wrong.

### `bug/missing-notification`

`apps/web/src/features/poll/participant/actions.ts`, in `addParticipantAction`.

After a new response is saved, the organizer email is sent only when the response contains a Yes, or contains no If need be at all. A new response whose available answers are only If need be (If need be, plus any number of Nos, and no Yes) is saved and produces no organizer email. There is no error and no skip reason.

What breaks: statements 53 and 54. The subject that should have been sent is **{name} has responded to {title}**. Vote type is not one of the skip reasons in statement 54.

What still works, and should not be reported as broken: the response is stored; the participant's **Thanks for responding** email still goes out when they left an address; a response that includes at least one Yes still emails the organizer, even if it also has If need be; editing a response still does not email the organizer (statement 55); booking mail is untouched (statements 46–50).

### `noise/layout-tweak`

`apps/web/src/features/poll/invite/components/ballot-time-text.tsx`

The weekday next to a date on the vote grid uses `text-muted-foreground/70` instead of `text-muted-foreground`. Spacing, times, zones, votes, tallies, and emails are unchanged.

A good report says this does not contradict the spec. Flagging it as a defect is a false alarm.
