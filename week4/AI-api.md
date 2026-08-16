# AI APIs

> **Note:** AI model names, APIs, capabilities, prices, and limits change frequently. This lecture explains general concepts, but you should always check the current provider documentation before building an application.

## ChatGPT, Gemini and others, what are they

### AI applications, models, APIs, and platforms

It is useful to separate a few related concepts:

- An **AI application** is a ready-to-use product for end users, such as [ChatGPT](https://chatgpt.com/) or the [Gemini app](https://gemini.google.com/).
- An **AI model** is the underlying system that generates or analyzes content, such as OpenAI GPT models or Google's Gemini models.
- An **API** lets developers send requests to a model or AI service from their own application.
- An **AI platform** provides APIs together with developer tools, security, deployment, monitoring, and integrations. Examples include the [OpenAI developer platform](https://platform.openai.com/docs/overview) and [Google Cloud Vertex AI](https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform).

An AI application may use several models and services behind the scenes. The product name and the model name are not always the same thing.

### Large Language Models (LLMs)

Large language models (LLMs) are AI models trained on large amounts of data. They learn patterns in language and other content and can generate or analyze text, code, images, audio, and files depending on the model.

Modern AI applications are often built from several components, not just one model. For example, an application might combine a language model, retrieval from documents, safety checks, speech processing, image generation, and external tools.

### Transformers Architecture

Many modern generative AI systems are based on the [transformer](https://aws.amazon.com/what-is/transformer-model/) architecture, which processes relationships between tokens in a sequence. During training, a language model learns to predict likely next tokens. Later it can be adapted to follow instructions, answer questions, summarize information, and perform other tasks.

This does not mean that the model simply stores fixed answers. Instead, it generates a response from patterns learned during training and from the context provided in the current request.

### ChatGPT and OpenAI

[ChatGPT](https://openai.com/chatgpt/overview/) is OpenAI's AI assistant application. It is not a single model and it is not limited to text-only conversation. Depending on the product surface, account type, region, and selected model, ChatGPT and OpenAI services may support:

- text and code generation
- image understanding and image generation
- file and document analysis
- audio input and speech output
- structured output such as JSON
- tool or function calling
- coding assistance
- real-time interaction

The [OpenAI API](https://platform.openai.com/docs/overview) gives developers access to many model capabilities from their own applications. Available features depend on the selected model and API, so it is better to describe capabilities at a high level than to memorize a fixed list of model version numbers.

### Gemini and Google

[Gemini](https://deepmind.google/technologies/gemini/) refers both to Google's family of generative AI models and to Google's assistant product. Gemini is not merely "part of Vertex AI", although Gemini models are available there.

Developers can access Gemini models through:

- the [Gemini API](https://ai.google.dev/gemini-api/docs) and [Google AI Studio](https://aistudio.google.com/)
- [Google Cloud Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/overview)
- Google products that integrate Gemini

Depending on the model, Gemini can support text, images, audio, video, and code. Some models are optimized for quality and complex tasks, while others are designed for speed, lower cost, or high-volume use.

### Comparing ChatGPT/OpenAI and Gemini/Google

It is no longer accurate to say that ChatGPT is text-focused while Gemini is multimodal. Both ecosystems offer multimodal models and developer APIs.

More meaningful comparison points include:

- available models and model families
- supported input and output modalities
- context-window limits
- reasoning and tool-use capabilities
- structured outputs and JSON support
- embeddings and retrieval workflows
- image, audio, speech, video, and real-time features
- pricing, quotas, and latency
- safety features, privacy options, and data-retention rules
- cloud, product, and enterprise integrations

These differences change frequently. A useful comparison should therefore include the date, model name, API version, and links to the provider documentation.

### Other LLMs

Other important model providers and ecosystems include:

- [Meta's Llama](https://www.llama.com/)
- [Anthropic's Claude](https://www.anthropic.com/claude)
- [Mistral](https://mistral.ai/)
- [Cohere](https://cohere.com/)
- [DeepSeek](https://www.deepseek.com/)
- [Qwen](https://qwenlm.github.io/)

Some models are available only through hosted APIs, while others can also be downloaded or self-hosted. Terms such as **open source**, **open weight**, and **open model** are not interchangeable, so you should always check the actual license and distribution terms.

## AI APIs for Developers

AI APIs let developers use models without training them from scratch. They still behave like other APIs: your application sends a request, the service processes it, and your application receives a response.

Common AI API features include:

1. **Generation and analysis**: text, code, and multimodal responses.
2. **Streaming**: receiving the response gradually instead of waiting for the whole result at once.
3. **Structured outputs**: returning JSON or another defined schema instead of free-form text.
4. **Tool or function calling**: letting the model request that your application call a predefined function, database, or external service.
5. **Embeddings**: converting content into numeric vectors for semantic search, recommendations, or retrieval.
6. **File and document processing**: sending files, PDFs, images, or other documents for analysis.
7. **Speech, audio, and image generation**: converting between text, speech, and images where supported.
8. **Moderation and safety**: checking content for policy or safety issues.
9. **Retrieval-augmented generation (RAG)**: supplying relevant information from files, search indexes, or databases.
10. **Real-time interaction**: low-latency voice or multimodal applications.

An AI model does not automatically know current events or your private data. If your application needs current or private information, it usually has to provide that information through retrieval, files, databases, or tools.

When using AI APIs:

- keep API keys on the server side, not in frontend code
- validate model output before using it in your application
- handle rate limits, timeouts, and API errors
- avoid sending sensitive personal or confidential data unless you have checked the provider's privacy and retention rules
- remember that the same prompt may produce different results in different runs

Some useful APIs and platforms:

- [OpenAI API](https://platform.openai.com/docs/overview)
- [Gemini API](https://ai.google.dev/gemini-api/docs)
- [Google Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/overview)

## Terminology

- **Prompt**: The input sent to the model. A prompt may include instructions, examples, user content, system or developer guidance, files, images, audio, or structured data.
- **Generation / response**: The output produced by the model. **Completion** is an older term that is still used by some APIs.
- **Fine-tuning**: Additional training of an existing model on a task-specific dataset. It is different from prompting, retrieval, or giving a few examples inside one request.
- **Temperature**: A sampling parameter that can affect how varied the output is. Its range, meaning, and availability depend on the model and API.
- **Token**: A unit used by a model to represent text. A token is not the same thing as a word: it may be a full word, part of a word, punctuation, or whitespace. Input and output tokens may be counted separately.
- **Context window**: The amount of text, files, or other content that the model can consider in one request or conversation. A context window is not the same as long-term memory.
- **Multimodal model**: A model that can process more than one type of data, such as text, images, audio, or video.
- **Embedding**: A numeric representation of content that can be used for similarity search, classification, recommendations, or retrieval.
- **RAG (retrieval-augmented generation)**: A technique where an application retrieves relevant information from an external source and includes it in the model request.
- **Tool calling / function calling**: A capability where the model asks the application to run a predefined function or external tool.
- **Structured output**: A response constrained to a format such as JSON or a defined schema.
- **Reasoning model**: A model or model mode designed for more complex multi-step tasks, often with different controls or latency characteristics.
- **Agent**: An application that combines a model with instructions, memory, tools, and a control loop to perform multiple steps.

## Assignment 1: Comment Generator

Generate a response to a YouTube comment. Try different prompts and observe how the model responds. Ask for different tones or styles, such as friendly, funny, formal, sarcastic, or professional. Prompts can influence tone, but they do not guarantee an exact result, so you should test the output and judge whether it is suitable.

- Clone [this repo](https://github.com/ilkkamtk/AI-commenter-starter) to get started.
- Instead of using the OpenAI library, use `fetchData` to make a POST request to the server.
- The course exercise may still use the [Chat Completions API](https://platform.openai.com/docs/api-reference/chat/create) as a simple or compatibility-focused example. In newer OpenAI applications, the [Responses API](https://platform.openai.com/docs/api-reference/responses) is often the newer general-purpose API.
- Do not put an API key in frontend JavaScript. Keep the key and the API call on the server side.
- If you use `OPENAI_API_URL` from the course material, treat it as a **course-specific proxy URL**, not as a general requirement of the OpenAI API.
  - Example: instead of `https://api.openai.com/v1/chat/completions`, use `new URL('/v1/chat/completions', process.env.OPENAI_API_URL).toString()`
  - The value for `OPENAI_API_URL` is in Oma/assignments.
- Use Postman for testing.
- Before displaying or publishing generated content, think about moderation, safety, and whether the response is appropriate for the user and the context.

## Assignment 2: Image generator

Generate a YouTube thumbnail image using an AI image API. The thumbnail should relate to a video topic. For example, a space video thumbnail might include stars, planets, astronauts, or splash text such as "Explore the Universe!"

You can use the same starter code as in the previous assignment. [Here is an example](https://github.com/ilkkamtk/AI-BE/blob/main/src/middlewares.ts#L49) of generating an image and saving the result to a file.

Keep in mind:

- Image capabilities are model-dependent. Some APIs support generation, some support editing, some support variations, and some support only part of that workflow.
- Image output may be returned as a URL, as Base64-encoded data, or in another provider-specific format.
- Prompting helps guide the image, but it does not give pixel-perfect control.
- Different providers may use different models for text generation and image generation.
- Check generated images for copyright, branding, misleading content, and suitability before using them.
- API endpoints and model support can change, so always verify the current documentation.

Useful documentation:

- [Create image](https://platform.openai.com/docs/api-reference/images/create)
- [Create image edit](https://platform.openai.com/docs/api-reference/images/createEdit)
- [Create image variation](https://platform.openai.com/docs/api-reference/images/createVariation)

Use Postman for testing.
