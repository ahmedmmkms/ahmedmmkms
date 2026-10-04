# How I work through a product decision

[← My profile](../README.md) · [Product cases](README.md)

A feature request is a starting point. I want to understand the work behind it: who is trying to do what, where things become difficult, and what would make a useful difference.

My engineering background comes into the conversation when we start choosing a solution. What does it depend on? How might it fail? What will the operating team have to live with? Those questions often change the scope.

## Start with something specific

A workflow is easier to reason about than a broad ambition. For the AI Operations Copilot, I would start with one kind of document and one review task. That gives us something concrete to observe and compare.

The same applies to the other concepts: one clinical handoff, one asset class, or one school pickup process.

## Put the choice on the page

I want someone reading a decision to understand three things: **why this option, what it costs us, and what could change our minds.**

A few examples from the concept cases:

- **AI Operations Copilot:** keep a reviewer between a suggestion and an action. The open question is whether the help saves more time than the review adds.
- **Clinical Workflow:** establish identity and traceable handoffs first. The open question is whether that addresses the most important problem in the actual setting.
- **Predictive Maintenance:** begin with explainable monitoring. The open question is whether available failure data supports something more predictive.
- **School Pickup:** keep release authority with staff and current guardian permissions. The open question is how to make verification practical during a busy pickup period.

These are proposed directions. They still need evidence.

## Use the numbers carefully

Prioritization scores can help when their inputs mean something. If reach or effort is largely a guess, I would rather make that uncertainty visible than let a tidy score settle the argument.

I also want a measure that could tell us we are making things worse: more review effort, more false alarms, a longer queue, or more exceptions for staff.

## Leave room to learn

The first version should help answer an important question. I would define that question, the evidence needed, and the conditions for continuing, changing direction, or stopping.

That is the part of systems thinking I find especially useful in product work: a decision is connected to what happens next, and the feedback should come back into the decision.

<details>
<summary><strong>The structure behind these notes</strong></summary>

For a deeper review, I use a [decision snapshot](templates/product-artifacts.md#decision-snapshot), an [assumption register](templates/product-artifacts.md#assumption-register), and [testable requirements](templates/product-artifacts.md#requirements-matrix).

The broader questions are customer value, business value, feasibility, strategic fit, risk, and evidence confidence. They support judgment; they are not a formula that produces the answer.

[All reusable product artifacts](templates/product-artifacts.md)

</details>
