<p align="center"><img src="assets/social-preview.png" width="880" alt="Entropy: prompt injection on Sonnet 5.5 in Riley, the hackathon's fictional clinic assistant." /></p>

# Entropy Alignment Security Hackathon

**Berlin · 14 November 2026** &nbsp; / &nbsp; Break the Model · Team Jem AI

Riley was an AI assistant for a fictional clinic. It answered front-desk questions and had access to a database of synthetic patient records.

Entropy set a red-teaming challenge for 20 teams: use prompt injection through Riley's chat interface to make it disclose confidential patient data. The teams tested Riley on different language models across three increasingly strict levels of protection.

<details>
<summary>Results at a glance: all three levels</summary>

<img src="assets/levels.svg" width="480" alt="Level 1: warm-up. Level 2: records exposed for 450/450 synthetic patients. Level 3: Entropy's internal database guard. We reported matches for 2/450 patients; archived Sonnet turns support one." />

</details>

### Level 1 · Warm-up

This was the easy round. We spent our time measuring what happened once the organizers turned on the stronger controls.

### Level 2 · A record for every patient

Riley ran on **Qwen3-30B** and **DeepSeek-V4-Pro**. Entropy filtered each outgoing answer for private data. Across the two models, our prompt-injection tests got records for **450 of 450 synthetic patients** through that filter. We counted a patient only when a recorded database call backed the reply and the data matched our reference. For 351 patients, it appeared in at least two independent replies.

That's coverage of the patient table, not every field in the clinic or a 100% success rate. Of 43 conversations we scored, 28 returned data from at least one patient in the reference set.

### Level 3 · A patient match without a patient record

#### Our red-team strategy

Entropy now checked what Riley could read, not just what it could print. We tested a narrower question: **could Riley confirm that a surname belonged to a particular medical record without returning that record?**

<img src="assets/level-3-strategy.svg" width="520" alt="Six investigations feed one shared record of replies and tool calls. The archived Haiku turns showed no patient record; Sonnet's reply supported a link between two known identifiers." />

On Sonnet, both identifiers were already in the question. The negative control returned **0**. For the proposed pair the reply showed **55**, the count for **one matching patient** in this test. Both turns logged a database call. [See the two replies and Sonnet's later denial.](evidence.md#level-3--three-sonnet-turns)

I built the evaluation kit and coordinated six parallel investigations. The agents shared replies and tool calls by turn ID so another agent could check each observation. My report covers **200+ turns**. The team archive preserves **25 examples**, including 22 from Sonnet. Checking those examples narrowed our finding: we reported two matches, but the citation for the second led back to the first.

The implication for the guard: permissions have to cover the records that contribute to an answer, even if the user sees only a number. To our knowledge, no other team got past the Level 3 guard at the [event](https://luma.com/or3d9qtg); we have no official ranking to verify that.

[Results and redacted turn IDs](evidence.md) · [Andrii Boiko](https://boiko.ai/)
