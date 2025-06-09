# Named Entity Recognition

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

Creating A Chatbot FastChatinterface ExamplesAgents And Tool UsageCreating A Custom Chatbot With BlocksChatbot Specific EventsCreating A Discord Bot From A Gradio AppCreating A Slack Bot From A Gradio AppCreating A Website Widget From A Gradio Chatbot

Data Science And Plots

Creating PlotsTime PlotsFilters Tables And StatsConnecting To A Database

Streaming

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity Recognition

IntroductionPrerequisitesApproach 1: List of Entity DictionariesApproach 2: List of Tuples

Plot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Other Tutorials
2. Named Entity Recognition

[←

Installing Gradio In A Virtual Environment](../guides/installing-gradio-in-a-virtual-environment/) [Plot Component For Maps

→](../guides/plot-component-for-maps/)

Related Spaces:

rajistics/biobert\_ner\_demo

abidlabs/ner

rajistics/Financial\_Analyst\_AI

# Named-Entity Recognition

## Introduction

Named-entity recognition (NER), also known as token classification or text tagging, is the task of taking a sentence and classifying every word (or "token") into different categories, such as names of people or names of locations, or different parts of speech.

For example, given the sentence:

> Does Chicago have any Pakistani restaurants?

A named-entity recognition algorithm may identify:

* "Chicago" as a **location**
* "Pakistani" as an **ethnicity**

and so on.

Using `gradio` (specifically the `HighlightedText` component), you can easily build a web demo of your NER model and share that with the rest of your team.

Here is an example of a demo that you'll be able to build:

This tutorial will show how to take a pretrained NER model and deploy it with a Gradio interface. We will show two different ways to use the `HighlightedText` component -- depending on your NER model, either of these two ways may be easier to learn!

### Prerequisites

Make sure you have the `gradio` Python package already installed. You will also need a pretrained named-entity recognition model. You can use your own, while in this tutorial, we will use one from the `transformers` library.

### Approach 1: List of Entity Dictionaries

Many named-entity recognition models output a list of dictionaries. Each dictionary consists of an *entity*, a "start" index, and an "end" index. This is, for example, how NER models in the `transformers` library operate:

```
from transformers import pipeline
ner_pipeline = pipeline("ner")
ner_pipeline("Does Chicago have any Pakistani restaurants")
```

Output:

```
[{'entity': 'I-LOC',
  'score': 0.9988978,
  'index': 2,
  'word': 'Chicago',
  'start': 5,
  'end': 12},
 {'entity': 'I-MISC',
  'score': 0.9958592,
  'index': 5,
  'word': 'Pakistani',
  'start': 22,
  'end': 31}]
```

If you have such a model, it is very easy to hook it up to Gradio's `HighlightedText` component. All you need to do is pass in this **list of entities**, along with the **original text** to the model, together as dictionary, with the keys being `"entities"` and `"text"` respectively.

Here is a complete example:

```
from transformers import pipeline

import gradio as gr

ner_pipeline = pipeline("ner")

examples = [
    "Does Chicago have any stores and does Joe live here?",
]

def ner(text):
    output = ner_pipeline(text)
    return {"text": text, "entities": output}

demo = gr.Interface(ner,
             gr.Textbox(placeholder="Enter sentence here..."),
             gr.HighlightedText(),
             examples=examples)

demo.launch()

```

### Approach 2: List of Tuples

An alternative way to pass data into the `HighlightedText` component is a list of tuples. The first element of each tuple should be the word or words that are being classified into a particular entity. The second element should be the entity label (or `None` if they should be unlabeled). The `HighlightedText` component automatically strings together the words and labels to display the entities.

In some cases, this can be easier than the first approach. Here is a demo showing this approach using Spacy's parts-of-speech tagger:

```
import gradio as gr
import os
os.system('python -m spacy download en_core_web_sm')
import spacy
from spacy import displacy

nlp = spacy.load("en_core_web_sm")

def text_analysis(text):
    doc = nlp(text)
    html = displacy.render(doc, style="dep", page=True)
    html = (
        "<div style='max-width:100%; max-height:360px; overflow:auto'>"
        + html
        + "</div>"
    )
    pos_count = {
        "char_count": len(text),
        "token_count": 0,
    }
    pos_tokens = []

    for token in doc:
        pos_tokens.extend([(token.text, token.pos_), (" ", None)])

    return pos_tokens, pos_count, html

demo = gr.Interface(
    text_analysis,
    gr.Textbox(placeholder="Enter sentence here..."),
    ["highlight", "json", "html"],
    examples=[
        ["What a beautiful morning for a walk!"],
        ["It was the best of times, it was the worst of times."],
    ],
)

demo.launch()

```

---

And you're done! That's all you need to know to build a web-based GUI for your NER model.

Fun tip: you can share your NER demo instantly with others simply by setting `share=True` in `launch()`.

[←

Installing Gradio In A Virtual Environment](../guides/installing-gradio-in-a-virtual-environment/) [Plot Component For Maps

→](../guides/plot-component-for-maps/)

Status