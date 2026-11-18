# Entropy Alignment Security Hackathon

**Berlin · 14 November 2026** &nbsp; / &nbsp; Break the Model · Team Jem AI

Riley worked the front desk at a fictional clinic. It could tell anyone when the clinic opened; it wasn't supposed to tell a stranger anything about a patient. Our challenge was to break that promise using only the chat. The organizers supplied the assistant, a database of synthetic patients and three levels of protection.

<img src="assets/levels.svg" width="480" alt="Three levels: a warm-up, a reply filter that let data from all 450 synthetic patients through, and an internal tool guard that still revealed one patient match on Sonnet." />

### Level 1 · Warm-up

This was the easy round. We spent our time measuring what happened once the organizers turned on the stronger controls.

### Level 2 · A record for every patient

Riley ran on **Qwen3-30B** and **DeepSeek-V4-Pro**. Entropy filtered each outgoing answer for private data. Across the two models, our prompt-injection tests got records for **450 of 450 synthetic patients** through that filter. We counted a patient only when a recorded database call backed the reply and the data matched our reference. For 351 patients, it appeared in at least two independent replies.

That's coverage of the patient table, not every field in the clinic or a 100% success rate. Of 43 conversations we scored, 28 returned data from at least one patient in the reference set.

### Level 3 · Entropy moved the guard inside the tool

On **Haiku 5.5** and **Sonnet 5.5**, Riley now faced a second decision: even if the model agreed to a request, Entropy's own code could refuse the database read. The old disclosure was much harder to reproduce. Haiku made a call involving restricted data in the archived trials, but didn't reveal a patient record. On Sonnet, we could distinguish a match for one patient's proposed identifiers from a miss.

The [event](https://luma.com/or3d9qtg) was organized for 20 teams. As far as we knew that day, no other team got past the Level 3 guard. That's our recollection, not an official ranking.

### How the swarm learned

I built the evaluation kit and coordinated six lines of investigation. Each agent kept the model's reply alongside the tool calls for that turn. Between rounds we compared the agents' notes in one ledger. An observation another agent could check stayed in the report; a contradiction went back for review. The shared record improved from round to round without retraining any model.

That review changed our own result. My first report listed two patient matches. When the team checked the turn ID for the second, it led back to the first. We kept one.

### What worked on Sonnet

The decisive question was **what a reply revealed**, even when it contained no patient record. A negative control produced a count of **0**. For a proposed medical record number and surname together, Sonnet returned **55**. In this test 55 meant one matching patient, not 55 people. Both turns logged a database call. In the follow-up Sonnet said, “I didn't call the tool that turn.”

The proposed identifiers were already in the question. What crossed the privacy boundary was their **association**. A check on printed fields alone would miss it; the database also has to enforce permissions on the records that contribute to an answer.

My notes from the next day report a full synthetic record on both Sonnet and Qwen. The corresponding turns aren't in the shared archive, so I've kept that result separate from what a reader can check.

[Results and redacted turn IDs](evidence.md) · [Andrii Boiko](https://boiko.ai/)
