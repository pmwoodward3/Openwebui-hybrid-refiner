# Open WebUI: Hybrid Refiner Pipeline

A generator-critic architecture for Open WebUI that bridges the gap between fast local hosting and high-reasoning cloud intelligence. This function allows you to use local models for primary generation while leveraging cloud models for an automated "senior developer" review. By shifting the bulk of the token generation to the local GPU and using the cloud only for auditing, you significantly reduce API costs and data exposure.



## Overview

The Hybrid Refiner acts as an automated quality control layer on the outgoing chat message. It is designed for developers who want the speed of local hardware without sacrificing the architectural reliability of state-of-the-art models.

1. **Local Generation:** Your primary local model (e.g., Qwen-Coder or DeepSeek-R1) writes the initial response.
2. **Interception:** This function captures the draft before it reaches the user interface.
3. **Automated Audit:** The draft is sent to a specified "Refiner" model (e.g., Gemini 1.5 Pro) for a logic and safety check.
4. **Correction:** If errors are found, the Refiner rewrites the code block. If the code is sound, the Refiner signs off on the draft.

## Features

- **Performance Dashboard:** A collapsible section providing metrics on cloud latency and character-per-second throughput.
- **Context Preservation:** The local model's original conversational text is preserved in a collapsible "Original Context" section, ensuring you don't lose setup instructions or metadata.
- **Peer-Review Grading:** The Refiner provides a grade for both the original draft and the final version, making the "IQ gap" between models transparent.
- **Real-Time UI Feedback:** Integrated event emitters show a "Waiting for Refiner" status pulse during the cloud API call.

## Installation

### 1. Configure Open WebUI
To prevent deadlocks during the internal API call, you must increase the worker count in your environment. Add the following to your `.env` or `docker-compose.yml`:

WEB_CONCURRENCY=4

### 2. Add the Function ###
Navigate to Admin Panel > Functions in Open WebUI.

Create a new function and paste the hybrid_refiner.py script.

In the function Valves, configure your:

OPENWEBUI_API_KEY: A persistent API key from Settings > Account.

REVIEW_MODEL_ID: The internal ID for your cloud model (e.g., models/gemini-pro-latest).
