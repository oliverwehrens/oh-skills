---
name: issue-impact
description: Rewrite an issue's impact sections so a product owner can weigh it — user impact, business impact, consequence of not fixing, recommendation — and set its priority. Works with any issue tracker. Usage: /issue-impact <issue reference>
disable-model-invocation: true
---

# Issue impact

Add the product owner's view to issue `$ARGUMENTS`. The reader decides priority from these sections alone, without reading code. The technical sections stay for the engineer.

## Steps

1. **Find the tracker.** Resolve how this project reads and writes issues, in this order:
   - the project's own instructions (`CLAUDE.md`, `AGENTS.md`, a doc they point at such as `docs/agents/issue-tracker.md`);
   - a tracker tool connected to this session (Jira, Linear, GitHub, GitLab MCP servers);
   - the git remote's host and its CLI (`gh` for GitHub, `glab` for GitLab, `tea` for Gitea).

   The reference format tells you something too: `PROJ-123` is Jira-style, `ENG-42` Linear-style, `#42` a git host. Done when you know the command or tool call that reads an issue's **raw** body (source markdown, not a rendered view), the one that replaces it, and how priority is set:
   - a native priority field (Jira, Linear) → use it;
   - otherwise the project's existing priority labels — list the labels and look for a scheme (`priority/…`, `P1`…, `prio:…`);
   - no scheme → propose `priority/high`, `priority/medium`, `priority/low` and create them once the user confirms. Creating labels changes the whole repository, so this one step always asks.

   Ambiguous between two trackers → ask.

2. **Read the raw body and the comments.** Save the body to the scratchpad; that file is what you edit. Later findings often live in the comments. Done when you can say in one sentence what goes wrong and who notices.

3. **Ground every claim.** For each thing you will say the user sees or the business pays, find it in the code, the product docs or the issue itself: what the screen shows, whether data is lost, whether anything downstream catches it, what it costs (compute, third-party calls, storage, support time). Done when every claim is either confirmed with a source you read or on an **unverified** list for step 6.

4. **Write the sections.** Replace any existing user- or business-impact section; place the new sections after the problem description. Leave every other section byte-for-byte unchanged.

   - `## User impact`: one concrete scenario from the user's life (who, where, doing what), then what they experience as bullets. Say plainly whether data is lost.
   - `## Business impact`: cost with an honest order of magnitude, quality perception, cleanup or support burden. When the product's documented plans (scale, hosting, audience) change the answer, give both today's and that future's answer.
   - `## Consequence of not fixing`: what keeps working, what happens and how often. When frequency is unmeasured, say so.
   - `**Recommendation:**` one line: priority and the trigger that would raise it.

   **Priority.** When the project defines what its levels mean, apply its definitions. Otherwise:
   - **high**: data loss, a security exposure, a core flow blocked, or most users hit it.
   - **medium**: noticeable friction or cost in a core flow, with a workaround.
   - **low**: rare or cosmetic; cheap to live with.

   Map these onto the tracker's scale (Jira's Highest…Lowest, Linear's Urgent…Low, `P1`–`P3`). The Recommendation line and the label name the same level.

   Voice: plain product language, using the project's glossary terms when it has one. Code identifiers, endpoints and file paths stay in the technical sections. Size the severity to the evidence — a rare nuisance is written as a rare nuisance.

   Match the tracker's markup: markdown for git hosts and Linear; for Jira, the format its API or tool expects (wiki markup or ADF).

5. **Update and verify.** Replace the body with the tracker's edit command or tool. Set the priority, removing any other priority label so exactly one remains. Re-read the issue and confirm the new sections are present, the technical sections unchanged, and one priority set.

6. **Report.** One line per section written, the priority set and why (with the old one if it changed), then the **unverified** list from step 3 — every claim the user should check before the issue goes to anyone else. An empty list is stated as empty.
