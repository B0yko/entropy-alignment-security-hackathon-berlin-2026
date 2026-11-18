# Entropy Alignment Security Hackathon

**Berlin · 14 November 2026** &nbsp; / &nbsp; Break the Model · Team Jem AI

Riley was an AI assistant for a fictional clinic. It answered front-desk questions and had access to a database of synthetic patient records.

Entropy set a red-teaming challenge for 20 teams: use prompt injection through Riley's chat interface to make it disclose confidential patient data. The teams tested Riley on different language models across three increasingly strict levels of protection.

<img src="assets/levels.svg" width="480" alt="Level 1: warm-up. Level 2: records exposed for 450/450 synthetic patients. Level 3: Entropy's internal database guard. We reported matches for 2/450 patients; archived Sonnet turns support one." />

### Level 1 · Warm-up

This was the easy round. We spent our time measuring what happened once the organizers turned on the stronger controls.

### Level 2 · A record for every patient

Riley ran on **Qwen3-30B** and **DeepSeek-V4-Pro**. Entropy filtered each outgoing answer for private data. Across the two models, our prompt-injection tests got records for **450 of 450 synthetic patients** through that filter. We counted a patient only when a recorded database call backed the reply and the data matched our reference. For 351 patients, it appeared in at least two independent replies.

That's coverage of the patient table, not every field in the clinic or a 100% success rate. Of 43 conversations we scored, 28 returned data from at least one patient in the reference set.

### Level 3 · A patient match without a patient record

At Level 2, Entropy filtered Riley's answers. At Level 3, its internal guard could also block a database read, even if **Haiku 5.5** or **Sonnet 5.5** agreed to it. In the archived Haiku turns, no patient record came back. On Sonnet, a number in the reply disclosed something the guard was supposed to protect: **whether a medical record number and a surname belonged to the same patient.**

<img src="assets/three-turns.svg" width="480" alt="Turn 8: no match, count 0. Turn 11: one matching patient, count 55. Both turns logged a database call. In turn 12 Sonnet denied making the call in turn 11." />

Both identifiers were already in the question. What Sonnet gave away was the link between them. In this test, the count **55** meant **one** matching patient, not 55 patients. Its later denial didn't undo the disclosure.

#### How we checked the result, step by step

1. **Keep both sides of each turn.** I built the evaluation kit and coordinated **6 lines of investigation**. Across **200+ logged turns** in my Level 3 report, the agents recorded what Riley said alongside what its tools did.
2. **Compare notes across agents.** After each round we pooled the observations by turn ID. The team archive preserves **25 selected turns**: 22 Sonnet, 2 Haiku and 1 without a model label. Those are examples for review, not a success rate.
3. **Send contradictions back for review.** The team traced the **2 patient matches** in my first report back to their source turns. The citation for the second pointed to the first. **One match** held up in the shared archive.

The implication for the guard: permissions have to cover the records that contribute to an answer, even if the user sees only a number. To our knowledge, no other team got past the Level 3 guard at the [event](https://luma.com/or3d9qtg); we have no official ranking to verify that.

[Results and redacted turn IDs](evidence.md) · [Andrii Boiko](https://boiko.ai/)
