# Rallly product spec

This describes how Rallly behaves today. Each statement is something a reviewer can check on a running poll. A change that makes the product do otherwise is a defect. A change that leaves these statements true is not.

The words below are the product's own: **Yes**, **If need be**, and **No**.

## Creating a poll

1. An organizer creates a poll with a non-blank title and at least one option. A description, a location, and a video call are optional.
2. A description longer than 8000 characters is rejected. A note on a response longer than 500 characters is rejected. A public comment longer than 2000 characters is rejected, and a comment author's name longer than 100 characters is rejected.
3. The poll starts **open**. People can vote only while it is open.
4. The create form starts with these settings: require an email address, comments off, If need be offered, votes visible, participant names visible, and the time zone not locked.
5. Anyone can create a poll. A visitor without an account gets a guest session. A signed-in member creates the poll in their current space.

## Options

6. A poll is either a **date poll** or a **time poll**. If every option is an all-day date (no start and end time), it is a date poll. If any option has a duration, it is a time poll.
7. Each option is one date, or one start time plus a duration in minutes. Options are shown in start-time order.
8. A time option's end is its start plus its duration. An all-day option covers that calendar date.

## Time zones

9. On a time poll whose time zone is not locked, the poll stores an IANA time zone. Each option is a fixed instant. The vote page converts every time into the viewer's selected time zone.
10. That vote page says which city the times are shown in. Choosing another city from that control redraws every time on the page in the new zone. It does not change anyone's vote.
11. A time poll also has a 12-hour / 24-hour switch. Changing it redraws the clock times and does not change votes.
12. When the organizer locks the time zone, the poll stores no zone. Every viewer sees the same wall-clock time. The vote page does not offer a city control.
13. A date poll stores no zone. Every viewer sees the same calendar date. There is no city control and no 12-hour / 24-hour switch. All-day dates are not shifted by the viewer's zone.
14. After at least one vote exists, the organizer can no longer change whether the time zone is locked or which zone a converting poll uses.
15. A fixed time slot is in the past when its start instant is before now. A floating time slot, and a date, is in the past by the viewer's own clock: a date is past once that calendar day has ended where the viewer is. Past options are hidden until the viewer chooses to show them, and they cannot be voted on. After a date is booked, the booked option stays visible even if it is past.

## Voting

16. Each option takes one answer: **Yes** (can make it), **If need be** (can make it, but would rather not), or **No** (cannot make it).
17. If need be is offered only while the poll allows tentative votes. That is the default. When it is turned off, the choices are Yes and No. The server rejects a new If need be vote on such a poll.
18. If need be votes already cast stay stored and stay visible after the organizer turns tentative votes off. The organizer cannot turn tentative votes off once any If need be vote exists. Turning them back on is allowed.
19. On the vote page, clicking a row cycles the answer: empty, then Yes, then If need be, then No, then Yes. When If need be is off, the cycle is empty, Yes, No, Yes.
20. A response must answer at least one option. Any option the participant leaves unanswered is saved as No.
21. A response needs a name. Email is optional unless the poll requires it. When an email is entered, it has to be a valid address. A note is optional, and only the organizer and the author see it.
22. No account is required to vote. Submitting creates a guest session when the visitor does not already have one.
23. A poll accepts at most 1000 responses.
24. While the poll is open, a participant can change their votes, change their name, or delete their response. They prove it with the session that created the response, or with the edit link from their confirmation email. A poll admin can do the same. Nobody can add or edit a response once the poll is closed, scheduled, or canceled, including an admin.
25. Saving a change replaces that response's votes with the new set.
26. While **Hide votes** is on, a viewer who has not responded sees no tallies. After they respond, tallies appear.
27. While **Hide participant names** is on, a viewer who is not the organizer does not see other people's names. Those people's votes still count. The viewer still sees their own response. Other people's notes and email addresses are never shown to a fellow participant.

## Tallies

28. Someone is **available** for an option when their answer is Yes or If need be. No is not available. An unanswered option saved as No is not available.
29. The number on each option on the vote page is how many people are available for it. If need be counts toward that number the same as Yes, whether it is someone else's saved answer or the viewer's own current answer.
30. While the viewer is choosing, the number uses their current answer in place of their saved one. If they have no saved response yet and the poll is still open, the total includes them, and they add to the available number when their current answer is Yes or If need be.
31. Opening the number lists who answered what, grouped as Yes, then If need be, then No. The bar is green for Yes and amber for If need be. The fraction is available people over responses, with available defined in statement 28. When names are hidden, the number still shows and this list does not.
32. On the organizer's results and in the finalize dialog, the score beside an option is the same sum: Yes plus If need be. Options are ordered by that sum, highest first. A tie is broken by more Yes votes, then by more If need be votes. A remaining tie stays in option order.
33. That score is emphasized only when it is the highest and greater than 1. A single Yes is a score of 1 and is not emphasized.

## Comments

34. Public comments are off unless the organizer turns them on.
35. When they are on, a signed-in person or a guest can post. The comment needs text. A guest types the name to show. A signed-in person's comment uses their account name.
36. The author of a comment, and a poll admin, can delete it. Other participants cannot.
37. When participant names are hidden, a viewer who is not the organizer sees only their own comments.
38. Comments follow the comments setting, not whether voting is open. A closed or scheduled poll still accepts comments when comments are on.
39. A response note is not a public comment. It is private to the organizer and the author, and it is optional for every answer, including No.

## When voting stops

40. The organizer can close an open poll without booking a date. It becomes **closed** for a manual reason. Results stay visible. Voting stops. A closed poll that was not booked can be reopened.
41. There is no organizer-set voting deadline. The `deadline` column on a poll is not read by the app, and voting does not close when a clock time passes.
42. A separate housekeeping task closes an open poll, with reason **auto**, once every option has ended. An all-day option counts as ending 24 hours after its start. Adding a future option to an auto-closed poll reopens it. A manually closed poll stays closed until the organizer reopens it.
43. Booking a date (below) also stops voting. A booked poll cannot be reopened.

## Finalizing a date, and the emails that go out

44. A poll admin books one of the poll's options. The poll becomes **scheduled**, a calendar event is created, and the poll cannot be booked a second time.
45. For a fixed time poll, the event runs from that option's start to start plus duration, in the poll's zone. For an all-day option, the event is that calendar date with no zone, so every recipient sees the same date. A locked (floating) time poll keeps the wall-clock time and does not name a zone.
46. Before booking, the organizer chooses who to email. Everyone who left an email address on their response starts selected. A response with no email address cannot be selected, and that person is not emailed. The organizer can deselect anyone.
47. Each selected email address receives one message with the subject **Date booked for {title}**. It names the organizer and the poll, shows the date and time (and the place and video call, when the poll has them), says **You voted Yes**, **You voted If need be**, or **You voted No** when that person voted on the booked option, and attaches a calendar invite.
48. Two responses that share an email address produce one invite and one email. The status kept is the most committal: Yes beats If need be, If need be beats No, and No beats a response that never voted on that option.
49. The organizer always receives their own **Date booked for {title}** message, with a calendar attachment, whether or not they selected themselves. Its attendee list is the people who voted Yes or If need be on the booked option.
50. Participant booking emails are limited by the organizer's daily email budget. If the budget runs out, the date is still booked, the unsent people are skipped, and the organizer is told how many were not emailed. The organizer's own booking message is not part of that budget.
51. If the booked event is later canceled, every invitee who had not declined receives a cancellation email with a cancellation calendar attachment. Someone whose invite was declined does not.

### Other mail a poll sends

52. When a new response is saved and the participant typed an email address, they receive **Thanks for responding to {title}** with a link to edit that response, unless the organizer's daily email budget is used up. A skipped confirmation does not undo the response.
53. When a new response is saved, the organizer receives **{name} has responded to {title}**. The message includes the participant's name, their note when they left one, and a link to the poll. It is sent for every combination of Yes, If need be, and No. A response whose only available answers are If need be still sends it.
54. That organizer message is skipped, and only skipped, when one of these is true: the poll has no organizer, the organizer submitted the response themselves, the organizer is a guest, the organizer is no longer a member of the poll's space, the poll is deleted, the poll is muted, or the organizer has turned off the **New response** notification. The answers on the response are not a reason to skip it.
55. Editing an existing response does not send statement 52 or statement 53 again. Deleting a response does not send them either.
56. When a public comment is posted, the organizer receives **{authorName} has commented on {title}** with the comment text and a link to the poll. The same skip rules as statement 54 apply, read against the **New comment** preference and the person who posted. Other participants are not emailed about the comment.
