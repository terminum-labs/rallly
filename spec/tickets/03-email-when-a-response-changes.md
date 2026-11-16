# Email the organizer when a response changes

## Background

Emailing participants when the final date is chosen already exists. The organizer picks the addresses, each selected address gets **Date booked for {title}** with a calendar invite, and the organizer gets their own copy. That is specified under "Finalizing a date, and the emails that go out" and is not this ticket.

The gap next to it is the other direction. The organizer is emailed **{name} has responded to {title}** when someone submits a response for the first time. Saving changes to a response that already exists updates the votes and sends nothing, so the tally can move with no mail. See the product spec, statements 53 and 55.

## Outcome

Editing a saved response emails the organizer, under the same rules as a new response.

## Acceptance criteria

1. When a participant or a poll admin changes the votes on an existing response, and the poll is still open, the organizer receives one email. The subject identifies the poll and the participant, and the body says the response was updated and links to the poll.
2. The mail goes out for every combination of Yes, If need be, and No, including a change that only adds or removes If need be.
3. It is skipped in exactly the cases that already skip the new-response email: no organizer, the organizer made the edit themselves, the organizer is a guest, the organizer has left the space, the poll is deleted or muted, or **New response** notifications are off.
4. The participant does not get a second "Thanks for responding" email because of the edit. Booking emails are unchanged: finalizing still emails only the people the organizer selects, and the organizer's own booking message still goes out.
5. Renaming a response or deleting it does not send this email.
6. A failed send does not roll back the saved votes, matching a new response whose notification fails.
