# Assessing BNY's current controls (task 4)

Krish - November 2026 - draft

## What this is

This is my part of the project. I looked at the controls BNY already has for insider threats and checked how well they'd hold up against our scenarios. The four controls from the meeting notes are DLP, UEBA, PAM, and whistleblower/reporting channels.

Our focus is agent insiders (Eliza digital employees), so for each control I ask what it would mean for an agent first. Then I rate it for human insiders so we have something to compare against.

The scenario numbers (T1 to T17) are the same ones used in `quantitative/attack_scenarios_and_sols/attack_scenarios_and_sols.md`. The attack-scenario report draft numbers some scenarios differently. Matched by content, its Scenarios 1, 2, 3, 4, 5, 6, 7, 8, 12 and 13 are T2, T3, T5, T7, T9, T15, T4, T8, T11 and T12 here (its T10 and T14 keep their numbers).

## The short version

- BNY's controls look decent for people and unproven for agents.
- That's mostly because BNY doesn't publish how DLP, monitoring, and access controls work on digital employees. A lot of my agent ratings are educated guesses, and I mark them that way below.
- Out of 17 scenarios, only 4 have a control that is clearly the main defense (T3, T4, T5, T17). In T4, T5, and T17 it works because the control can see the human behind the agent.
- In the other 13 (T1, T2, and T6 to T16) the agent itself is the one acting, and none of the four controls is a main defense.
- Approval fatigue (T12) doesn't have an owner in any of the four controls.

| Control | Agent insider | Human insider (baseline) | Evidence | Confidence |
|---|---|---|---|---|
| DLP | Low to Medium | Medium | DLP is confirmed, including for Eliza. How it works is not | Low |
| UEBA / behavior monitoring | Low | Medium | Monitoring and an insider threat program are confirmed. UEBA is never named | Low |
| PAM / IAM | Low to Medium | Medium to High | IAM and agent logins are confirmed. PAM is my inference from regulation | Medium |
| Whistleblower / reporting | Low (untested) | High | Confirmed | Medium to High |

## How I rated things

- **Evidence:** "Confirmed" means BNY says it publicly. "Partial" means BNY describes something related and I'm guessing at the detail. "Not found" means I couldn't find a public statement. That doesn't mean the control doesn't exist, only that I couldn't confirm it.
- **Ratings:** Low, Medium, or High. I looked at whether the control prevents, detects, or helps respond, whether it covers our scenarios, and whether it leaves evidence that can be traced back to a person or an agent. These are my judgment calls, not measurements.
- **Benchmarks:** NYDFS Part 500 (sections 500.7 and 500.14), the CISA Insider Threat Mitigation Guide, and NIST's recent work on AI agent identity. The NIST work is still a draft, so falling short of it is a maturity gap and not a compliance failure.
- **Assumption:** BNY is very likely a "Class A" company under Part 500, which is the tier for large firms. We still need to confirm which BNY entities are licensed by NYDFS.

## What BNY says about its program overall

- Its information security management system is ISO 27001:2022 certified, and the program lines up with the NIST Cybersecurity Framework.
- Everyone has to do cybersecurity awareness training, and phishing tests run at least quarterly.
- There is an enterprise-wide insider threat program for direction, governance, and awareness. It treats misuse of technology as either intentional or unintentional.
- Identity and access management covers provisioning, approval, periodic review, and removal, based on least privilege and separation of duties.
- Monitoring and logging are meant to find suspicious activity and send unusual activity to incident response.
- Information is sorted into four levels: Highly Confidential, Confidential, Internal Use Only, and Public.

None of the program documents I found name specific tools like a DLP platform, a UEBA product, or a PAM vault. That's why some ratings have low confidence.

## 1. Data loss prevention (DLP)

DLP software watches data leaving the company and blocks it when it looks like something that shouldn't go, like SSNs or account numbers.

**What BNY says**
- The cybersecurity overview says its PCI-DSS program has preventive and detective processes for data leakage or loss.
- Sensitive information sent to outside recipients has to be encrypted, using TLS or the Secure Messaging Portal.
- A BNY executive told American Banker that Eliza has built-in safeguards, including data loss prevention, along with identity and access management, vulnerability management, and third-party governance.
- Employees have to pass a test on responsible AI use and data use before they get Eliza access.

**For agents.** The question is whether DLP can see what an agent sends out through its tools, memory, and shared databases. BNY doesn't say. DLP is usually built to catch known patterns on known channels like email, uploads, and USB drives. Agents can leave through other routes, like links with data in the URL, memory saved on an outside service, or shared retrieval indexes. They also reword data, so a summary of client records might not match any pattern the rules look for. This is my reasoning and not something BNY has said. It matters for T2, T9, T13, and T15.

**For people.** It should work well for normal mistakes on normal channels, like T3 (pasting SSNs into an AI tool) and T17 (shadow AI).

**Rating:** agent Low to Medium, human Medium, confidence Low.

## 2. UEBA and behavior monitoring

UEBA tools learn what normal looks like for each person or system and flag anything unusual, like someone downloading thousands of files at 3am.

**What BNY says**
- Monitoring and logging exist to detect suspicious or harmful activity, and unusual activity goes to the incident response team.
- The insider threat program is governed by a written policy (I.N.500). It's mentioned in BNY's SEC filings, but the full text isn't public.
- The related personal trading policy defines internal fraud by an insider to include unauthorized trading, market manipulation, and fraudulent funds transfers.
- Personal trading is monitored through the Star Compliance system.
- No public BNY document names UEBA or any behavior analytics product.

**Benchmarks.** NYDFS 500.14(b) requires Class A companies to use endpoint detection and response to monitor unusual activity, plus centralized logging and alerting. CISA's guide describes a team that includes HR and IT. NIST is asking how an agent's actions can be tied back to the agent.

**For agents.**
- There's no known baseline. Agents run around the clock and in parallel, so it's unclear what normal looks like for one.
- Logs might only show the agent's ID, so investigators can't tell which prompt, tool, model version, or approval caused something (T1, T4, T11).
- Attacks that look like routine work, such as poisoned patches pushed by trusted bots (T8), are hard to spot.
- Approval fatigue (T12) leaves no anomaly because the approvals are real.
- I'm guessing that Compliance monitoring (like Star) and Cybersecurity monitoring are separate and not connected in real time. That's a common setup, but I found no public statement either way, so I'm treating it as a hypothesis.

**For people.** It should work for unusual human behavior, like a leaver downloading a lot of data before they go (T5).

**Rating:** agent Low, human Medium, confidence Low.

## 3. Privileged access management (PAM) and IAM

PAM protects the accounts that have the most power, like the ones that can change payment records. Passwords go in a vault, access is handed out for a specific job and a limited time, and sessions are recorded.

**What BNY says**
- IAM follows least privilege and separation of duties, with periodic access reviews and removal of access.
- The ISO 27001 scope includes user access management and remote access management.
- Digital employees have their own logins, human managers, and email addresses (Axios, October 2025). BNY's OpenAI case study says they have identities, access controls, and dedicated workflows.
- No public document names a PAM product or says how long agent credentials last.

**Benchmarks.** NYDFS 500.7 requires limiting access to what each role needs and removing it promptly when someone leaves or changes roles. Class A companies also have to monitor privileged access and use a PAM solution, or an approved equivalent. Since BNY is very likely Class A, I think a PAM capability is a fair assumption even though BNY doesn't name one. One summary of the rule says privileged accounts include service accounts, which would put agent identities in scope, but I haven't checked that against the rule text.

**For agents.** These are open questions, not confirmed failures.
- Do leaver and role-change processes cover agents? What happens to an agent when its owner leaves (T5)?
- Do agents cloned from a template pick up extra permissions over time?
- Are agent credentials short-lived and tied to one task, or are they static?
- How is authority tracked when one agent asks another to act (T11, T14)?
- Does a manager signing off on an agent still work at high volume, or does it turn into rubber-stamping (T12)?
- If the model behind an agent is swapped, behavior can change while the identity stays the same.

**For people.** This is strong on paper: vaulted credentials, time-limited access, session recording, and removal of access when someone leaves (T4, T5).

**Rating:** agent Low to Medium, human Medium to High, confidence Medium.

## 4. Whistleblower and reporting channels

This is how employees report wrongdoing safely, anonymously if they want. It matters because coworkers often notice problems before software does.

**What BNY says**
- The Ethics Help Line is run by BNY's Ethics Office. It's confidential, and anonymous contact is possible.
- The Ethics Hot Line is run by an independent provider (EthicsPoint) by phone or web. Reporters can follow up with a report key and password.
- Employees who report in good faith are protected from retaliation, and the Code of Conduct has a zero-tolerance retaliation section.
- Employees can also contact a non-management director, including by anonymous mail.
- BNY has whistleblowing, speak-up, and non-retaliation policies, and there is a separate whistleblower standard for Australia.
- The hotline FAQ says concerns are reviewed, investigations happen where appropriate, the reporter is told when it closes, and retaliation is monitored.

**For agents.** The hotline is described as being for unethical or illegal conduct or reputation risk. Nothing says an employee should report an agent that's acting strangely, a near miss, or their own mistake made through an agent. Someone who's rubber-stamping approvals probably won't report themselves. Agent owners are best placed to notice an agent drifting, and they may be the ones who'd rather stay quiet.

**For people.** It works for things coworkers can see, like collusion, bypassing controls, or sending company data to a personal tool (T4, T8, T17). Insider threat guidance says coworkers often see warning signs and don't report them, so a trusted channel matters. I got that from secondary summaries of the ODNI/NITTF guide.

**Rating:** agent Low (untested), human High, confidence Medium to High for the human channel and Low for the agent case.

## How BNY compares to NIST's agent identity areas


| Area | What BNY says publicly |
|---|---|
| Identification | Digital employees have their own logins and human managers |
| Authentication | Nothing found |
| Authorization | Least privilege for people. Eliza has access controls (OpenAI case study) |
| Delegation | Nothing found |
| Logging and attribution | General monitoring only |
| Prompt injection controls | Eliza has built-in safeguards (American Banker). Nothing on injection testing |

I could confirm part of three areas and found nothing on the other three. That's a gap in public evidence, not proof that controls are missing.

## How the controls hold up against each scenario

This is my own judgment from the evidence above. "Good" means the control is probably the main defense. "Partial" means it helps a bit or only under some conditions. "None" means little or no help. DLP's "Partial" in agent scenarios only holds if DLP actually watches what agents send out, which I haven't confirmed.

| # | Scenario | DLP | UEBA | PAM / IAM | Reporting |
|---|---|---|---|---|---|
| T1 | Prompt injection in the payment agent | None | Partial | Partial | None |
| T2 | Hidden instructions in an uploaded file | Partial | Partial | None | None |
| T3 | Employee pastes SSNs into an AI tool | Good | Partial | None | None |
| T4 | Rogue admin or backdoor in an agent | None | Partial | Good | Partial |
| T5 | Fired employee still has access | Partial | Good | Good | None |
| T6 | Context window overflow | None | Partial | None | None |
| T7 | Fake or hallucinated link | None | Partial | None | None |
| T8 | Poisoned patches from bot engineers | None | None | Partial | Partial |
| T9 | RAG data leaking between agents | Partial | Partial | Partial | None |
| T10 | Agents looping and overloading systems | None | Partial | None | None |
| T11 | Agent emails an exec as the employee | None | Partial | Partial | None |
| T12 | Approval fatigue and rubber-stamping | None | None | None | None |
| T13 | Malicious tool in the tool repository | Partial | Partial | Partial | Partial |
| T14 | IT agent breaks access for teams | None | Partial | Partial | None |
| T15 | Data saved to an outside website | Partial | Partial | None | None |
| T16 | API spend and quota blowup | None | Partial | None | None |
| T17 | Shadow AI (personal chatbot) | Good | Partial | None | Partial |

Notes on the table:
- **UEBA "Partial" in agent scenarios** means it could alert after the fact if agents had their own behavior baseline. Nothing public says they do.
- **T8 and T12 are "None" for UEBA.** Poisoned patches pushed by trusted bots look like routine work (T8), and approvals that were really clicked leave no anomaly (T12). That makes T12 "None" across all four controls, matching the summary above.
- **T16** (runaway multi-agent polling and cost) is written up in `attack_scenarios_and_sols.md`. Its main defenses there are spend budgets, circuit breakers, and cycle detection on agent-to-agent messages, which none of the four controls covers directly. UEBA's "Partial" only assumes agent baselines would flag the spend spike.

## How each control could be extended to agents

BNY doesn't publish how its controls work on agents, so here is what each one would have to do. These are ideas, not statements about what BNY does today.

**DLP**
- Inspect what agents send out through tool calls and links, and block destinations that aren't on an approved list (T2, T13, T15).
- Redact SSNs before they reach the model, and scan agent logs for them (T3).
- Keep data classification labels on anything an agent retrieves (T9).

**UEBA**
- Set a baseline for each agent: release rate, destinations, data volume, how many agents it calls. Alert when it drifts (T1, T10, T16).
- Alert when many agents suggest the same unusual dependency (T8).
- Track approval speed and "approve all" habits to catch rubber-stamping (T12).
- Raise a person's risk score when they resign, and watch the agents they own (T5).

**PAM / IAM**
- Give every agent its own identity with short-lived credentials limited to one task, and tie it to a named owner (T4, T5, T11).
- Let agents propose payments but not finish them. Keep a second approver (T1).
- Enforce limits in code outside the model, so a forgotten instruction doesn't matter (T6).
- Switch off a person's agents, tokens, and scheduled jobs the same day they leave (T5).
- Check the end user's permissions when an agent retrieves data, not the agent's own account (T9).

**Reporting**
- Add a clear way to report odd agent behavior and near misses.
- Make reporting blameless, so people can say "I'm approving too much" (T12).

## Gaps I found

1. **The controls were built for people.** Passwords, roles, behavior baselines, and hotlines all assume someone at a keyboard. Agents can run in parallel, be cloned, and keep one identity across model changes.
2. **It's hard to trace what an agent did.** An investigator needs to connect an action to a prompt, a tool, a model version, and an approver. NIST is asking the same question and the industry hasn't settled it.
3. **A valid login doesn't mean a safe agent.** A hijacked or prompt-injected agent still signs in correctly, so identity controls alone can't stop it.
4. **Nobody owns approval fatigue.** None of the four controls checks whether human approvals are meaningful.

## What I'd recommend, in order

This is my proposed ordering based on the gaps above. It's a judgment call about which fixes cover the most scenarios and close the biggest gaps, not a cost-benefit result. Edit it with the team.

1. **Make every agent traceable and revocable.** Give each agent its own identity with short-lived, task-limited credentials and a named human owner. Log the prompt, tool call, model version, and approver, not just the agent ID. Start with the cheapest step: switch off a person's agents, tokens, and scheduled jobs the same day they leave. This closes gaps 1 to 3 and covers T1, T4, T5, T11 and T14.
2. **Make DLP see what agents send out.** Inspect tool calls and links, allow only approved destinations, redact sensitive data before it reaches the model, and keep classification labels on anything an agent retrieves or stores. This covers T2, T3, T9, T13 and T15, which is where DLP's agent coverage is most uncertain today.
3. **Make approvals meaningful.** Agents propose, but payments and irreversible actions need a limit or second approver enforced in code outside the model. Track approval speed and "approve all" habits, and add a blameless way to report "I'm approving too much." This closes gap 4, which none of the four controls owns, and covers T1, T6, T10, T12 and T16.

Why this order: the first item is the foundation. Without traceable identities and logs, nobody can tell who or what approved or sent something, so the DLP and approval fixes in items 2 and 3 are harder to audit or investigate.

## What this means for the risk model

The team's numbers moved on Nov 22. The v2 likelihood note (unchanged) still has the human-insider baselines. The new agentic update (`monte_carlo_agentic_update.md` and `agentic_insider_risk.md`) raises them for agentic paths. My ratings are ordinal (Low, Medium, High), so they can check direction but can't confirm any probability.

- **Malicious insider:** v2 assumes 5% to 10% vulnerability (10% point estimate), "limited by dual-control & PAM." The agentic update raises it to 12%. The reason given is that agents trust forwarded instructions without re-checking where they came from. My ratings point the same way: PAM/IAM is Medium to High for people but only Low to Medium for agents, and nothing public says how agent credentials are scoped or how long they last. They can't tell us whether 12% is the right number.
- **Negligent insider:** v2 assumes about 3% (range 2% to 4%), "filtered by automated email/cloud guards." The agentic update raises it to 4%, citing over-permissioned agents and rubber-stamped approvals. That is the gap I found in T12, where none of the four controls checks whether approvals are real. But I rate DLP only Medium with Low confidence, and BNY doesn't say how it works, so I can't confirm the 3% or the 4%.
- **Ranges differ between the team's files.** The agentic Monte Carlo spec uses 6% to 20% (malicious) and 2% to 7% (negligent). `agentic_insider_risk.md` uses 10% to 15% and 3% to 5%. The active simulation doesn't sample vulnerability at all. It uses fixed event rates of 0.12 per year (malicious) and 8.0 per year (negligent), so the team should decide which ranges go in the report.
- **What the simulation does and doesn't cover.** It models a human poisoning an agent or over-permissioning one, leading to downstream agent failures. It has no human-only comparison, and the team says that is out of scope. So these ratings are best used as a qualitative cross-check on the elevated assumptions, not as an input.

## Still to check with BNY

1. Does DLP inspect what agents send out through tool calls, or only human channels?
2. Do data classification labels follow data into retrieval databases and agent memory?
3. Are agent outputs scanned before they leave the platform?
4. Is agent behavior baselined separately from human behavior?
5. Do agent logs record the prompt, tool call, model version, and approver, or only the agent ID?
6. Are HR, Compliance, and security monitoring signals combined into one view of insider risk?
7. Are agent identities covered by PAM and access reviews, and how long do agent credentials last?
8. What happens to an agent when its owner leaves or changes roles?
9. Is there a defined way to report unusual agent behavior, and who investigates it?

## Limits of this write-up

- I only used public sources. BNY's internal policies, including I.N.500, aren't public, and some of the Eliza information comes from press and a vendor case study that could be promotional.
- The Code of Conduct and hotline pages I found look like older versions. Check the current text before quoting contact details.
- The NYDFS point about service accounts and the ODNI/NITTF point about unreported warning signs come from secondary summaries and should be checked against the originals.
- The ratings are judgment calls. They should change if BNY gives us real evidence.
- Provenance: the web research was done in November 2026 with AI assistance (Claude) and is not in the `sources/` folder and did not come from Gemini Notebook. Everything here is derived research, not primary-source fact.

## Sources

BNY
- BNY Cybersecurity Program Overview: https://www.bny.com/assets/corporate/documents/pdf/bny-cybersecurity-overview.pdf
- BNY Trust Center, Information Security: https://www.bny.com/corporate/global/en/about-us/trust-center/information-security.html
- BNY, Contact Ethics: https://www.bny.com/corporate/global/en/about-us/sustainability/contact-ethics.html
- EthicsPoint, How to File a Report: https://secure.ethicspoint.com/domain/media/en/gui/7423/howto.html
- BNY Australia Whistleblower Protection Standard: https://www.bny.com/corporate/au/en/australia-whistleblower-protection-policy.html
- BNY Employee Code of Conduct: https://www.bny.com/assets/corporate/documents/pdf/csr/employee-code-of-conduct.pdf
- BNY Form 10-K FY2025, Exhibit 19.1 (Personal Securities Trading Policy): https://www.sec.gov/Archives/edgar/data/1390777/000139077726000033/form10-k_ex191x4q25.htm

Eliza and digital employees
- Axios, "This Wall Street bank has over 100 digital employees" (October 17, 2025): https://www.axios.com/2025/10/17/ai-wall-street-digital-workers
- OpenAI, BNY customer story: https://openai.com/index/bny/
- American Banker, "Inside BNY's plans to upgrade Eliza": https://www.americanbanker.com/news/inside-bnys-plans-to-upgrade-eliza-its-generative-ai-platform

Regulation and standards
- 23 NYCRR 500.7 (Cornell LII): https://www.law.cornell.edu/regulations/new-york/23-NYCRR-500.7
- CISA, Insider Threat Mitigation Guide: https://cisa.gov/resources/insider-threat-mitigation-guide
- NIST NCCoE, concept paper on identity and authority of software agents (February 2026): https://www.nist.gov/news-events/news/2026/02/new-concept-paper-identity-and-authority-software-agents
