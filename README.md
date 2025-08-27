# Genimate

Welcome to Genimate! This project uses Large Language Models (LLMs) to generate Manim animations from natural language descriptions. It features a web interface and a backend service (`manimgen`) that handles the animation generation.

This document provides an overview of the project and the key LLM concepts used in the `manimgen` service.

## Prompting Techniques

### Zero-shot Prompting
**How it's used:** The `manimgen` service does not provide the LLM with examples of Manim code in the prompts. Instead, it relies on the model's pre-existing knowledge to generate the animation. This is evident in the `_generate_manim_code` function in `src/rendering/generator.py`, which constructs a prompt from the user's description and sends it directly to the LLM without any example code.

### Dynamic Prompting
**How it's used:** Prompts are built dynamically using templates defined in `config/prompts.json`. For example, the `basic_animation` template is: `"Create a Manim animation that demonstrates {concept}."`. The `ManimPrompt.create_from_description` function takes the user's input from the API and inserts it into these templates, creating a specific, dynamic prompt for each request.
