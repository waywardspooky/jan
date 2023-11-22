---
title: Models
---

:::caution

Draft Specification: functionality has not been implemented yet.

:::

## Overview

In Jan, models are primary entities with the following capabilities:

- Users can import, configure, and run models locally.
- An [OpenAI Model API](https://platform.openai.com/docs/api-reference/models) compatible endpoint at `localhost:3000/v1/models`.
- Supported model formats: `ggufv3`, and more.

## Folder Structure

- Models are stored in the `/models` folder.
- Models are organized by individual folders, each containing the binaries and configurations needed to run the model. This makes for easy packaging and sharing.
- Model folder names are unique and used as `model_id` default values.

```bash
jan/                               # Jan root folder
  models/
    llama2-70b-q4_k_m/             # Example: standard GGUF model
        model.json
        model-binary-1.gguf
    mistral-7b-gguf-q3_k_l/        # Example: quantizations are separate folders
        model.json
        mistral-7b-q3-K-L.gguf
    mistral-7b-gguf-q8_k_m/        # Example: quantizations are separate folders
        model.json
        mistral-7b-q8_k_k.gguf
    llava-ggml-Q5/                 # Example: model with many partitions
        model.json
        mmprj.bin
        model_q5.ggml
```

## `model.json`

- Each `model` folder contains a `model.json` file, which is a representation of a model.
- `model.json` contains metadata and default parameters used to run a model.
- The only required field is `source_url`.

### GGUF Example

Here's a standard example `model.json` for a GGUF model.

- `source_url`: https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF/.

```json
"source_url": "https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF/blob/main/zephyr-7b-beta.Q4_K_M.gguf",
"type": "model",                    // Defaults to "model"
"version": "1",                     // Defaults to 1
"id": "zephyr-7b"                   // Defaults to foldername
"name": "Zephyr 7B"                 // Defaults to foldername
"owned_by": "you"                   // Defaults to you
"created": 1231231                  // Defaults to file creation time
"description": ""
"state": enum[null, "downloading", "ready", "starting", "stopping", ...]
"format": "ggufv3",                 // Defaults to "ggufv3"
"settings": {                       // Models are initialized with these settings
    "ctx_len": "2048",
    "ngl": "100",
    "embedding": "true",
    "n_parallel": "4",
    // KIV: "pre_prompt": "A chat between a curious user and an artificial intelligence",
    // KIV:"user_prompt": "USER: ",
    // KIV: "ai_prompt": "ASSISTANT: "
}
"parameters": {                     // Models are called with these parameters
    "temperature": "0.7",
    "token_limit": "2048",
    "top_k": "0",
    "top_p": "1",
    "stream": "true"
},
"metadata": {}                    // Defaults to {}
"assets": [                       // Filepaths to model binaries; Defaults to current dir
    "file://.../zephyr-7b-q4_k_m.bin",
]
```

## API Reference

Jan's Model API is compatible with [OpenAI's Models API](https://platform.openai.com/docs/api-reference/models), with additional methods for managing and running models locally.

See [Jan Models API](https://jan.ai/api-reference#tag/Models)

## Importing Models

:::caution

This is current under development.

:::

You can import a model by dragging the model binary or gguf file into the `/models` folder.

- Jan automatically generates a corresponding `model.json` file based on the binary filename.
- Jan automatically organizes it into its own `/models/model-id` folder.
- Jan automatically populates the `model.json` properties, which you can subsequently modify.