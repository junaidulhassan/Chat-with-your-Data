# RAGs



https://github.com/run-llama/rags/assets/4858925/a6204550-b3d1-4cde-b308-8d944e5d3058



RAGs is a Streamlit app that lets you create a RAG pipeline from a data source using natural language.

You get to do the following:
1. Describe your task (e.g. "load this web page") and the parameters you want from your RAG systems (e.g. "i want to retrieve X number of docs")
2. Go into the config view and view/alter generated parameters (top-k, summarization, etc.) as needed.
3. Query the RAG agent over data with your questions.

This project is inspired by [GPTs](https://openai.com/blog/introducing-gpts), launched by OpenAI.

## Installation and Setup 

Clone this project, go into the `rags` project folder. We recommend creating a virtual env for dependencies (`python3 -m venv .venv`).

```
poetry install --with dev
```

By default, we use OpenAI for both the builder agent as well as the generated RAG agent.
Add `.streamlit/secrets.toml` in the home folder.

Then put the following:
```
openai_key = "<openai_key>"
```


Then run the app from the "home page" file.

```

streamlit run 1_🏠_Home.py

```

**NOTE**: If you've upgraded the version of RAGs, and you're running into issues on launch, you may need to delete the `cache` folder in your home directory (we may have introduced breaking changes in the stored data structure between versions).

## Detailed Overview

The app contains the following sections, corresponding to the steps listed above.

### 1. 🏠 Home Page
This is the section where you build a RAG pipeline by instructing the "builder agent". Typically to setup a RAG pipeline you need the following components:
1. Describe the dataset. Currently, we support either **a single local file** or a **web page**. We're open to suggestions here! 
2. Describe the task. Concretely this description will be used to initialize the "system prompt" of the LLM powering the RAG pipeline.
3. Define the typical parameters for a RAG setup. See the below section for the list of parameters.

### 2. ⚙️ RAG Config

This section contains the RAG parameters, generated by the "builder agent" in the previous section. In this section, you have a UI showcasing the generated parameters and have full freedom to manually edit/change them as necessary.

Currently, the set of parameters is as follows:
- System Prompt
- Include Summarization: whether to also add a summarization tool (instead of only doing top-k retrieval.)
- Top-K
- Chunk Size
- Embed Model
- LLM 

If you manually change parameters, you can press the "Update Agent" button in order to update the agent.

```{tip}
If you don't see the `Update Agent` button, that's because you haven't created the agent yet. Please go to the previous "Home" page and complete the setup process.
```

We can always add more parameters to make this more "advanced" 🛠️, but thought this would be a good place to start.

### 3. Generated RAG Agent

Once your RAG agent is created, you have access to this page.

This is a standard chatbot interface where you can query the RAG agent and it will answer questions over your data.

It will be able to pick the right RAG tools (either top-k vector search or optionally summarization) in order to fulfill the query.


## Supported LLMs and Embeddings

### Builder Agent

By default the builder agent uses OpenAI. This is defined in the `core/builder_config.py` file.

You can customize this to whatever LLM you want (an example is provided for Anthropic).

Note that GPT-4 variants will give the most reliable results in terms of actually constructing an agent (we couldn't get Claude to work).

### Generated RAG Agent

You can set the configuration either through natural language or manually for both the embedding model and LLM.

- **LLM**: We support the following LLMs, but you need to explicitly specify the ID to the builder agent.
    - OpenAI: ID is "openai:<model_name>" e.g. "openai:gpt-4-1106-preview"
    - Anthropic: ID is "anthropic:<model_name>" e.g. "anthropic:claude-2"
    - Replicate: ID is "replicate:<model_name>"
    - HuggingFace: ID is "local:<model_name>" e.g. "local:BAAI/bge-small-en"
- **Embeddings**: Supports text-embedding-ada-002 by default, but also supports Hugging Face models. To use a hugging face model simply prepend with local, e.g. local:BAAI/bge-small-en.


## Resources

Running into issues? Please file a GitHub issue or join our [Discord](https://discord.gg/dGcwcsnxhU).

This app was built with [LlamaIndex Python](https://github.com/run-llama/llama_index).

See our launch blog post [here](https://blog.llamaindex.ai/introducing-rags-your-personalized-chatgpt-experience-over-your-data-2b9d140769b1).
