---
layout: post
title: "Eval What You Own"
date: 2026-05-11 19:08
comments: true
categories: engineering
published: true
cta: none
---
Recently, I've been setting up agents to map reduce my workflows. I'm generally pulling content from a few sources, synthesizing it, and reporting. The shape is pretty consistent. It ends up looking like a few specialist connectors and agents and one synthesizing agent running weekly. This leads to one of the more sinister problems in software development: a mighty long feedback loop. So I looked to evals to speed up iteration.

There were three places I considered investing in evals:
1. Sub-agent level: deterministic checks on what each connector returns
2. Synthesis level: given sub-agent output checks what the synthesizer produces
3. End-to-end: run the whole system, and grade the final output

In comparing the three, I found a lot of resonance with the testing pyramid. Sub-agent tests look like unit tests, synthesis tests are almost like integration tests, and end-to-end, well, they're like end-to-end tests. While the pyramid analogy was useful for wrapping my head around the options, the right choice was derived from first principles.

Sub-agent evals are easy to set up. But you end up mostly mocking the wrong things and testing third-party output. End-to-end evals fight the wide disparity of outputs that emerge from non-deterministic systems. The noise from the output would be too much given my multi-level agentic system. That process of elimination led me to the synthesis level.

The synthesis layer is unique in that it's the layer that I fully own. This means every failure point is something I can steer toward the intended output. Sub-agent evals could actually fail on the third-party output. End-to-end failures add too much noise. Synthesis failure is either in the parsing of the third-party output, the synthesis of it or the output itself. All of this is within my system boundaries.

The trick that makes this work: input canned sub-agent output into the real synthesizer as fixtures. If you have set up JSON contracts between your sub-agent and agent, you are not mocking the LLM, you're mocking the data the LLM sees. This provides a deterministic input boundary around a non-deterministic one focusing the signal on the synthesis behavior. Now you can start providing fixtures to mock potential failure modes.

The fixture patterns present themselves as you write down failure modes. How might your sub-agents fail? How might the synthesis fail? And what category of failure mode might they hit? I found myself generating a lot of fixtures to test for groundedness across sub-agents and the synthesis agent. I found a smaller subset of failures in privacy which might pass groundedness checks but provide sub-agent data I would not want in my output. Lastly, I put together a happy path test of what the system should look like given everything is wired up well.

Given more time, people, and scope, I can imagine spending more time on tuning holistic evals. But given these evals are for my personal projects, focusing on evaling the layer I own provides a ton of value in tightening my feedback loop.
