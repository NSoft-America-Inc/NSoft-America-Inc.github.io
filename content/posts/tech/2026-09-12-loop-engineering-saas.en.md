---
title: "Loop Engineering for SaaS: From Finished Tasks to Verified Outcomes"
date: 2026-09-12T12:00:00-05:00
draft: false
translationKey: loop-engineering-saas
tags: ["AI-Agents", "Loop-Engineering", "SaaS", "SoftwareEngineering"]
categories: ["Tech"]
description: "What our NSoft-SaaS development records taught us about feedback loops, integration ownership, and resuming interrupted work—and the small experiment we will try next."
author: "NSoft America"
---

A coding agent finishes its task. The tests it ran pass. Another agent finishes a related change. Yet someone still has to connect both changes to the application, check which revision was tested, and decide whether the work is actually complete.

We encountered this gap while reviewing NSoft-SaaS development records. It led us to a practical question: **how can the result of each attempt reliably determine the next action?**

This is the starting point for our study of *Loop engineering*. In this article, we use that term for designing an execution cycle with feedback, explicit completion criteria, and recoverable state. It is our working scope, not a claim that there is one standardized method with this name. Our first analysis is complete; the experiment described below has not yet run.

## When individual tasks finish before the system does

Our review covered parallel implementation, independent code review, and deployment preparation. Three observations stood out.

| What the records showed | What still required coordination |
| --- | --- |
| Security, request logging, and audit metrics were developed in separate working directories with defined file ownership. | Shared application entry points and configuration still needed integration. |
| Code approval, local tests, CI, and status documents were completed at different times. | Someone had to reconcile what each completion claim covered. |
| A deployment attempt was interrupted, and a subsequent resume command omitted its execution profile. | Someone had to reconstruct the environment and check the actual state before continuing. |

These observations do not prove that parallel work was inefficient. Separating file ownership gave us clear boundaries, and independent reviews found useful defects. The unresolved problem was the connection between **a local result and the next justified action**.

An agent's final response is one event in that process. It does not, by itself, establish that the application is integrated or that a deployment process has stopped.

## Turn execution into a feedback loop

There is useful prior work behind this approach. Anthropic describes an evaluator–optimizer pattern in which generation receives evaluation and feedback, particularly when criteria are clear. It also emphasizes environmental feedback and stopping conditions for agents. We use those architectural ideas here, rather than treating its older tooling examples as a current product comparison. [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents).

For our proposed development loop, the essential decision happens after verification. A successful check may justify completion. A failure needs classification before another attempt.

![Proposed development loop: define the target, execute a bounded change, verify evidence, then finish, revise, or pause for a decision. A revision returns to execution.](/images/loop-engineering-feedback.svg)

*Figure 1. Proposed design, not an execution trace. Verification determines whether the loop finishes, revises its approach, or pauses for a decision.*

The return path matters as much as the forward path. An environment error calls for environment repair. A product defect calls for a code change. An unclear requirement calls for a decision. Sending all three back as “try again” discards the information that verification produced.

Completion also needs a scope. “Local tests passed” and “the deployed service passed acceptance checks” are different outcomes. Each result should identify the revision and environment it actually covers.

## Keep parallel work inside an explicit integration boundary

The first change we want to test is deliberately small: keep separate file ownership, but define integration ownership and completion evidence before work starts.

Consider request logging and access control. Both can be developed independently, but both may need registration in the same application entry point. We would assign that shared file to an integration owner and agree on the connection requirements in advance.

| Responsibility | Evidence needed for the next step |
| --- | --- |
| Component worker | Identified change, relevant local checks, and remaining connection requirements |
| Integration owner | Components connected to the application with the agreed configuration |
| Independent reviewer | Checks of the integrated behavior, including relevant failure cases |
| Loop coordinator | A decision tied to those results: complete, revise, or wait for a specific input |

These are responsibilities, not a requirement to create four agents. A small change may need fewer participants. The useful separation is between producing a change and deciding what evidence supports the next transition.

Our records also contained a revealing test issue: a helper process returned success after receiving no messages. Its exit status did not prove that the intended processing had occurred. The investigation led to checking the target message's processing and acknowledgement instead. That is a concrete reason to define the observable result before relying on a green status.

## Make an interrupted loop understandable

Long-running work adds another problem: the next session needs enough state to continue. Anthropic's long-running harness work describes initialization and incremental coding sessions supported by progress records and version history. We take this as support for explicit handoff state, not evidence that our own resume process is already reliable. [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents).

For NSoft-SaaS, we propose separating three kinds of information. The first changes infrequently; the second accumulates; the third describes where execution stands now.

![Three records for a recoverable loop: a task contract defining the goal and limits, an attempt log recording changes and observations, and a checkpoint recording the last successful step and actual execution state.](/images/loop-engineering-records.svg)

*Figure 2. Proposed record structure. A checkpoint helps locate the next action, but current process and resource state must still be checked before repeating an external operation.*

The **task contract** captures the target, ownership, success criteria, and applicable limits. The **attempt log** records what changed, what happened, and why the next action was selected. The **checkpoint** identifies the last successful step, code revision, environment references, and any operation that may still be running.

After an interruption, the first question is whether the previous action is still running or already changed the environment. Only then can the coordinator choose to wait, verify, repair, or resume. A saved message saying “deployment in progress” is insufficient to make that choice.

## Test the method before selecting the orchestration tool

Our first experiment will use a small NSoft-SaaS change that can be split into two parts and integrated through a shared entry point. We will retain our current tools and introduce the explicit ownership and evidence record.

We will track elapsed time through integrated verification, clarification requests after handoff, rework caused by missing connections, defects found during review, and the time spent maintaining the record. Where usage data is available, we will also record cost. Unknown values will remain unmeasured.

The first run can establish a baseline if a comparable historical measurement is unavailable. It will not establish a general productivity improvement. A later comparison with orchestration tools must account for differences in task size, model, concurrency, and verification scope.

For this project, a useful loop must answer three questions: **What result did we observe? Why does it justify the next action? What state is needed to continue after an interruption?**

Our next step is to answer those questions in one bounded experiment. If the record reduces coordination without weakening verification or adding more overhead than it saves, we will extend it. Tool selection will follow the operational needs that the experiment exposes.
