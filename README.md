# Multimodal Vision Analyzer (Llama-3.2 Vision)

A web-based multimodal image understanding demo that uses a Vision-Language Model (VLM) to analyze and interpret images in natural language — built with Gradio and OpenRouter, runnable directly in Google Colab.

## Overview

This project demonstrates a multimodal AI pipeline: a user uploads an image through a simple web UI, and a vision-language model analyzes the image content, identifying objects, brands, text, and contextual details, then returns a natural-language description grounded in what it actually sees.

It was built as an extension of an existing text-based RAG pipeline, applying the same "ground answers in retrieved/observed evidence" philosophy to the visual domain.

## Features

- **Web UI in Colab**: Gradio-based interface generates a shareable public link — no local setup required, just upload and analyze
- **Vision-Language Model Integration**: Calls `meta-llama/llama-3.2-11b-vision-instruct` via OpenRouter API
- **Custom Prompting**: Users can ask targeted questions (e.g. "identify the car model and any visible sponsor logos") instead of relying on a generic description
- **Image Preprocessing**: Automatic RGB conversion and Base64 encoding to handle various image formats (PNG, JPEG, etc.)
- **Error Handling**: Graceful fallback messages for missing images or failed API calls

## Tech Stack

| Component | Tool |
|---|---|
| Web Interface | Gradio |
| Vision-Language Model | Llama-3.2-11B-Vision-Instruct (via OpenRouter) |
| Image Processing | Pillow (PIL) |
| Environment | Google Colab |

## How It Works

1. User uploads an image through the Gradio interface
2. The image is converted to RGB and encoded as Base64
3. The encoded image, along with a text prompt (custom or default), is sent to the vision-language model via OpenRouter's chat completions API
4. The model returns a structured natural-language analysis of the image
5. Results are displayed directly in the web UI

## Example Results

### Example 1: Modified Sports Car (F1/Motorsport-adjacent)

**Prompt:** *"Identify the exact model of this car. Does it have any specific racing modifications visible, such as a wide-body kit, a GT wing, aftermarket wheels, or a roll cage? Look closely at the livery and stickers on the car. Can you recognize any brand logos, numbers, or sponsor text written on the car body?"*

**Result:** The model correctly identified the car as a Nissan Silvia S15, accurately described visible racing modifications (wide-body kit, GT wing, aftermarket wheels, roll cage), and extracted over a dozen sponsor/brand logos directly from the livery — including Liberty Walk, ADVAN, Toyota, RAYS Engineering, HKS, and Nissan — demonstrating strong fine-grained visual text recognition (OCR-like capability) combined with domain knowledge.

### Example 2: Football Match Broadcast Frame

**Prompt:** *"Describe the current scanning scene. Which teams are playing based on their jersey colors, and what is the current match situation (e.g., attacking, defending, corner kick)?"*

**Result:** The model correctly read the on-screen scoreboard (NOR vs ISR, 0-0) and matched jersey colors to each team. It accurately reported the scoreless outcome, but the in-play action description (attacking/defending/corner kick) was vague and partially generic — highlighting a current limitation: the model reasons well from clearly rendered on-screen text (scoreboards, jersey numbers) but is less reliable at inferring dynamic tactical context from a single static frame.

## Key Takeaways

- The model performs strongly on tasks combining **visual text extraction** (logos, scoreboards, livery text) with **object/scene recognition** — a core capability for sports and motorsport media analysis use cases.
- Performance varies with prompt specificity: targeted, detail-oriented prompts (Example 1) produced significantly more accurate and structured output than open-ended scene description (Example 2).
- Single-frame static images have inherent limits for inferring dynamic context (e.g. live match state) — a natural next step would be frame-sequence or video-based analysis.

## Relevant Skills Demonstrated

Multimodal AI · Vision-Language Models (VLM) · Visual Understanding · Image-to-Text · Prompt Engineering for Visual Tasks

## Setup

```bash
pip install gradio requests
```

Set your OpenRouter API key as a Colab secret (`OPENROUTER_API_KEY`).

## Future Improvements

- [ ] Benchmark against other VLMs (e.g. GPT-4o-mini, Qwen2-VL) for accuracy comparison on sports imagery
- [ ] Extend to video frame sequences for temporal context (e.g. tracking play development over several frames)
- [ ] Add structured JSON output mode (team names, score, detected objects) for downstream integration
- [ ] Combine with the existing RAG pipeline for a unified multimodal Q&A system (text + image retrieval)
