---
tags: [prompt-engineering, fae, templates]
created: 2026-11-02
---

# Prompt Library: FAE work (copy, fill the `{{ }}`, send)

> [!tip] The pattern behind every template
> **Role → Context → Task → Constraints → Output format → Evidence rule.**
> Most weak prompts skip *Context* (device, firmware, network) and *Output format*. These templates force both.

## 0. Universal evidence footer (paste at the end of any technical prompt)
```
Rules: Label each claim as Verified (cite the source), Assumption, or Unknown.
Do not invent SDK methods, API fields, ports or specs. If you don't know, say Unknown and tell me where to check.
Output: Markdown that works in Obsidian. Vietnamese prose is fine; keep English technical terms.
```

## 1. Field troubleshooting (on site, under time pressure)
```
I'm on site at {{customer/store}}. Device: {{model}}, firmware {{fw}}, app {{app+version}}.
Network: {{wired/wifi, VLAN, DHCP/static, IP}}.
Symptom: {{exact symptom + since when + how often}}.
Already tried: {{list}}.
Give me: (1) the top 3 likely causes ranked by probability, each with a 2-minute check;
(2) the order to run the checks so the cheapest goes first; (3) what evidence to capture for escalation.
{{evidence footer}}
```

## 2. SDK / integration question (stops made-up API answers)
```
Context: integrating {{device}} with {{WMS/ERP name}} through {{SDK name + version / API}}.
Attached/pasted: {{relevant SDK doc excerpt or demo code}}.
Question: {{one specific question}}.
Answer ONLY from the material I pasted. If it isn't covered, say "Not in provided docs" and suggest what to test on the bench.
{{evidence footer}}
```
**Why it works:** grounding the model in pasted docs is the most effective anti-hallucination step for vendor SDKs.

## 3. Field survey → deployment report
```
Turn these raw survey notes into a deployment report for {{audience: customer IT / sales / manager}}.
Notes: {{paste}}
Sections: Site summary · Network findings · RF/environment risks · Bill of materials · Open questions · Next actions (owner + date).
Flag anything in my notes that is contradictory or missing.
```

## 4. Training material from a procedure
```
Convert this procedure into training material for {{store staff / partner technicians}}, skill level {{low/medium}}.
Procedure: {{paste}}
Output: (1) a 1-page quick guide with numbered steps; (2) 5 common mistakes and how to spot each;
(3) a 5-question check quiz with answers. Plain words, short sentences.
```

## 5. Customer email (English, with your second-language polish)
```
Draft a {{tone: firm/polite/apologetic}} email to {{role}} at {{customer}} about {{issue}}.
Facts: {{bullets}}. What I need from them: {{ask + deadline}}.
Keep it under 150 words. Then list any phrasing in your draft a native speaker might find unclear.
```
Bonus: it doubles as English practice, one of your stated gaps.

## 6. Spec review (before you build anything)
```
Review this spec as a skeptical senior engineer. Spec: {{paste or link}}.
Find: (1) unstated assumptions; (2) missing failure modes; (3) the single riskiest unknown and the cheapest experiment to resolve it.
Do not rewrite the spec. Give a ranked list only.
```
Try it now on [[02-capstone-spec-ura4-store-monitor]].

## 7. Meta-prompt: improve my prompt
```
Here is a prompt I'm about to send: {{paste}}.
Before answering it, list the 3 details missing from it that would most change your answer. Then answer as best you can, stating your assumptions.
```

---

## Practice drill (10 min/day, goal #1)
1. Pick one real ticket from today.
2. Write the prompt with the template **from memory**.
3. Compare it with the template and note which part you skipped.
4. Log the miss below. After 2 weeks you will see your personal blind spot.

| Date | Template | Part I skipped |
|---|---|---|
| | | |
