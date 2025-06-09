# Creating Plots

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

Creating Plots

Creating a Plot with a pd.DataframeBreaking out Series by ColorAggregating ValuesSelecting RegionsMaking an Interactive Dashboard

Time PlotsFilters Tables And StatsConnecting To A Database

Streaming

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Data Science And Plots
2. Creating Plots

[←

Creating A Website Widget From A Gradio Chatbot](../guides/creating-a-website-widget-from-a-gradio-chatbot/) [Time Plots

→](../guides/time-plots/)

# Creating Plots

Gradio is a great way to create extremely customizable dashboards. Gradio comes with three native Plot components: `gr.LinePlot`, `gr.ScatterPlot` and `gr.BarPlot`. All these plots have the same API. Let's take a look how to set them up.

## Creating a Plot with a pd.Dataframe

Plots accept a pandas Dataframe as their value. The plot also takes `x` and `y` which represent the names of the columns that represent the x and y axes respectively. Here's a simple example:

```
import gradio as gr
import pandas as pd
import numpy as np
import random

df = pd.DataFrame({
    'height': np.random.randint(50, 70, 25),
    'weight': np.random.randint(120, 320, 25),
    'age': np.random.randint(18, 65, 25),
    'ethnicity': [random.choice(["white", "black", "asian"]) for _ in range(25)]
})

with gr.Blocks() as demo:
    gr.LinePlot(df, x="weight", y="height")

demo.launch()
```

All plots have the same API, so you could swap this out with a `gr.ScatterPlot`:

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    gr.ScatterPlot(df, x="weight", y="height")

demo.launch()
```

The y axis column in the dataframe should have a numeric type, but the x axis column can be anything from strings, numbers, categories, or datetimes.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    gr.ScatterPlot(df, x="ethnicity", y="height")

demo.launch()
```

## Breaking out Series by Color

You can break out your plot into series using the `color` argument.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    gr.ScatterPlot(df, x="weight", y="height", color="ethnicity")

demo.launch()
```

If you wish to assign series specific colors, use the `color_map` arg, e.g. `gr.ScatterPlot(..., color_map={'white': '#FF9988', 'asian': '#88EEAA', 'black': '#333388'})`

The color column can be numeric type as well.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    gr.ScatterPlot(df, x="weight", y="height", color="age")

demo.launch()
```

## Aggregating Values

You can aggregate values into groups using the `x_bin` and `y_aggregate` arguments. If your x-axis is numeric, providing an `x_bin` will create a histogram-style binning:

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    gr.BarPlot(df, x="weight", y="height", x_bin=10, y_aggregate="sum")

demo.launch()
```

If your x-axis is a string type instead, they will act as the category bins automatically:

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    gr.BarPlot(df, x="ethnicity", y="height", y_aggregate="mean")

demo.launch()
```

## Selecting Regions

You can use the `.select` listener to select regions of a plot. Click and drag on the plot below to select part of the plot.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    plt = gr.LinePlot(df, x="weight", y="height")
    selection_total = gr.Number(label="Total Weight of Selection")

    def select_region(selection: gr.SelectData):
        min_w, max_w = selection.index
        return df[(df["weight"] >= min_w) & (df["weight"] <= max_w)]["weight"].sum()

    plt.select(select_region, None, selection_total)

demo.launch()
```

You can combine this and the `.double_click` listener to create some zoom in/out effects by changing `x_lim` which sets the bounds of the x-axis:

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    plt = gr.LinePlot(df, x="weight", y="height")

    def select_region(selection: gr.SelectData):
        min_w, max_w = selection.index
        return gr.LinePlot(x_lim=(min_w, max_w))

    plt.select(select_region, None, plt)
    plt.double_click(lambda: gr.LinePlot(x_lim=None), None, plt)

demo.launch()
```

If you had multiple plots with the same x column, your event listeners could target the x limits of all other plots so that the x-axes stay in sync.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    plt1 = gr.LinePlot(df, x="weight", y="height")
    plt2 = gr.BarPlot(df, x="weight", y="age", x_bin=10)
    plots = [plt1, plt2]

    def select_region(selection: gr.SelectData):
        min_w, max_w = selection.index
        return [gr.LinePlot(x_lim=(min_w, max_w))] * len(plots)

    for plt in plots:
        plt.select(select_region, None, plots)
        plt.double_click(lambda: [gr.LinePlot(x_lim=None)] * len(plots), None, plots)

demo.launch()
```

## Making an Interactive Dashboard

Take a look how you can have an interactive dashboard where the plots are functions of other Components.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    with gr.Row():
        ethnicity = gr.Dropdown(["all", "white", "black", "asian"], value="all")
        max_age = gr.Slider(18, 65, value=65)

    def filtered_df(ethnic, age):
        _df = df if ethnic == "all" else df[df["ethnicity"] == ethnic]
        _df = _df[_df["age"] < age]
        return _df

    gr.ScatterPlot(filtered_df, inputs=[ethnicity, max_age], x="weight", y="height", title="Weight x Height")
    gr.LinePlot(filtered_df, inputs=[ethnicity, max_age], x="age", y="height", title="Age x Height")

demo.launch()
```

It's that simple to filter and control the data presented in your visualization!

[←

Creating A Website Widget From A Gradio Chatbot](../guides/creating-a-website-widget-from-a-gradio-chatbot/) [Time Plots

→](../guides/time-plots/)

Status