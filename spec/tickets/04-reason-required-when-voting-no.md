# Require a reason when a participant votes no

## Background

A No is saved with no explanation. The note on a response is optional for every answer, including a response that is No on every option the participant touched, and only the organizer sees it. Public comments are a separate thread, off by default, and are not tied to a vote. See the product spec, statements 21 and 39.

## Outcome

A response that votes No on any option cannot be saved until the participant leaves a note saying why. Public comments stay optional and separate.

## Acceptance criteria

1. On the vote page, if the participant's current answers include at least one No, the note field is shown and required. The hint still says that only the organizer will see it.
2. Saving is refused, in the form and on the server, while that note is empty or only whitespace. The message says a note is required because they voted No.
3. A response that is only Yes and If need be can still be saved with no note.
4. An option left unanswered is stored as No. That stored No counts: if any option is unanswered or explicitly No, the note is required. A participant who is available for every option they were offered (Yes or If need be on each one) is not asked for a note.
5. Editing an existing response uses the same rule. Removing the last No, or filling every option with Yes or If need be, makes the note optional again. Adding a No to a response that had no note blocks the save until a note is entered.
6. The note remains private to the organizer and the author. The public comment thread is unchanged: comments stay off unless the organizer enables them, and a comment is never required in place of the note.
7. Tallies are unchanged. No still does not count as available.
