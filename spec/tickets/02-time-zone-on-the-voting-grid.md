# Show the time zone on the voting grid

## Background

A time poll that is not locked already converts each option into the viewer's zone, and a line above the list names the city ("Times shown in {city} time"). The rows themselves show a clock time with no zone abbreviation, so a reader who misses that line can take "9:00 AM" as their own zone or as the organizer's. A locked time poll, and a date poll, do not say on the grid that everyone is looking at the same clock time or the same calendar date. See the product spec, "Time zones".

## Outcome

The voting grid states the zone the times belong to, on the grid, not only in a control above it.

## Acceptance criteria

1. On a fixed time poll, every time shown on the vote page includes the short zone name for the zone currently selected (for example, "9:00 – 10:00 AM EDT"). Changing the city control updates the times and the abbreviation together.
2. The day headings and the option rows agree. A time is never shown in one zone while its heading is in another.
3. On a locked (floating) time poll, the grid says that the clock times are the same for every participant and are not converted. It does not invent a zone abbreviation.
4. On a date poll, the grid does not add a time-zone abbreviation to the dates. It does not imply that the calendar date shifts for the viewer.
5. Past-time hiding, the 12-hour / 24-hour switch, tallies, and saved votes are unchanged. Switching zone or hour cycle still does not change stored votes.
6. The organizer's results for the same poll use the same labeling, so the host and a participant are not shown different zone rules for the same option.
