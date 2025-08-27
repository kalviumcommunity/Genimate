# Genimate

Welcome to Genimate! This project uses Large Language Models (LLMs) to generate Manim animations from natural language descriptions. It features a web interface and a backend service (`manimgen`) that handles the animation generation.

This document provides an overview of the project and the key LLM concepts used in the `manimgen` service.

## Prompting Techniques

### Zero-shot Prompting
**How it's used:** The `manimgen` service does not provide the LLM with examples of Manim code in the prompts. Instead, it relies on the model's pre-existing knowledge to generate the animation. This is evident in the `_generate_manim_code` function in `src/rendering/generator.py`, which constructs a prompt from the user's description and sends it directly to the LLM without any example code.

### Dynamic Prompting
**How it's used:** Prompts are built dynamically using templates defined in `config/prompts.json`. For example, the `basic_animation` template is: `"Create a Manim animation that demonstrates {concept}."`. The `ManimPrompt.create_from_description` function takes the user's input from the API and inserts it into these templates, creating a specific, dynamic prompt for each request.

## Key Concepts in LLMs

### Tokens and Tokenization
**How it's used:** The service monitors LLM usage by tracking the number of tokens processed. The `TokenLogger` class is initialized in the `ManimGenerator` (`src/rendering/generator.py`). After each call to the LLM, the `log_request` method is invoked to record the number of tokens used, which is useful for monitoring costs and performance.

### Temperature
**How it's used:** The `temperature` is a user-configurable parameter that controls the creativity of the generated code. The `/api/v1/generate` endpoint in `src/api/v1/endpoints.py` accepts a `temperature` value in the request body. This value is then passed to the LLM provider in the `_generate_manim_code` function, allowing users to choose between more deterministic or more creative outputs.

### Top P
**How it's used:** Top P, also known as nucleus sampling, is a method for controlling the randomness of the LLM's output. It works by selecting from the smallest set of tokens whose cumulative probability exceeds a certain threshold (p). This allows for more dynamic control over the number of tokens considered at each step. In the `manimgen` service, `top_p` can be configured in the `/api/v1/generate` endpoint. If not provided, a default value from `settings.py` is used.

### Top K
**How it's used:** Top K is another method for controlling the randomness of the LLM's output. It works by restricting the model to select from a fixed number (k) of the most likely next tokens. A value of 0 for `top_k` disables this feature. In the `manimgen` service, `top_k` can be configured in the `/api/v1/generate` endpoint. If not provided, a default value from `settings.py` is used.




