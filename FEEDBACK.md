# Client review: Larkspur disruption-care agent

**To:** Room 17  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 11:30

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py hold_seat and send_confirmation descriptions are 73 and 71 characters, and the diff leaves both untouched.**

hold_seat reads "Place a 15-minute hold on one alternative. Reversible. It simply expires." and send_confirmation reads "Send the customer a written confirmation of what was just done. Benign." Neither says when to call it or what comes first. The diff rewrote search_alternatives and left these two as shipped. The committed trace calls none of the three tools, so no run in the repo shows what the model does when it reaches rebooking.

Run python3 run.py --all --trace and paste the totals footer, so the tool order for every booking shape is on the page.

**2. agent.py search_alternatives description now says "if the get_flight_status returns an empty list", a condition the status tool may not produce.**

You replaced the 6-character "search" with "Search for alternate flights if the get_flight_status returns an empty list." Nothing in the diff or the material shows get_flight_status ever returning an empty list. A cancelled or delayed flight returns a status, so the model has been told to search only on a trigger that may never fire. The committed trace in readout-trace.json never calls search_alternatives.

Run python3 run.py K7PQ2M --trace and paste the tool-call list, showing whether search_alternatives is ever reached on a cancelled flight.

**3. agent.py fare_rules is the only tool you added, and the committed trace calls it last, after check_policy.**

The new fare_rules description tells the model to call it "after check_policy" for why-questions, and readout-trace.json shows lookup_booking, get_flight_status, check_policy, fare_rules in that order. That is 4 API turns and 4 tool calls for one run. The tool takes no parameters and returns the whole file as handbook_text, and the committed trace does not show how many of the 15137 input tokens that file accounts for.

Run python3 run.py --tool-tax and paste the output, so the per-turn cost of carrying fare_rules is a number.

**4. The repository has no evals/cases.json, and the only measured numbers are one trace: 15137 in, 861 out, 9.4s, 0 cache reads.**

The scan finds no eval cases, no cache_control anywhere in agent.py, and a trace that reports 0 read and 0 written with hit ratio None. Every turn re-sends runtime_preamble() + SYSTEM_PROMPT + 10 tool schemas, and thinking is on with {"type": "adaptive"}. Nothing in the repo says which of those drives the 15137 input tokens or the 9.4s. A bigger model does not change an uncached prompt that is re-sent on each of 4 turns.

Run python3 bench.py --label before --stage 1 --runs 3 and paste the summary, so there is a baseline for cost and speed.

**5. PITCH.md is blank, and TONE_ADDENDUM in agent.py is still 0 characters.**

The Built, Does, Number, Safety check, Next and Still broken lines are empty, the Lever line still reads "<cost | speed | intelligence>", and all four of Priya's answers are empty. TONE_ADDENDUM has no text, so the lever is not yet visible in code. Verifier and harness files differ from the shipped pack, so any number from verify.py or eval_harness.py needs a note on which copy produced it.

Run python3 verify.py 1.2 and python3 eval_harness.py and paste both outputs, with a line saying whether verify.py or eval_harness.py was edited.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (257 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
