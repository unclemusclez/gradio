# Chatinterface Examples

**Gradio Agents & MCP Hackathon · Virtual, June 2-8 · $10k+ in prizes**

Register Now →

  ⚡ Quickstart ✍️ Docs 🎢 Playground 🖼️ Custom Components

🖐 Community

Search Search

⌘K

Getting Started

Quickstart

Building Interfaces

The Interface ClassMore On ExamplesFlaggingInterface StateReactive InterfacesFour Kinds Of Interfaces

Building With Blocks

Blocks And Event ListenersControlling LayoutState In BlocksDynamic Apps With Render DecoratorMore Blocks FeaturesCustom CSS And JSUsing Blocks Like Functions

Additional Features

QueuingStreaming OutputsStreaming InputsAlertsProgress BarsBatch FunctionsSharing Your AppFile AccessMultipage AppsEnvironment VariablesResource CleanupThemesClient Side FunctionsView Api PageInternationalization

Chatbots

Creating A Chatbot FastChatinterface ExamplesAgents And Tool UsageCreating A Custom Chatbot With BlocksChatbot Specific EventsCreating A Discord Bot From A Gradio AppCreating A Slack Bot From A Gradio AppCreating A Website Widget From A Gradio Chatbot

Data Science And Plots

Creating PlotsTime PlotsFilters Tables And StatsConnecting To A Database

Streaming

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

5.33.04.44.1main

Getting Started

Quickstart

Building Interfaces

The Interface ClassMore On ExamplesFlaggingInterface StateReactive InterfacesFour Kinds Of Interfaces

Building With Blocks

Blocks And Event ListenersControlling LayoutState In BlocksDynamic Apps With Render DecoratorMore Blocks FeaturesCustom CSS And JSUsing Blocks Like Functions

Additional Features

QueuingStreaming OutputsStreaming InputsAlertsProgress BarsBatch FunctionsSharing Your AppFile AccessMultipage AppsEnvironment VariablesResource CleanupThemesClient Side FunctionsView Api PageInternationalization

Chatbots

Creating A Chatbot FastChatinterface Examples

Llama IndexLangChainOpenAIHugging Face transformersSambaNovaHyperbolicAnthropic's Claude

Agents And Tool UsageCreating A Custom Chatbot With BlocksChatbot Specific EventsCreating A Discord Bot From A Gradio AppCreating A Slack Bot From A Gradio AppCreating A Website Widget From A Gradio Chatbot

Data Science And Plots

Creating PlotsTime PlotsFilters Tables And StatsConnecting To A Database

Streaming

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Chatbots
2. Chatinterface Examples

[←

Creating A Chatbot Fast](../guides/creating-a-chatbot-fast/) [Agents And Tool Usage

→](../guides/agents-and-tool-usage/)

# Using Popular LLM libraries and APIs

In this Guide, we go through several examples of how to use `gr.ChatInterface` with popular LLM libraries and API providers.

We will cover the following libraries and API providers:

* Llama Index
* LangChain
* OpenAI
* Hugging Face `transformers`
* SambaNova
* Hyperbolic
* Anthropic's Claude

For many LLM libraries and providers, there exist community-maintained integration libraries that make it even easier to spin up Gradio apps. We reference these libraries in the appropriate sections below.

## Llama Index

Let's start by using `llama-index` on top of `openai` to build a RAG chatbot on any text or PDF files that you can demo and share in less than 30 lines of code. You'll need to have an OpenAI key for this example (keep reading for the free, open-source equivalent!)

```
# This is a simple RAG chatbot built on top of Llama Index and Gradio. It allows you to upload any text or PDF files and ask questions about them!
# Before running this, make sure you have exported your OpenAI API key as an environment variable:
# export OPENAI_API_KEY="your-openai-api-key"

from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
import gradio as gr

def answer(message, history):
    files = []
    for msg in history:
        if msg['role'] == "user" and isinstance(msg['content'], tuple):
            files.append(msg['content'][0])
    for file in message["files"]:
        files.append(file)

    documents = SimpleDirectoryReader(input_files=files).load_data()
    index = VectorStoreIndex.from_documents(documents)
    query_engine = index.as_query_engine()
    return str(query_engine.query(message["text"]))

demo = gr.ChatInterface(
    answer,
    type="messages",
    title="Llama Index RAG Chatbot",
    description="Upload any text or pdf files and ask questions about them!",
    textbox=gr.MultimodalTextbox(file_types=[".pdf", ".txt"]),
    multimodal=True
)

demo.launch()

```

## LangChain

Here's an example using `langchain` on top of `openai` to build a general-purpose chatbot. As before, you'll need to have an OpenAI key for this example.

```
# This is a simple general-purpose chatbot built on top of LangChain and Gradio.
# Before running this, make sure you have exported your OpenAI API key as an environment variable:
# export OPENAI_API_KEY="your-openai-api-key"

from langchain_openai import ChatOpenAI
from langchain.schema import AIMessage, HumanMessage
import gradio as gr

model = ChatOpenAI(model="gpt-4o-mini")

def predict(message, history):
    history_langchain_format = []
    for msg in history:
        if msg['role'] == "user":
            history_langchain_format.append(HumanMessage(content=msg['content']))
        elif msg['role'] == "assistant":
            history_langchain_format.append(AIMessage(content=msg['content']))
    history_langchain_format.append(HumanMessage(content=message))
    gpt_response = model.invoke(history_langchain_format)
    return gpt_response.content

demo = gr.ChatInterface(
    predict,
    type="messages"
)

demo.launch()

```

**Tip:**
For quick prototyping, the community-maintained langchain-gradio repo makes it even easier to build chatbots on top of LangChain.

## OpenAI

Of course, we could also use the `openai` library directy. Here a similar example to the LangChain , but this time with streaming as well:

**Tip:**
For quick prototyping, the openai-gradio library makes it even easier to build chatbots on top of OpenAI models.

## Hugging Face `transformers`

Of course, in many cases you want to run a chatbot locally. Here's the equivalent example using the SmolLM2-135M-Instruct model using the Hugging Face `transformers` library.

```
from transformers import AutoModelForCausalLM, AutoTokenizer
import gradio as gr

checkpoint = "HuggingFaceTB/SmolLM2-135M-Instruct"
device = "cpu"  # "cuda" or "cpu"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForCausalLM.from_pretrained(checkpoint).to(device)

def predict(message, history):
    history.append({"role": "user", "content": message})
    input_text = tokenizer.apply_chat_template(history, tokenize=False)
    inputs = tokenizer.encode(input_text, return_tensors="pt").to(device)
    outputs = model.generate(inputs, max_new_tokens=100, temperature=0.2, top_p=0.9, do_sample=True)
    decoded = tokenizer.decode(outputs[0])
    response = decoded.split("<|im_start|>assistant\n")[-1].split("<|im_end|>")[0]
    return response

demo = gr.ChatInterface(predict, type="messages")

demo.launch()

```

## SambaNova

The SambaNova Cloud API provides access to full-precision open-source models, such as the Llama family. Here's an example of how to build a Gradio app around the SambaNova API

```
# This is a simple general-purpose chatbot built on top of SambaNova API.
# Before running this, make sure you have exported your SambaNova API key as an environment variable:
# export SAMBANOVA_API_KEY="your-sambanova-api-key"

import os
import gradio as gr
from openai import OpenAI

api_key = os.getenv("SAMBANOVA_API_KEY")

client = OpenAI(
    base_url="https://api.sambanova.ai/v1/",
    api_key=api_key,
)

def predict(message, history):
    history.append({"role": "user", "content": message})
    stream = client.chat.completions.create(messages=history, model="Meta-Llama-3.1-70B-Instruct-8k", stream=True)
    chunks = []
    for chunk in stream:
        chunks.append(chunk.choices[0].delta.content or "")
        yield "".join(chunks)

demo = gr.ChatInterface(predict, type="messages")

demo.launch()

```

**Tip:**
For quick prototyping, the sambanova-gradio library makes it even easier to build chatbots on top of SambaNova models.

## Hyperbolic

The Hyperbolic AI API provides access to many open-source models, such as the Llama family. Here's an example of how to build a Gradio app around the Hyperbolic

```
# This is a simple general-purpose chatbot built on top of Hyperbolic API.
# Before running this, make sure you have exported your Hyperbolic API key as an environment variable:
# export HYPERBOLIC_API_KEY="your-hyperbolic-api-key"

import os
import gradio as gr
from openai import OpenAI

api_key = os.getenv("HYPERBOLIC_API_KEY")

client = OpenAI(
    base_url="https://api.hyperbolic.xyz/v1/",
    api_key=api_key,
)

def predict(message, history):
    history.append({"role": "user", "content": message})
    stream = client.chat.completions.create(messages=history, model="gpt-4o-mini", stream=True)
    chunks = []
    for chunk in stream:
        chunks.append(chunk.choices[0].delta.content or "")
        yield "".join(chunks)

demo = gr.ChatInterface(predict, type="messages")

demo.launch()

```

**Tip:**
For quick prototyping, the hyperbolic-gradio library makes it even easier to build chatbots on top of Hyperbolic models.

## Anthropic's Claude

Anthropic's Claude model can also be used via API. Here's a simple 20 questions-style game built on top of the Anthropic API:

```
# This is a simple 20 questions-style game built on top of the Anthropic API.
# Before running this, make sure you have exported your Anthropic API key as an environment variable:
# export ANTHROPIC_API_KEY="your-anthropic-api-key"

import anthropic
import gradio as gr

client = anthropic.Anthropic()

def predict(message, history):
    keys_to_keep = ["role", "content"]
    history = [{k: d[k] for k in keys_to_keep if k in d} for d in history]
    history.append({"role": "user", "content": message})
    if len(history) > 20:
        history.append({"role": "user", "content": "DONE"})
    output = client.messages.create(
        messages=history,
        model="claude-3-5-sonnet-20241022",
        max_tokens=1000,
        system="You are guessing an object that the user is thinking of. You can ask 10 yes/no questions. Keep asking questions until the user says DONE"
    )
    return {
        "role": "assistant",
        "content": output.content[0].text,
        "options": [{"value": "Yes"}, {"value": "No"}]
    }

placeholder = """
<center><h1>10 Questions</h1><br>Think of a person, place, or thing. I'll ask you 10 yes/no questions to try and guess it.
</center>
"""

demo = gr.ChatInterface(
    predict,
    examples=["Start!"],
    chatbot=gr.Chatbot(placeholder=placeholder),
    type="messages"
)

demo.launch()

```

[←

Creating A Chatbot Fast](../guides/creating-a-chatbot-fast/) [Agents And Tool Usage

→](../guides/agents-and-tool-usage/)

Status