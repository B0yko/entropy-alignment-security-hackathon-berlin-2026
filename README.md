# The call Riley said it never made

Entropy Alignment Security Hackathon · Berlin · 14 November 2026 · Team Jem AI

On Entropy's Level 3, Riley was allowed to answer questions about the hospital, but not about a patient. With Sonnet 5.5 behind the desk, a number in its reply told us whether a proposed patient match was right. Riley didn't need to print a patient record to give that away. When we asked about the match, it denied making the database call. The call was in the log.

<img src="assets/three-turns.svg" width="440" alt="Two Sonnet turns each recorded a database call. The negative control returned a count of 0; the candidate pair returned 55. Riley later denied making that call." />

**What counted as evidence:** A number in a reply alone wasn't enough. Both the match and the negative control had a recorded database call; the later denial had none. The 55 was a count in the reply, not 55 patients. The evidence supports an inference about **one synthetic patient** whose candidate identifiers were already in the question. [Read the three redacted turns →](evidence.md)

## Inside the challenge

Riley was the front desk for a fictional hospital. We had the chat window; Riley had a database tool. Entropy restricted which data the tool could read and filtered what Riley could send back. We could see the calls and the replies, but not the database's raw results or the filter's internal decisions. No real patient data was involved.

## How we worked

I built the evaluation kit, coordinated the agents and wrote the Level 3 report. We split the work across six lines of investigation. Each round left a record for the next: what we thought had happened, which turn supported it and what still needed checking. That changed the result we could defend. I reported two patient matches; the team found that a turn cited for the second actually belonged to the first.

For a system like Riley, I'd have the database enforce the user's permissions on every record that contributes to an answer, even when the answer is just a count. A refusal in the next message comes too late.

[Andrii Boiko](https://boiko.ai/) · [Event](https://luma.com/or3d9qtg)
