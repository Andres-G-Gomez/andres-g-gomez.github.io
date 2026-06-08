---
layout: page
title: Medical Records Chronology Generator
description:
img: assets/img/medchrons_homepage.jpg
importance: 2
category: Independent
---

**Stack:** Python · Flask · Google Cloud Document AI · Google Gemini · PyMuPDF · python-docx · openpyxl · Docker

## The Problem

In medical-legal cases, attorneys need to reconstruct the complete clinical history of a patient — every visit, diagnosis, procedure, and prescription, in order, across potentially hundreds of pages of records from multiple hospitals and providers. This process is done manually, takes hours per case, and is prone to error.

Medical records are voluminous, inconsistently formatted, full of clinical jargon, and spread across incompatible systems. Legal teams need a single, organised table of events that establishes the facts of a case, validates a client's medical history, and identifies billing discrepancies — the kind of clear record that supports settlement negotiations and, where necessary, litigation. This tool aims to automates that process.

## What It Does

MedChron is an AI-powered web application that ingests PDF medical records and outputs structured chronology tables in Word or Excel format, ready for legal use. It extracts dates, providers, diagnoses, medications, and procedures; writes plain-English summaries suitable for a lay jury; flags low-confidence extractions for attorney review; and automatically highlights gaps in treatment that may be strategically significant in litigation.

<div class="container-fluid text-center">
        {% include figure.html path="assets/img/medchrons.jpg" title="MedChron application screenshot" class="img-fluid d-block mx-auto w-100" %}
</div>

## My Role

I owned the project end-to-end: from early user research and outreach to interested parties, through system design, backend development, cloud integrations, prompt engineering, and output formatting. I also partnered with a board-certified Legal Nurse Consultant to validate outputs and iteratively refine the system against real and synthetic medical records.

## How It Works

The system processes documents through seven stages:

**Ingestion and OCR.** Uploaded PDFs are processed using Google Cloud Document AI, which extracts text from typed records, handwritten notes, tables, and mixed layouts, returning per-page confidence scores.

**Document segmentation.** Pages are grouped into logical clinical documents using heuristic boundary detection — recognising discharge summaries, operative reports, page numbering patterns, and section headers. This mirrors how a human reviewer would mentally organise the records before reading them.

**Image extraction.** Embedded images — charts, scans, diagnostic graphs — are extracted using PyMuPDF, with automated filtering to exclude logos and decorative elements. Clinically relevant visuals are preserved in the output.

**Clinical summarisation.** Each document segment is passed to Google Gemini with custom prompts developed in collaboration with the Legal Nurse Consultant. The model generates two outputs per record: a clinical summary maintaining medical accuracy, and a plain-English explanation written for attorneys and jurors. Prompt engineering safeguards prevent the model from making standard-of-care opinions, outcome predictions, or permanency claims — common failure modes for LLMs in legal contexts.

**Chronology assembly.** The structured data is assembled into a sortable table with columns for date, source document, page number, facility, physician, provider type, clinical findings, and attorney-facing explanation. Rows where consecutive records exceed a 30-day gap are automatically flagged with gap markers. Extractions below 90% confidence are highlighted in amber for manual review.

**Concurrent job management.** Processing runs asynchronously in background threads with real-time status updates, supporting multiple simultaneous uploads without blocking the interface.

## Technology Stack

- **Backend:** Python, Flask
- **OCR and document understanding:** Google Cloud Document AI
- **LLM summarisation:** Google Gemini via Vertex AI
- **PDF image extraction:** PyMuPDF
- **Output generation:** python-docx, openpyxl
- **Infrastructure:** Docker, Google Cloud service accounts

## Validation Process

Accuracy in a legal context isn't optional — a missed diagnosis or a misattributed provider could affect a case outcome. To validate the system, I worked with a board-certified Legal Nurse Consultant throughout development.

We created synthetic medical records using LLMs, then used those records to stress-test document segmentation, entity extraction, gap detection, and explanation quality. The consultant reviewed outputs for clinical accuracy, appropriate terminology, and legal relevance. Each round of feedback drove prompt refinements and pipeline adjustments — a tight loop of model output, expert review, and system improvement.

## Engineering Decisions Worth Noting

**Prompt guardrails over post-processing.** Rather than filtering LLM outputs after generation, I built constraints directly into the prompt — no causal claims without hedging, no first-person phrasing, no speculation about outcomes. This produces cleaner outputs at source rather than patching them downstream.

**Confidence-aware output.** Every extraction carries a confidence score from its source API. Rather than silently discarding uncertain results, the system surfaces them to attorneys with amber highlighting, preserving recall while directing human attention to where it's most needed.

**Heuristic segmentation before AI.** Document boundary detection uses deterministic rules rather than an LLM. This keeps the segmentation stage fast, predictable, and debuggable — a deliberate choice in a domain where silent failures are costly.

## Results

MedChron reduces a process that typically takes legal teams several hours per case to a matter of minutes, while producing output that is formatted, sourced, and ready for litigation use. The system handles records from multiple providers and formats in a single pass, and its confidence-flagging approach means attorneys know exactly where to focus their review time.
