---
layout: page
title: AI-Powered Legal Intake Assistant
description:
img: assets/img/10_project/aliados_thumbnail.svg
importance: 1
category: Independent
---

**Stack:** Python · Flask · Google Gemini 2.5 · Resend · HTML · CSS · JavaScript

**Link:** [https://aliados.onrender.com/](https://aliados.onrender.com/)

## Overview

Aliados is a legal intake assistant that solves a core LLM orchestration challenge: getting a conversational AI to simultaneously interview, extract, assess, and route information without drift or quality loss. The solution is an agentic pipeline of six specialized models. Four run in sequence on every user message — conducting the interview, classifying responses, scoring topic coverage, and selecting what to address next. The remaining two handle end-of-interview tasks: generating the attorney-ready summary and managing corrections during review.

## LLM Orchestration — Agentic Pipeline

The most technically interesting piece is a custom multi-agent pipeline that runs on every user message. Rather than relying on a single LLM to do everything, the system chains six specialized models, each with a narrow, well-defined responsibility. Each model makes autonomous decisions that shape subsequent steps.

**LLM 1 — Interviewer**
Conducts the conversation. Follows trauma-informed guidelines: 6th-grade reading level, no compound questions, normalized framing ("Many people experience…"), sensitivity to emotional cues. Receives only the active topic and recent history so it stays focused.

**LLM 2 — Information Classifier**
Reads the user's response and extracts discrete facts, routing each to the correct topic bucket. Handles ambiguous references (e.g., bare "yes") using conversation context to resolve the referent.

**LLM 3 — Thoroughness Assessor**
Scores each topic 0.0–1.0 based on coverage depth. Considers specificity, dates, timelines, and completeness. Each topic has a configurable threshold — sensitive topics (trauma, persecution) require 0.75+, low-sensitivity intro topics can close at 0.2.

**LLM 4 — Topic Selector**
Decides which topic to focus on next, balancing completion scores against conversational momentum. Keeps recently-surfaced topics active even if incomplete; avoids jarring topic switches mid-emotional disclosure.

**LLM 5 — Summary Generator**
After all topics clear their thresholds, produces a plain-language, professionally structured summary organized by topic for the attorney's review.

**LLM 6 — Edit Intent Detector**
During review mode, identifies whether a user message is requesting a correction, which topic it concerns, and how confident the model is. Routes accordingly without restarting the interview.

Four models run in sequence on every user message, handling the core interview loop. The summary generator activates once all topics reach their completion thresholds, and the edit intent detector takes over during review mode. The system includes fallback parsing to handle cases where a model returns a malformed response, keeping the conversation on track.

## Evaluation — Law Student Testing

Rather than relying solely on automated metrics, the current stage of the system has been evaluated primarily through structured human testing with law students. Testers ran live sessions simulating real client scenarios — including sensitive disclosures, ambiguous answers, and mid-session corrections — and provided feedback on conversation quality, information accuracy in the generated summaries, and how well the trauma-informed guidelines held up under realistic conditions.

This feedback loop has been the main driver of prompt iteration, threshold tuning, and topic flow improvements. Using law students specifically brought domain knowledge into the evaluation process: testers could judge not just whether the system felt natural, but whether the outputs would actually be useful to a practicing attorney.

## Key Features

**Bilingual Support**
All prompts, UI strings, and first messages are loaded from language packs (English and Spanish). Switching languages is a single environment variable.

**Voice Input**
Hold-to-record microphone button built into the interface. Audio is streamed to a transcription service backed by OpenAI GPT-4o Whisper, then auto-submitted into the chat flow.

**Review & Edit Mode**
Once all topics are complete, the system enters review mode. Users can correct information; old entries are marked `superseded` with a timestamp and new entries flagged `updated`, maintaining a full audit trail without losing prior data.

**Email Artifacts**
On completion, the system emails the attorney a package containing: the plain-text summary, the full structured data, the conversation transcript, and the session log — everything needed to begin the case.

**Session Isolation**
Each session gets a unique ID and its own dedicated namespace. Concurrent users never share state or data.

**Progressive Topic Completion**
The progress bar and sidebar checklist update in real time as topics are marked complete. Attorneys watching a live session can see exactly which areas have been covered.

## Architecture

When a user sends a message, the backend processes it through the agentic pipeline in sequence — classifying what was said, re-scoring topic coverage, selecting what to address next, and generating the interviewer's reply — before returning a response and updated progress to the frontend.

Topic data is persisted to a per-session file so it survives server restarts and can be emailed on completion.

The mode system (immigration vs. personal injury) is a folder-based plugin: each mode has its own topic definitions and language files. Adding a new practice area requires no code changes — only a new config folder.

## Design Decisions Worth Calling Out

**Why six LLMs instead of one?**
A single LLM asked to simultaneously converse, extract, assess, and select topics tends to drift — it prioritizes whichever role fits the prompt best. Separating concerns gives each model a clean, evaluable task and makes failures diagnosable. It also allows independent model swaps: the classifier could run on a cheaper model while the interviewer uses a more capable one.

**Why Gemini 2.5 Flash-Lite as the primary model?**
Low latency matters in a conversational context. Flash-Lite's speed keeps the chat feeling live. The trade-off in capability is acceptable because each model call is narrowly scoped — no single call needs to do heavy reasoning.

**Trauma-informed constraints baked into prompts, not UI copy**
The trauma-informed guidelines (normalized language, no accusatory phrasing, validation of emotional responses) are embedded directly in the model system prompts rather than handled as frontend copy. This means the AI's word choices, not just the interface's label text, follow these principles.

## Impact

Built for a practicing immigration attorney to replace manual phone intake calls. The system reduces attorney time spent on preliminary information gathering, gives clients a private, low-pressure channel to disclose sensitive history, and produces a structured output that feeds directly into case preparation — without the client ever needing to repeat themselves.
