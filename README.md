![Firebase Genkit + Hugging Face Models](https://github.com/xavidop/genkitx-huggingface/blob/main/assets/genkit-huggingface.png?raw=true)

<h1 align="center">
   Firebase Genkit <> Hugging Face Models Plugin
</h1>

<h4 align="center">Hugging Face Models Community Plugin for Google Firebase Genkit</h4>

<div align="center">
   <img alt="GitHub version" src="https://img.shields.io/github/v/release/xavidop/genkitx-huggingface">
   <img alt="NPM Downloads" src="https://img.shields.io/npm/dw/genkitx-huggingface">
   <img alt="GitHub License" src="https://img.shields.io/github/license/xavidop/genkitx-huggingface">
   <img alt="Static Badge" src="https://img.shields.io/badge/yes-a?label=maintained">
</div>

<div align="center">
   <img alt="GitHub Issues or Pull Requests" src="https://img.shields.io/github/issues/xavidop/genkitx-huggingface?color=blue">
   <img alt="GitHub Issues or Pull Requests" src="https://img.shields.io/github/issues-pr/xavidop/genkitx-huggingface?color=blue">
   <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/m/xavidop/genkitx-huggingface">
</div>

</br>

**`genkitx-huggingface`** is a community plugin for using Hugging Face Models APIs with
[Firebase Genkit](https://github.com/firebase/genkit). Built by [**Xavier Portilla Edo**](https://github.com/xavidop).

This Genkit plugin allows to use Hugging Face models through their official APIs.

## Installation

Install the plugin in your project with your favorite package manager:

- `npm install genkitx-huggingface`
- `pnpm add genkitx-huggingface`    

## Usage

### Configuration

To use the plugin, you need to configure it with your Hugging Face Token key. You can do this by calling the `genkit` function:

```typescript
import { genkit, z } from 'genkit';
import { huggingface, openAIGpt4o } from "genkitx-huggingface";

const ai = genkit({
  plugins: [
    huggingface({
      huggingfaceToken: '<my-huggingface-token>',
    }),
    openAIGpt4o,
  ]
});
```

You can also initialize the plugin in this way if you have set the `HUGGINGFACE_TOKEN` environment variable:

```typescript
import { genkit, z } from 'genkit';
import { huggingface, openAIGpt4o } from "genkitx-huggingface";

const ai = genkit({
  plugins: [
    huggingface({
      huggingfaceToken: '<my-huggingface-token>',
    }),
    openAIGpt4o,
  ]
});
```

### Basic examples

The simplest way to call the text generation model is by using the helper function `generate`:

```typescript
import { genkit, z } from 'genkit';
import { huggingface, openAIGpt4o } from "genkitx-huggingface";

// Basic usage of an LLM
const response = await ai.generate({
  prompt: 'Tell me a joke.',
});

console.log(await response.text);
```

### Within a flow

```typescript
// ...configure Genkit (as shown above)...

export const myFlow = ai.defineFlow(
  {
    name: 'menuSuggestionFlow',
    inputSchema: z.string(),
    outputSchema: z.string(),
  },
  async (subject) => {
    const llmResponse = await ai.generate({
      prompt: `Suggest an item for the menu of a ${subject} themed restaurant`,
    });

    return llmResponse.text;
  }
);
```

### Tool use

```typescript
// ...configure Genkit (as shown above)...

const specialToolInputSchema = z.object({ meal: z.enum(["breakfast", "lunch", "dinner"]) });
const specialTool = ai.defineTool(
  {
    name: "specialTool",
    description: "Retrieves today's special for the given meal",
    inputSchema: specialToolInputSchema,
    outputSchema: z.string(),
  },
  async ({ meal }): Promise<string> => {
    // Retrieve up-to-date information and return it. Here, we just return a
    // fixed value.
    return "Baked beans on toast";
  }
);

const result = ai.generate({
  tools: [specialTool],
  prompt: "What's for breakfast?",
});

console.log(result.then((res) => res.text));
```

For more detailed examples and the explanation of other functionalities, refer to the [official Genkit documentation](https://firebase.google.com/docs/genkit/get-started).

## Supported models

This plugin supports all currently available **Chat/Completion** and **Embeddings** models from Hugging Face Models. This plugin supports image input and multimodal models.

## API Reference

This plugin supports all Hugging Face models available in the [Inference Providers on the Hub](https://huggingface.co/blog/inference-providers).

## Contributing

Want to contribute to the project? That's awesome! Head over to our [Contribution Guidelines](https://github.com/xavidop/genkitx-huggingface/blob/main/CONTRIBUTING.md).

## Need support?

> [!NOTE]  
> This repository depends on Google's Firebase Genkit. For issues and questions related to Genkit, please refer to instructions available in [Genkit's repository](https://github.com/firebase/genkit).

Reach out by opening a discussion on [GitHub Discussions](https://github.com/xavidop/genkitx-huggingface/discussions).

## Credits

This plugin is proudly maintained by Xavier Portilla Edo [**Xavier Portilla Edo**](https://github.com/xavidop).

I got the inspiration, structure and patterns to create this plugin from the [Genkit Community Plugins](https://github.com/TheFireCo/genkit-plugins) repository built by the [Fire Compnay](https://github.com/TheFireCo) as well as the [ollama plugin](https://firebase.google.com/docs/genkit/plugins/ollama).

## License

This project is licensed under the [Apache 2.0 License](https://github.com/xavidop/genkitx-huggingface/blob/main/LICENSE).

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202%2E0-lightgrey.svg)](https://github.com/xavidop/genkitx-huggingface/blob/main/LICENSE)
