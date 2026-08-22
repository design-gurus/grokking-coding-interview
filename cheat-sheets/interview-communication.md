# How to Talk Through a Coding Interview

The code is graded, and so is everything around it. Two candidates who reach the same solution are
scored differently depending on how much of the thinking was visible, because the interviewer is
guessing at how you would behave on a real task with a real team.

This is the order that works, with the sentences that carry the most weight.

## The order

| Minutes | What you do | Why it counts |
|---|---|---|
| 0 to 2 | Restate the problem, ask about the input | Catches a misread before it costs you the round |
| 2 to 4 | State the brute force and its cost | Proves you understood the question |
| 4 to 6 | Read the constraints, name the pattern | This is where most of the signal is |
| 6 to 8 | Walk one small example by hand | Settles the invariant before the code |
| 8 to 25 | Write it, narrating as you go | Lets the interviewer help you |
| 25 to 30 | Test out loud on the edge cases | Finds your own bugs before they do |
| 30 to 35 | State the complexity, discuss trade-offs | Where senior candidates separate |

Timings are for a 35 to 45 minute round with one problem. Adjust, but keep the order.

## 1. Restate and clarify

Say the problem back in one sentence, then ask about the input. Good questions, in rough order of
value:

- How large can the input be?
- Is it sorted? Can I sort it?
- Can it be empty? Can there be duplicates? Can values be negative?
- What should I return when there is no answer?
- Can I modify the input?

Two minutes here is never wasted. See the [edge cases checklist](edge-cases-checklist.md).

## 2. State the brute force

> "The obvious version is a nested loop over every pair, which is O(n²). Let me see if I can do better."

Do not skip this because it feels beneath you. It proves you understood the problem, it gives you
something to improve, and if you run out of time it is a working answer rather than nothing.

## 3. Read the constraints out loud, then name the pattern

This is the highest-value sentence in the whole interview:

> "n is up to 10^5, so O(n²) is about 10^10 operations and will not pass. I need something closer to
> linear. The answer has to be a contiguous subarray and the values are all positive, so this is a
> sliding window."

Three things happen at once. You show the constraints were read, you rule out the brute force with
arithmetic instead of a hunch, and you name the approach. See
[what complexity passes](what-complexity-passes.md) and
[how to recognize the pattern](recognize-the-pattern.md).

## 4. Walk one example by hand

Take the smallest interesting input and step through your idea on the whiteboard or in a comment.
This is where you find out the plan is wrong, and finding out now costs two minutes rather than
fifteen.

Name the invariant while you do it: "the window always holds at most k distinct characters", "the
stack always holds the indices still waiting for a greater value". A candidate who can state the
invariant almost always writes correct code. One who cannot almost always does not.

## 5. Write it, narrating

Narrate at the level of intent, not syntax. "Now I shrink from the left while the window is still
valid" is useful. "Now I write a while loop" is noise.

If you get stuck, say what you are stuck on. Interviewers are usually allowed to help, and they
cannot help with silence. "I need the largest value in the window and a heap will not let me remove
the element that leaves. I am trying to remember the structure that does" is an invitation, and often
gets you the hint.

Write the helper functions you wish you had, and fill them in afterwards if there is time. A clear
`isValid(state)` call is better than a nested condition nobody can read.

## 6. Test out loud

Run your own code on paper, on the smallest input first. Then the edge cases you named in step 1.

> "Empty array: the loop never runs and I return the initial value, which is correct. One element:
> start and end are both zero, and the length calculation gives one. Two equal values: this is the
> case I was worried about, let me trace it."

Finding your own bug is a positive signal. Having the interviewer find it is a neutral one. Having it
found by a hidden test is a negative one.

## 7. Complexity and trade-offs

State time and space without being asked, and say what n and k are.

> "Time is O(n) because each index enters and leaves the window once, even though there is a nested
> loop. Space is O(k) for the frequency map, where k is the alphabet size."

Then offer the trade-off you did not take:

> "If I were allowed to sort, two pointers would do it in O(1) extra space, but the question wants
> the original indices, so I kept the map."

## Sentences worth memorizing

| Situation | What to say |
|---|---|
| You do not know the problem | "I have not seen this one. Let me work from the constraints." |
| You are stuck | "I am stuck on X. My options are A and B, and I am leaning toward A because..." |
| You realize the approach is wrong | "This does not handle negative values. I need to back up." |
| You are asked to optimize | "The bottleneck is the inner search. If I remember what I have seen in a map, that becomes O(1)." |
| The interviewer hints | "That is a good point, so let me redo the shrink condition." |
| You finish early | "Let me test a few more cases, then I can talk about how this changes if the input does not fit in memory." |
| You are almost out of time | "I have the approach and the loop. Let me state the remaining cases rather than half-write them." |

## What loses points

- Silence longer than about twenty seconds.
- Writing code before saying what it will do.
- Ignoring a hint. A hint is not a comment, it is an instruction.
- Arguing when corrected instead of checking.
- Claiming a complexity you have not thought about, especially calling a hash map O(1) worst case.
- Saying "this is easy" and then getting it wrong.
- Optimizing before anything works.

## When you have seen the problem before

Say so. "I have seen this one, so let me tell you the approach and then walk through why it works."
Pretending to derive a memorized answer is obvious, and the interviewer will usually just make the
problem harder, which is fine and is a much better outcome than being caught.

## Go deeper

- [How to recognize the pattern in sixty seconds](recognize-the-pattern.md)
- [Edge cases checklist](edge-cases-checklist.md)
- Practice with a person: [Mock interviews with ex-FAANG engineers](https://www.designgurus.io/mock-interviews)
