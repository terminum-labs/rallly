# Close voting at a deadline

## Background

Voting stays open until the organizer closes the poll, books a date, or housekeeping closes it because every option has ended. Organizers often want responses in before a meeting is announced, while the dates themselves are still in the future. A `deadline` column exists on the poll, and seed data writes it, but the application never reads it. Passing a clock time does not close voting. See the product spec, "When voting stops".

## Outcome

The organizer can set a voting deadline. When that moment has passed, the poll stops taking new responses and stops taking edits, and the vote page says that voting closed because the deadline passed.

## Acceptance criteria

1. While creating or editing an open poll, the organizer can set a deadline as a date and time, or leave it unset. Unset means today's behavior: voting stays open until the poll is closed, booked, or auto-closed.
2. The deadline is shown on the poll and on the vote page before it has passed, in the same time-zone rules as the rest of that poll (a fixed poll converts it for the viewer; a floating or all-day poll shows the wall time or the calendar date without shifting it).
3. Once the deadline has passed, a new response is refused and an existing response cannot be edited, renamed, or deleted. This holds for participants and for poll admins, matching a closed poll.
4. The vote page still shows the options, the tallies, and the comments (when comments are on). It says that voting is closed because the deadline passed, which is distinct from a manual close and from a booked date.
5. The organizer can clear or move a deadline that has not been reached. Moving it into the past closes voting immediately. Clearing it on a poll that closed only because of the deadline makes voting open again, unless the poll was also manually closed or booked.
6. Booking a date and a manual close still stop voting even when the deadline is in the future. Auto-close after every option has ended is unchanged.
7. No email is required by this ticket. Notification behavior stays as in the product spec.
