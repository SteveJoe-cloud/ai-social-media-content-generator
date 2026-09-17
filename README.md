# AI Social Media Content Generator (Qwen)

**Sector:** General, internal use
**Technique:** Two-pass LLM generation, self-review pass before output, single input driving five platform-specific formats
**Built:** September 2026
**Status:** Complete

## Cost solved

Writing separate posts for Twitter, LinkedIn, Reddit, and Instagram from scratch for every piece of content, each with a different length, tone, and format expectation.

## How it works

1. A manual trigger starts the workflow with three parameters: topic, brand voice, target audience
2. Pass 1 sends these to Qwen (via DashScope's OpenAI-compatible endpoint) with a prompt requesting all five platform formats back as a single JSON object: Twitter (under 280 characters with hashtags), LinkedIn (3-4 professional paragraphs), Reddit title and body, and an Instagram caption
3. A Code node parses the JSON out of the model's response, handling cases where it wraps the output in markdown fences anyway
4. Pass 2 sends the generated content back to Qwen as a self-review pass, checking grammar, tone consistency with the stated brand voice, and platform-appropriate formatting, returning corrected versions where needed
5. A final Code node merges the reviewed content with the original, tagging whether the review pass actually ran

## Stack

n8n (self-hosted), Qwen via DashScope (HTTP Request node, OpenAI-compatible chat completions endpoint)

## Result

Drives the DAASK content calendar directly. One topic in, five platform-ready drafts out, with a built-in quality pass rather than a single ungraded generation.

## Reuse notes

The brand voice and target audience are currently hardcoded in the Content Parameters node for repeatable output. Worth exposing as workflow inputs if this gets reused across more than one brand voice.
