# Prompt Templates for Critical Question Generation

Complete set of 18 prompt templates used in the experiments reported in:

> **Critical Question Generation for Arguments: A Large-Scale Analysis of Prompting and Reranking Strategies**  

The taxonomy of prompting strategies is summarised below.
Full experimental results are reported in the paper.

| Category | Prompts | Mechanism |
|---|---|---|
| Baseline | P1, P14 | Minimal zero-shot task instruction |
| Constraint-oriented | P11, P13, P15 | Explicit quality constraints and prohibitions against generic questions; P15 adds structured vulnerability guidelines |
| Persona-based | P2, P16, P17 | Role-based behavioural framing |
| Scaffolding | P3, P7 | Structured intermediate reasoning steps before generation |
| Few-shot | P4, P5, P6 | Positive and negative output examples |
| Hybrid | P8, P9 | Combined reasoning and behavioural guidance |
| Scheme-aware | P10, P12, P18 | Walton-style scheme definitions and template questions injected into the prompt |

The placeholder `{text}` is replaced at inference time with the argumentative intervention.
Scheme-aware prompts additionally use `{scheme_details}`, `{argumentation_scheme}`, and `{scheme_examples}` placeholders filled from the CQs-Gen annotation.

---

## Prompt 1 — Baseline Zero-Shot

*Adapted from Calvo-Figueras et al. (2025).*

```
Suggest 3 critical questions that should be raised before accepting the arguments in this text:
{text}

Give one question per line. Make the questions simple, and do not give any explanation regarding why the question is relevant.
```

---

## Prompt 2 — Expert Reasoner Persona

```
You are an expert in argument evaluation and critical reasoning.

Your task is to generate 3 critical questions that should be raised before accepting the claims in the following argument:

Argument:
{text}

Definition of Critical Questions (CQs):
Critical Questions are inquiries designed to evaluate the strength and validity of an argument by uncovering and examining the assumptions underlying its premises. They help determine whether an argument is sound or fallacious by challenging its reasoning, evidence, and potential implications.

Guidelines for Generating Critical Questions:
- Directly challenge the reasoning, evidence, or assumptions in the argument.
- Focus on unintended consequences, feasibility, or alternative solutions.
- Be precise and specific — avoid vague or generic questions.
- Ensure the question is clear and relevant to the given argument.

Provide one critical question per line without any additional explanation.
```

---

## Prompt 3 — Step-by-Step Analysis (Scaffolding)

```
You are an expert in critical reasoning and argument analysis.

First, analyze the following argument step by step to identify its key weaknesses, assumptions, and potential consequences:

Argument:
{text}

After your analysis, generate exactly 3 critical questions that directly challenge the argument by:
- Questioning its underlying assumptions,
- Examining the logical flow from premises to conclusion,
- Probing the evidence and consequences.

Each question must be clear, specific, and capable of diminishing the acceptability of the argument if answered.

Provide your final output as exactly 3 critical questions, one per line, without any additional explanation.
```

---

## Prompt 4 — Few-Shot (3 Positive Examples)

```
You are an expert in argument analysis and critical reasoning.

Below is an example of an argument and high-quality critical questions:

Example Argument (CLINTON):
The central question in this election is really what kind of country we want to be and what kind of future we'll build together. Today is my granddaughter's second birthday. I think about this a lot. We have to build an economy that works for everyone, not just those at the top. We need new jobs, good jobs, with rising incomes. I want us to invest in you. I want us to invest in your future. Jobs in infrastructure, in advanced manufacturing, innovation and technology, clean, renewable energy, and small business. Most of the new jobs will come from small business. We also have to make the economy fairer. That starts with raising the national minimum wage and also guaranteeing equal pay for women's work. I also want to see more companies do profit-sharing.

High-Quality CQ Example 1:
"What is the proposed plan for making the economy fairer, beyond raising the national minimum wage and guaranteeing equal pay for women's work?"

High-Quality CQ Example 2:
"Could Clinton investing in you have consequences that we should take into account? Is it practically possible?"

High-Quality CQ Example 3:
"Could creating jobs in infrastructure, in advanced manufacturing, innovation and technology, clean, renewable energy, and small businesses have consequences that we should take into account? Is it practically possible?"

Now, given the argument below, generate exactly 3 high-quality critical questions (one per line) that challenge the argument's reasoning, assumptions, evidence, consequences, or alternatives. Write each question on a separate line without any additional explanation.

Argument:
{text}
```

---

## Prompt 5 — Few-Shot (2 Positive Examples + Definition)

```
You are an expert in argument analysis and critical reasoning.

Definition of Critical Questions (CQs):
Critical Questions are inquiries designed to evaluate the strength of an argument by uncovering and challenging its underlying assumptions, evidence, reasoning, and potential consequences.

Below is an example from our dataset:

Example Argument (CLINTON):
The central question in this election is really what kind of country we want to be and what kind of future we'll build together. Today is my granddaughter's second birthday. I think about this a lot. We have to build an economy that works for everyone, not just those at the top. We need new jobs, good jobs, with rising incomes. I want us to invest in you. I want us to invest in your future. Jobs in infrastructure, in advanced manufacturing, innovation and technology, clean, renewable energy, and small business. Most of the new jobs will come from small business. We also have to make the economy fairer. That starts with raising the national minimum wage and also guaranteeing equal pay for women's work. I also want to see more companies do profit-sharing.

High-Quality CQ Example 1:
"What is the proposed plan for making the economy fairer, beyond raising the national minimum wage and guaranteeing equal pay for women's work?"

High-Quality CQ Example 2:
"Could Clinton investing in you have consequences that we should take into account? Is it practically possible?"

Now, generate exactly 3 high-quality critical questions for the following argument. Each question should directly challenge some aspect of the argument (its reasoning, assumptions, evidence, consequences, or alternatives). Write one question per line with no additional explanation.

Argument:
{text}
```

---

## Prompt 6 — Few-Shot (Positive + Negative Contrast Examples)

```
You are an expert in argument analysis and critical reasoning.

Definition of Critical Questions (CQs):
Critical Questions are inquiries that examine an argument's underlying assumptions, logic, evidence, and potential consequences to determine its soundness.

Guidelines:
- Challenge the reasoning: Does the conclusion follow logically from the premises?
- Challenge the assumptions: Are there hidden or questionable assumptions?
- Challenge the evidence: Is there sufficient proof to support the claims?
- Challenge the consequences: Could accepting the argument lead to negative side effects?
- Challenge alternative explanations: Are there better ways to explain or address the issue?

Below are two examples from our dataset:

Example 1 (CLINTON):
Argument:
"The central question in this election is really what kind of country we want to be and what kind of future we'll build together. Today is my granddaughter's second birthday. I think about this a lot. We have to build an economy that works for everyone, not just those at the top. We need new jobs, good jobs, with rising incomes. I want us to invest in you. I want us to invest in your future. Jobs in infrastructure, in advanced manufacturing, innovation and technology, clean, renewable energy, and small business. Most of the new jobs will come from small business. We also have to make the economy fairer. That starts with raising the national minimum wage and also guaranteeing equal pay for women's work. I also want to see more companies do profit-sharing."

Good CQ: "What is the proposed plan for making the economy fairer, beyond raising the national minimum wage and guaranteeing equal pay for women's work?"
Bad CQ: "What specific policies would you implement to achieve an economy that works for everyone, and how would you ensure their effectiveness?"

Example 2 (CLINTON):
Argument:
"There are different views about what's good for our country, our economy, and our leadership in the world. It's important to look at what we need to do to get the economy going again: new jobs with rising incomes, investments, not in more tax cuts that would add $5 trillion to the debt."

Good CQ: "How will the success of these economic solutions be evaluated, and what metrics will be used to measure their effectiveness?"
Bad CQ: "What specific views is Clinton referring to, and how do they differ from her own views?"

Now, generate exactly 3 high-quality critical questions for the argument below. Write one question per line without any additional explanation.

Argument:
{text}
```

---

## Prompt 7 — Self-Assessment Check (Scaffolding)

```
You are an expert in argument analysis and critical reasoning.

Definition of Critical Questions (CQs):
Critical Questions are inquiries designed to uncover and challenge the assumptions, logic, evidence, and consequences within an argument, thereby assessing its overall strength.

How to Construct High-Quality Critical Questions:
1. Challenge the reasoning: Does the conclusion logically follow from the premises?
2. Challenge the assumptions: What hidden assumptions underlie the argument?
3. Challenge the evidence: Is there sufficient proof supporting the argument's claims?
4. Challenge the consequences: Could there be unintended negative outcomes if the argument is accepted?
5. Challenge alternative explanations: Are there other, better explanations or solutions?

Final Self-Assessment Check:
After generating each question, ask yourself: "Can the answer to this question diminish the acceptability of the argument?" Only questions that pass this check are high-quality.

Example (from CLINTON):
Argument:
"The central question in this election is really what kind of country we want to be and what kind of future we'll build together. Today is my granddaughter's second birthday. I think about this a lot. We have to build an economy that works for everyone, not just those at the top. We need new jobs, good jobs, with rising incomes. I want us to invest in you. I want us to invest in your future. Jobs in infrastructure, in advanced manufacturing, innovation and technology, clean, renewable energy, and small business. Most of the new jobs will come from small business. We also have to make the economy fairer. That starts with raising the national minimum wage and also guaranteeing equal pay for women's work. I also want to see more companies do profit-sharing."

Good CQ: "What is the proposed plan for making the economy fairer, beyond raising the national minimum wage and guaranteeing equal pay for women's work?"
Bad CQ: "What specific policies would you implement to achieve an economy that works for everyone, and how would you ensure their effectiveness?"

(The good CQ passes the self-assessment check because its answer could diminish the argument's acceptability.)

Now, generate exactly 3 high-quality critical questions for the argument below. Write each question on a separate line without any additional explanation, and ensure each question passes the self-assessment check.

Argument:
{text}
```

---

## Prompt 8 — Hybrid: Self-Assessment + Cross-Scheme Examples

```
You are an expert in argument analysis and critical reasoning. Your task is to generate exactly 3 high-quality critical questions for the argument provided below. These questions should challenge the argument by examining its reasoning, underlying assumptions, evidence, consequences, or by proposing alternative explanations.

Definition of Critical Questions (CQs): Critical Questions are inquiries that assess the soundness of an argument by uncovering and challenging its underlying assumptions, logic, and evidence. They are designed to expose weaknesses or gaps in the argument, thereby reducing its acceptability if answered unfavorably.

Examples to Guide You:
Example 1 (Argument from Cause to Effect): Argument: "If people migrate, unemployment rises." Good CQ: "Are there other economic factors that contribute to unemployment besides migration?" (Directly challenges the assumed direct causation by asking for alternative factors.) Bad CQ: "What is the history of migration?" (Off-topic and does not examine the causative link.)

Example 2 (Practical Reasoning): Argument: "Raising the minimum wage makes the economy fairer, so we should raise it." Good CQ: "Are there alternative policies that could achieve economic fairness without raising the minimum wage?" (Challenges the assumption that raising the minimum wage is the only solution.) Bad CQ: "What is the history of minimum wage laws?" (Too broad and does not evaluate the argument's core claim.)

Example 3 (Evaluating Evidence): Argument: "Investing in renewable energy will create jobs and boost the economy." Good CQ: "What evidence supports the claim that renewable energy investments lead to significant job creation and economic growth?" (Targets the evidence behind the claim.) Bad CQ: "Is renewable energy popular?" (Vague and unrelated to the economic impact.)

Final Self-Assessment Check: After generating each question, ask: "Can the answer to this question diminish the acceptability of the argument?" Only include questions that pass this check.

Your Task: Generate exactly 3 high-quality critical questions for the argument below. Write each question on a separate line without any additional explanation.

Argument: {text}
```

---

## Prompt 9 — Hybrid: Definition + Examples + Self-Assessment

```
You are an expert in argument analysis and critical reasoning. Your task is to generate exactly 3 critical questions that should be asked before accepting the argument below.

Argument: {text}

Definition of Critical Questions (CQs): Critical Questions are inquiries designed to evaluate the strength and validity of an argument by uncovering and examining the assumptions underlying its premises. They serve as tools to assess whether an argument is sound or fallacious by challenging its reasoning, evidence, and potential implications.

How to Construct High-Quality Critical Questions: Each critical question must: (1) Challenge the reasoning - Does the argument's conclusion logically follow from its premises? (2) Challenge the assumptions - Is the argument relying on hidden assumptions that might be false? (3) Challenge the evidence - What proof supports the argument's claims? (4) Challenge the consequences - Could there be unintended side effects of accepting the argument? (5) Challenge alternative explanations - Are there better explanations or solutions?

Examples of Strong Critical Questions:
Example 1: Argument: "If people migrate, unemployment rises." Good CQ: "Are there other economic factors that contribute to unemployment apart from migration?" Bad CQ: "What is the history of migration?" (Not directly relevant)
Example 2: Argument: "Raising the minimum wage makes the economy fairer, so we should raise it." Good CQ: "Are there alternative policies that could also achieve economic fairness without raising the minimum wage?" Bad CQ: "What is the history of minimum wage policies?" (Too broad)

Final Self-Assessment: After generating the 3 critical questions, apply this check to each one: "Can the answer to this question diminish the acceptability of the argument?" If yes, keep the question. If no, refine the question to make it more impactful.

Your Task: Generate exactly 3 high-quality critical questions. Ensure each question directly relates to the given argument (avoid generic questions). Do not introduce new topics or concepts not present in the argument. After generating each question, apply the self-assessment check. Write each question in one line without any explanation.

Now, generate the 3 critical questions:
```

---

## Prompt 10 — Scheme-Aware + Self-Assessment

*Uses `{scheme_details}` and `{scheme_examples}` placeholders filled from CQs-Gen scheme annotations.*

```
You are an expert in argument analysis and critical reasoning.

Your task is to generate exactly 3 critical questions that should be asked before accepting the argument below.

Argument:
{text}

Definition of Critical Questions (CQs):
Critical Questions are inquiries designed to evaluate the strength and validity of an argument by uncovering and examining the assumptions underlying its premises. They serve as tools to assess whether an argument is sound or fallacious by challenging its reasoning, evidence, and potential implications.

How to Construct High-Quality Critical Questions:
Each critical question must:
- Challenge the reasoning – Does the argument's conclusion logically follow from its premises?
- Challenge the assumptions – Is the argument relying on hidden assumptions that might be false?
- Challenge the evidence – What proof supports the argument's claims?
- Challenge the consequences – Could there be unintended side effects of accepting the argument?
- Challenge alternative explanations – Are there better explanations or solutions?

Argumentation Schemes Present in the Intervention:
{scheme_details}

Below are examples of how to critically analyze the argumentation schemes identified above.

Example 1: Argument from Cause to Effect
Argument: "If people migrate, unemployment rises."
Good CQ: "Are there other economic factors that contribute to unemployment apart from migration?"
Bad CQ: "What is the history of migration?" (Not directly relevant)

{scheme_examples}

Final Self-Assessment:
After generating the 3 critical questions, apply this check to each one:
"Can the answer to this question diminish the acceptability of the argument?"
- If yes, keep the question.
- If no, refine the question to make it more impactful.

Your Task:
- Generate exactly 3 high-quality critical questions.
- Ensure each question directly relates to the given argument (avoid generic questions).
- Do not introduce new topics or concepts not present in the argument.
- After generating each question, apply the self-assessment check.
- Write each question in one line without any explanation.

Now, generate the 3 critical questions:
```

---

## Prompt 11 — Constraint-Oriented (Negative Constraints Only)

```
Your objective is to generate critical questions that can effectively challenge the validity of the reasoning in the argument below:

{text}

A question is NOT a useful critical question if:
- The question is unrelated to the content of the argument.
- The question lacks specificity (for example, it could apply generically to numerous arguments).
- The question introduces concepts or entities not present in the argument (such as by suggesting potential answers).
- The question does not serve to challenge the argument's validity. For instance, if it merely tests reading comprehension or solicits the speaker's personal views.
- Its answer would be unlikely to weaken the argument. This occurs when the answer represents common knowledge, or when the argument itself already provides the answer.

Generate exactly 3 critical questions. Write one question per line.
Provide only the questions without any additional commentary or justification.
```

---

## Prompt 12 — Scheme-Aware + Expert Reasoner Persona

*Uses `{argumentation_scheme}` placeholder filled from CQs-Gen scheme annotations.*

```
Role: You are an expert critical thinker and an argumentation theorist, highly skilled in identifying weaknesses, assumptions, and implications within arguments. Your task is to generate insightful and probing critical questions.

Task: Given the argument below and its identified argumentation scheme, generate a set of high-quality, useful critical questions. These questions should challenge the argument's validity, expose its underlying assumptions, and probe for potential exceptions, counter-arguments, or overlooked factors.

Argument Text: {text}
Argumentation Scheme: {argumentation_scheme}

Output Requirements:
- Generate exactly 3 distinct critical questions.
- Each question must be clear, concise, and unambiguous.
- Questions should go beyond simple factual recall; they must encourage deeper analysis and evaluation of the argument.
- Ensure the questions are directly relevant to the provided argument and its specific argumentation scheme. They should challenge the typical critical questions associated with that scheme, adapted to the specifics of the given argument.

Guidelines for Critical Questions (ensure your questions reflect these considerations):
- Challenge Premises: Do the premises truly support the conclusion? Are they accurate or complete?
- Expose Assumptions: What unstated beliefs or conditions must be true for the argument to hold? Are these assumptions justified?
- Identify Exceptions/Counter-examples: Are there situations where the argument's logic would break down?
- Consider Alternative Explanations: Could there be other reasons for the observed phenomena or different ways to interpret the evidence?
- Assess Relevance/Sufficiency: Is the evidence presented relevant and sufficient to establish the conclusion?
- Probe for Bias/Credibility: (Especially for schemes like "Argument from Expert Opinion") Is the source credible, unbiased, and within their area of expertise?
- Evaluate Consequences/Implications: What are the broader implications if the argument is accepted? Are there unintended negative consequences?

Suggest 3 critical questions that should be raised before accepting the arguments in this text:
Give one question per line. Make the questions simple, and do not give any explanation regarding why the question is relevant.
```

---

## Prompt 13 — Constraint-Oriented (Adapted from Calvo-Figueras et al.)

*Adapted from Calvo-Figueras et al. (2025).*

```
You are tasked with generating critical questions that are useful for diminishing the acceptability of the arguments in the following text:
{text}

Take into account a question is not a useful critical question:
- If the question is not related to the text.
- If the question is not specific (for instance, if it's a general question that could be applied to a lot of texts).
- If the question introduces new concepts not mentioned in the text (for instance, if it suggests possible answers).
- If the question is not useful to diminish the acceptability of any argument. For instance, if it's a reading-comprehension question or if it asks about the opinion of the speaker/reader.
- If its answer is not likely to invalidate any of the arguments in the text. This can be because the answer to the question is common sense, or because the text itself answers the question.

Output 3 critical questions.
Give one question per line.
Make sure there are at least 3 questions.
Do not give any other output.
Do not explain why the questions are relevant.
```

---

## Prompt 14 — Baseline: Claim-Specific Instruction

```
Suggest 3 critical questions that should be raised before accepting the arguments in this text:
{text}

Make sure the questions are directly related to the specific claims, evidence, or reasoning in the argument.
Each question should be concise, self-contained, and avoid asking for general background knowledge.
Do not provide explanations or justifications for the questions.
Give one question per line.
```

---

## Prompt 15 — Constraint-Oriented with Vulnerability Guidelines

```
Suggest 3 critical questions that should be raised before accepting the arguments in this text:
{text}

A critical question is a focused inquiry aimed at testing the strength, credibility, or completeness of the argument.
Each question must be:
- Grounded in the given argument, not generic.
- Designed to reveal potential weaknesses, hidden assumptions, or missing information.
- Understandable in isolation (someone reading the question should grasp what part of the argument it refers to).

Consider possible vulnerabilities, including:
- Reliability, bias, or expertise of the source.
- Truth or sufficiency of premises.
- Relevance of premises to the conclusion.
- Alternative explanations or counterexamples.
- Exceptions, limitations, or unstated conditions.
- Consequences or implications if the claim is accepted.

Give exactly 3 questions, one per line, and do not include explanations.
```

---

## Prompt 16 — Investigative Journalist Persona *(Best-Performing)*

```
You are an experienced investigative journalist who also teaches critical thinking. Your dual expertise allows you to both detect misinformation and educate others on how to critically analyze arguments.

Your goal is to generate questions that students should reflect on before accepting the arguments in a text as valid. These questions should help students develop critical thinking skills and identify potential weaknesses in argumentative content.

Text to Analyze:
{text}

Your Mission:
Generate critical questions that expose weaknesses in the arguments presented. As a journalist specializing in misinformation detection, focus on questions that would help readers identify:
- Missing or weak evidence
- Unsubstantiated claims
- Logical gaps or fallacies
- Source credibility issues
- Hidden assumptions
- Alternative explanations

Quality Standards for Your Questions:

Generate USEFUL questions that:
- Challenge claims that lack sufficient evidence
- Question the reliability or credibility of sources mentioned
- Expose logical gaps between premises and conclusions
- Identify missing context or information
- Challenge generalizations or causal claims
- Question methodology behind studies or data cited
- Are essential — students should not accept arguments without considering these questions

Avoid UNHELPFUL questions:
- Where the answer is common sense or well-known facts
- That are overly complicated or impractical to investigate
- Where the text already provides the answer
- That are too broad or theoretical to be actionable

Never create INVALID questions that:
- Introduce new concepts not mentioned in the text (e.g., bringing up topics like "income inequality" when the text doesn't mention it)
- Are unrelated to the actual content presented
- Challenge arguments not made (e.g., criticizing positions the speaker doesn't actually hold)
- Are too general — could apply to any text without being specific to this content
- Are non-critical — reading comprehension questions or requests for speaker opinions

Professional Standards:
As an investigative journalist, ensure each question:
- Targets specific claims made in the text
- Could be investigated through research or fact-checking
- Exposes potential misinformation tactics
- Helps readers think like fact-checkers

Output Format:
Generate exactly 3 critical questions, one per line. Each question should:
- Be specific to the content presented
- Challenge a particular claim or assumption
- Help students identify potential weaknesses before accepting the arguments
- Be written in a clear, professional style

Example Approach:
If text claims: "Studies show X causes Y"
Your question: "What specific studies demonstrate this causal relationship, and have they been independently replicated?"

If text states: "Experts agree that..."
Your question: "Which experts specifically, and what are their credentials in this particular domain?"

Now generate 3 critical questions that would help students critically evaluate this argumentative text before accepting its claims. Do not explain why the questions are relevant.
```

---

## Prompt 17 — Skeptical Judge Persona

```
You are a rigorous and skeptical evaluator of arguments.

Task:
Generate 3 critical questions to examine the reasoning in the provided argument. Critical questions are probing inquiries that test an argument's validity by exposing underlying assumptions in its claims.

Write one question per line. Ensure questions are concise and focused, without providing justifications.

Argument:
{text}
```

---

## Prompt 18 — Scheme-Aware + Skeptical Judge Persona

*Uses `{scheme_details}` placeholder filled from CQs-Gen scheme annotations.*

```
You are a rigorous and skeptical evaluator of arguments.

Argumentation Schemes: Argumentation schemes are recurring patterns of reasoning that represent common forms of defeasible arguments — arguments that are reasonable but subject to challenge. Each scheme characterizes a reasoning structure with standard premises and a conclusion. Below are the argumentation schemes identified in this argument, along with their definitions and associated critical question templates:

{scheme_details}

Task:
Using the supplied schemes and their critical question templates, generate 3 critical questions to examine the reasoning in the provided argument. Critical questions are probing inquiries that test an argument's validity by exposing underlying assumptions in its claims.

Write one question per line. Ensure questions are concise and focused, without providing justifications.

Argument:
{text}
```

---
