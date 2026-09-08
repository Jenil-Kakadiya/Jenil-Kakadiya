# MeetMate — AI Meeting Assistant

> An AI-powered meeting assistant that joins Google Meet, understands the ongoing conversation, maintains meeting context, and can participate in two-way conversations when triggered.

## Overview

MeetMate was an AI meeting assistant designed to participate in Google Meet sessions through a browser-controlled bot.

Unlike a conventional meeting transcription tool that only records and summarizes conversations, MeetMate was designed around **continuous meeting understanding and contextual interaction**.

The system could:

* Join and operate inside a Google Meet session.
* Extract meeting conversation/audio through a Playwright-controlled Chrome session.
* Process and chunk meeting information.
* Generate embeddings and store them in ChromaDB.
* Retrieve relevant meeting context using a RAG pipeline.
* Detect configured trigger words to determine when the assistant should respond.
* Generate a contextual response based on the conversation that had occurred so far.
* Convert the generated response into speech.
* Inject the generated audio back into the active Chrome/Google Meet session.
* Generate a complete meeting summary after the meeting ended.

The system was also designed to support extending the assistant's knowledge beyond the current meeting by incorporating external sources such as **Jira, Confluence, and Google Docs**.

---

## My Role

I worked on implementing core features required to make MeetMate function as an interactive meeting participant.

My primary contributions included:

### 1. Two-Way Audio Interaction

Implemented the capability for the bot to inject generated speech back into the ongoing Google Meet conversation.

The flow was:

```text
AI Response
     ↓
Text-to-Speech
     ↓
Flask API
     ↓
Generated Audio
     ↓
Playwright / Chrome Session
     ↓
Google Meet
     ↓
Meeting Participants
```

This allowed MeetMate to move beyond passive listening and participate in a meeting when triggered.

---

### 2. Audio Injection into the Browser Session

One of the challenging parts of the implementation was making generated audio consistently available to the **same Chrome/browser session being used by the meeting bot**.

The initial approach of injecting audio into the Chrome environment was not persistent.

The challenge was that the audio being generated and the audio path used by the active meeting session were not always operating on the same session/context.

I debugged the browser/session lifecycle and identified the correct session used by the bot.

The implementation was then changed so that generated audio was injected into the **Chrome session used by the MeetMate bot**, allowing the audio to persist long enough for the bot to participate in the meeting.

This was important because the overall interaction depended on:

```text
Meeting Audio
      ↓
Conversation Understanding
      ↓
Trigger Detection
      ↓
Context Retrieval
      ↓
LLM Response
      ↓
Text-to-Speech
      ↓
Audio Injection
      ↓
Meeting
```

A failure at the audio-injection layer would effectively break the entire two-way interaction pipeline.

---

# RAG Pipeline

I implemented the RAG pipeline responsible for converting meeting information into searchable contextual knowledge.

The pipeline consisted of:

```text
Meeting Data
     ↓
Data Extraction
     ↓
Cleaning / Pre-processing
     ↓
Chunking
     ↓
Embedding Generation
     ↓
ChromaDB
     ↓
Similarity Retrieval
     ↓
Relevant Context
     ↓
LLM
     ↓
Context-aware Response
```

## Data Extraction

Information generated during the meeting was extracted and prepared for downstream processing.

The extracted content became the source material for both:

* contextual question answering during the meeting
* final meeting summarization

---

## Chunking

Long meeting content was divided into smaller chunks before generating embeddings.

This allowed the retrieval system to search for specific portions of the meeting rather than passing the complete conversation to the model for every request.

Conceptually:

```text
Long Meeting Transcript
          ↓
 ┌────────┬────────┬────────┐
 │Chunk 1 │Chunk 2 │Chunk 3 │ ...
 └────────┴────────┴────────┘
          ↓
      Embeddings
```

---

## Embedding Storage

Generated embeddings were stored in **ChromaDB**.

This provided a vector-search layer for the meeting knowledge base.

```text
Meeting Chunk
     ↓
Embedding
     ↓
ChromaDB
```

The stored vectors could then be searched when MeetMate needed to understand previous parts of the conversation.

---

## Retrieval

When the assistant was triggered, the current conversation context could be used to identify relevant historical meeting information.

The retrieval process was conceptually:

```text
Current Conversation
        ↓
Query / Context
        ↓
Embedding
        ↓
Vector Search
        ↓
Relevant Meeting Chunks
        ↓
LLM Context
```

This allowed responses to be grounded in information discussed earlier in the meeting instead of relying only on the most recent utterance.

---

# Trigger-Based Interaction

MeetMate was not intended to continuously speak.

Instead, trigger words were monitored during the meeting to determine when the assistant should participate.

The interaction model was:

```text
Meeting Conversation
        ↓
Monitor Conversation
        ↓
Trigger Word Detected?
      /       \
    No         Yes
    ↓           ↓
Continue     Retrieve Context
Monitoring       ↓
             Generate Response
                  ↓
              Text-to-Speech
                  ↓
             Inject Audio
                  ↓
             Continue Meeting
```

This helped make the assistant behave more like a meeting participant rather than continuously interrupting the conversation.

---

# Context-Aware Two-Way Conversation

A key part of the design was that MeetMate's response could be generated using the **meeting context accumulated up to that point**.

For example:

```text
Participant:
"We should deploy the service next Friday."

...

Participant:
"MeetMate, what did we decide about the deployment?"

              ↓

Retrieve relevant meeting context

              ↓

LLM

              ↓

"Earlier in the meeting, the team agreed
to deploy the service next Friday."
```

The objective was to make the assistant aware of the conversation rather than treating every interaction as an isolated question.

---

# Text-to-Speech Service

I implemented Flask-based endpoints responsible for providing text-to-speech functionality.

The interaction was:

```text
Generated Text
      ↓
Flask Endpoint
      ↓
Text-to-Speech Processing
      ↓
Audio Output
      ↓
Playwright-controlled Chrome Session
```

The generated audio was then passed into the browser environment used by the meeting bot for playback into Google Meet.

---

# End-to-End Architecture

The overall MeetMate workflow can be represented as:

```text
                         GOOGLE MEET
                             │
                             ▼
                     Playwright / Chrome
                             │
                     ┌───────┴────────┐
                     │                │
                  Audio           Conversation
                     │                │
                     └───────┬────────┘
                             ▼
                      Data Extraction
                             │
                             ▼
                         Chunking
                             │
                             ▼
                        Embeddings
                             │
                             ▼
                         ChromaDB
                             │
                             │
                  ┌──────────┴──────────┐
                  │                     │
             Retrieval             Meeting Context
                  │                     │
                  └──────────┬──────────┘
                             ▼
                           LLM
                             │
                    Trigger Detected?
                             │
                             ▼
                       AI Response
                             │
                             ▼
                    Flask TTS Endpoint
                             │
                             ▼
                       Generated Audio
                             │
                             ▼
                  Playwright / Chrome
                             │
                             ▼
                       GOOGLE MEET
```

---

# External Knowledge Integration

The architecture was designed to extend MeetMate's knowledge beyond the current meeting.

Potential knowledge sources included:

* Jira
* Confluence
* Google Docs
* Meeting documents
* Other organizational knowledge sources

This would allow the retrieval layer to combine:

```text
Current Meeting
      +
Previous Meeting Context
      +
Jira
      +
Confluence
      +
Google Docs
      ↓
Unified Knowledge Retrieval
      ↓
Context-Aware AI Response
```

For example, a participant could ask:

> "What is the status of the Jira issue we discussed?"

Instead of only searching the current transcript, the assistant could retrieve relevant information from the organization's connected systems.

---

# Meeting Summary

At the end of the meeting, the accumulated conversation was processed to generate a complete meeting summary.

The summary could include information such as:

* Key discussion points
* Decisions
* Action items
* Important context
* Follow-up topics

The lifecycle therefore became:

```text
Meeting Starts
      ↓
Listen + Extract
      ↓
Store Context
      ↓
Monitor Triggers
      ↓
Interact When Required
      ↓
Meeting Ends
      ↓
Generate Complete Summary
```

---

# Key Engineering Challenges

## Persistent Audio Injection

The most challenging issue was ensuring that generated audio was injected into the **actual Chrome session being used by the meeting bot**.

The initial audio injection approach was not persistent, which prevented reliable two-way communication.

Debugging the browser/session lifecycle and moving the audio injection into the correct session resolved the issue.

---

## Maintaining Context During a Live Meeting

A meeting is a continuously growing source of information.

Sending the entire transcript to the LLM for every interaction would be inefficient and would eventually exceed practical context limits.

The RAG architecture addressed this by:

```text
Growing Meeting
      ↓
Chunk Information
      ↓
Generate Embeddings
      ↓
Vector Storage
      ↓
Retrieve Only Relevant Context
      ↓
LLM
```

This made contextual interaction more practical as meeting length increased.

---

## Connecting Multiple Systems

MeetMate involved multiple independent components:

```text
Google Meet
     ↕
Chrome / Playwright
     ↕
Python / Flask
     ↕
RAG Pipeline
     ↕
ChromaDB
     ↕
LLM
     ↕
Text-to-Speech
```

Making these components work together reliably was a significant part of the engineering challenge.

---

# Technology

**Languages**

* Python
* JavaScript/TypeScript where applicable

**Backend**

* Flask

**Browser Automation**

* Playwright
* Chrome

**AI**

* LLM
* Embeddings
* Retrieval-Augmented Generation (RAG)
* Text-to-Speech

**Vector Database**

* ChromaDB

**Integrations**

* Google Meet
* Jira
* Confluence
* Google Docs

---

# What I Learned

Working on MeetMate gave me practical experience building an AI system that operates in a **real-time environment rather than a simple request/response application**.

Key areas I worked with included:

* Real-time meeting interaction
* Browser automation
* Audio processing and injection
* Session lifecycle debugging
* RAG architecture
* Document chunking
* Embedding generation
* Vector search
* Context retrieval
* LLM-based response generation
* Text-to-speech pipelines
* Flask API development
* Integrating multiple independent systems

The project particularly strengthened my understanding of how individual AI components need to be connected into a reliable end-to-end system.

---

# Project Impact

MeetMate explored a different model for meeting assistants:

> **From an AI that records meetings → to an AI that understands, retrieves, and participates in meetings.**

The combination of real-time trigger detection, contextual retrieval, LLM reasoning, text-to-speech, and browser-based audio injection created the foundation for an interactive AI meeting participant.

---

## My Contribution at a Glance

| Area            | Contribution                                                     |
| --------------- | ---------------------------------------------------------------- |
| Google Meet     | Worked on interactive bot functionality                          |
| Playwright      | Integrated browser/session-level meeting functionality           |
| Audio           | Implemented generated-audio injection into the meeting session   |
| RAG             | Built the extraction → chunking → embedding → retrieval pipeline |
| ChromaDB        | Implemented embedding storage and retrieval                      |
| Flask           | Developed text-to-speech endpoints                               |
| AI              | Connected retrieved meeting context with LLM responses           |
| Trigger System  | Implemented monitoring for assistant trigger words               |
| Meeting Context | Enabled responses based on conversation context                  |
| Summary         | Supported end-of-meeting summary generation                      |
| Integrations    | Designed for Jira, Confluence and Google Docs context            |

---

## Portfolio Takeaway

**MeetMate demonstrates my experience building AI systems that combine real-time data processing, RAG, vector databases, browser automation, APIs, and LLMs into a single end-to-end product.**

Rather than only integrating an LLM API, my work involved solving the underlying engineering problems required to make an AI assistant operate inside a live communication environment.
