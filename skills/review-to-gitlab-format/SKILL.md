---
name: review-to-gitlab-format
description: Converts a code review into paste-ready GitLab MR comments with file:line, severity, a plain-English TLDR and collapsible details. Use when asked to format a review for GitLab.
---

Convert a code review into the specialized format described below. There are two cases:

- **No branch name provided:** format the latest code review in the chat.
- **Branch name provided:** first review the branch with the project's code review skill or, if there is none, review the diff yourself. Don't switch to the branch. Review the diff against the main/master branch. Then format that review.

First of all, check whether there's already a code review file for this branch in `~/.gitlab-review-notes/` (see the file naming at the end). Match any file ending in `-myapp-branchname.md`, regardless of the date and time. If there is and it's not empty, ask the user whether they want to format the MR again.

The file has three parts: properties, an index table and one section per review issue.

1. Properties at the very top of the file (Obsidian frontmatter): `project` (the project name as on the GitLab remote), `branch`, `target` (main or master), `reviewed` (date and time), `issues` (the total number of issues), `high`, `medium` and `low` (the number of issues of each level) and `tags: [code-review]`. The counts must match the issues below.
2. An index table with one row per issue: the number of the issue (linked to its heading), the level, the file name with lines and a TLDR of a few words.
3. For each issue, a section with:
   - The heading `## number · level · file:line[-line]`, with file:line[-line] in backticks. Write the exact file:line[-line] where the comment should be left. This is needed to leave the comment in the correct place in the GitLab MR.
   - `- [ ] Posted` on the next line. The user ticks it after posting the comment.
   - The comment itself inside a ````markdown block (four backticks, so the code blocks inside it still work). The user copies it with Obsidian's copy button and pastes it into GitLab as is. Inside the block:
     1. Write the text "`AI Review` (level)" (without the quotes), where level is one of High, Medium or Low.
     2. Add two newlines. Write a human-friendly TLDR in the following format: "This and this should be changed. With the current implementation, if this and that happens, it will do this and that", or something similar. Use simplified English.
     3. Add two newlines. Then write the following:

        ```
        <details>
        <summary>More details from AI review</summary>

        A description that provides more technical data and the full technical explanation of the problem, including code references. Don't bloat it. It should be exhaustive yet concise.

        </details>
        ```

        where "More details from AI review" is written literally.

Example of the complete file for a review with two issues:

`````markdown
---
project: myapp
branch: feat/invoice-export
target: main
reviewed: 2026-01-02 14:30
issues: 2
high: 1
medium: 1
low: 0
tags: [code-review]
---

| # | Level | Location | TLDR |
|---|---|---|---|
| [[#1 · High · `src/invoices/export.controller.ts:42-47`\|1]] | High | `export.controller.ts:42-47` | Any user can download any invoice |
| [[#2 · Medium · `src/invoices/format-amount.ts:12`\|2]] | Medium | `format-amount.ts:12` | 1.005 rounds down to 1.00 |

## 1 · High · `src/invoices/export.controller.ts:42-47`

- [ ] Posted

````markdown
`AI Review` (High)

The owner of the invoice should be checked. With the current implementation, if a user changes the invoice id in the export URL, they can download any other customer's invoice.

<details>
<summary>More details from AI review</summary>

The new endpoint loads the invoice by id only:

```ts
const invoice = await this.invoices.findById(params.id);
return this.pdf.render(invoice);
```

Every other invoice endpoint goes through `findForUser(id, user.id)`, which returns 404 when the invoice belongs to someone else. This one skips it, and the ids are sequential, so they are easy to guess.

Fix: use `findForUser(params.id, req.user.id)` and add a test that requests another user's invoice and expects 404.

</details>
````

## 2 · Medium · `src/invoices/format-amount.ts:12`

- [ ] Posted

````markdown
`AI Review` (Medium)

The rounding should be done in cents. With the current implementation, if an amount is like 1.005, the PDF shows 1.00 instead of 1.01, so the export total can differ from the total on the invoice page.

<details>
<summary>More details from AI review</summary>

```ts
return amount.toFixed(2);
```

`toFixed` works on the binary float, and 1.005 is stored as 1.00499999…, so it rounds down. The invoice page uses `formatMoney(amountInCents)`, which rounds correctly.

Fix: reuse `formatMoney` here instead of formatting the float directly.

</details>
````
`````

---

After creating the text in the specified format, save the output to a text file in `~/.gitlab-review-notes/` (not in the project). Create the folder if it doesn't exist. Name the file gitlab-review-20260102-1430-myapp-branchname.md,
where the date (YYYYMMDD), the time (HHMM, 24-hour), myapp (the project name as on the GitLab remote) and the branch name are dynamic. Replace / with - in the branch name.

Don't show the formatted review in the chat. Only reply with short stats: the number of issues and how many there are of each level (High, Medium, Low).
