# Time Plots

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

Creating PlotsTime Plots

Creating a Plot with a pd.DataframeAggregating by TimeDateTime ComponentsRealTime Data

Filters Tables And StatsConnecting To A Database

Streaming

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Data Science And Plots
2. Time Plots

[←

Creating Plots](../guides/creating-plots/) [Filters Tables And Stats

→](../guides/filters-tables-and-stats/)

# Time Plots

Creating visualizations with a time x-axis is a common use case. Let's dive in!

## Creating a Plot with a pd.Dataframe

Time plots need a datetime column on the x-axis. Here's a simple example with some flight data:

```
import gradio as gr
import pandas as pd
import numpy as np
import random

from datetime import datetime, timedelta
now = datetime.now()

df = pd.DataFrame({
    'time': [now - timedelta(minutes=5*i) for i in range(25)],
    'price': np.random.randint(100, 1000, 25),
    'origin': [random.choice(["DFW", "DAL", "HOU"]) for _ in range(25)],
    'destination': [random.choice(["JFK", "LGA", "EWR"]) for _ in range(25)],
})

with gr.Blocks() as demo:
    gr.LinePlot(df, x="time", y="price")
    gr.ScatterPlot(df, x="time", y="price", color="origin")

demo.launch()
```

## Aggregating by Time

You may wish to bin data by time buckets. Use `x_bin` to do so, using a string suffix with "s", "m", "h" or "d", such as "15m" or "1d".

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    plot = gr.BarPlot(df, x="time", y="price", x_bin="10m")

    bins = gr.Radio(["10m", "30m", "1h"], label="Bin Size")
    bins.change(lambda bins: gr.BarPlot(x_bin=bins), bins, plot)

demo.launch()
```

## DateTime Components

You can use `gr.DateTime` to accept input datetime data. This works well with plots for defining the x-axis range for the data.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    with gr.Row():
        start = gr.DateTime("now - 24h")
        end = gr.DateTime("now")
        apply_btn = gr.Button("Apply")
    plot = gr.LinePlot(df, x="time", y="price")

    apply_btn.click(lambda start, end: gr.BarPlot(x_lim=[start, end]), [start, end], plot)

demo.launch()
```

Note how `gr.DateTime` can accept a full datetime string, or a shorthand using `now - [0-9]+[smhd]` format to refer to a past time.

You will often have many time plots in which case you'd like to keep the x-axes in sync. The `DateTimeRange` custom component keeps a set of datetime plots in sync, and also uses the `.select` listener of plots to allow you to zoom into plots while keeping plots in sync.

Because it is a custom component, you first need to `pip install gradio_datetimerange`. Then run the following:

```
import gradio as gr
from gradio_datetimerange import DateTimeRange
from data import df

with gr.Blocks() as demo:
    daterange = DateTimeRange(["now - 24h", "now"])
    plot1 = gr.LinePlot(df, x="time", y="price")
    plot2 = gr.LinePlot(df, x="time", y="price", color="origin")
    daterange.bind([plot1, plot2])

demo.launch()
```

Try zooming around in the plots and see how DateTimeRange updates. All the plots updates their `x_lim` in sync. You also have a "Back" link in the component to allow you to quickly zoom in and out.

## RealTime Data

In many cases, you're working with live, realtime date, not a static dataframe. In this case, you'd update the plot regularly with a `gr.Timer()`. Assuming there's a `get_data` method that gets the latest dataframe:

```
with gr.Blocks() as demo:
    timer = gr.Timer(5)
    plot1 = gr.BarPlot(x="time", y="price")
    plot2 = gr.BarPlot(x="time", y="price", color="origin")

    timer.tick(lambda: [get_data(), get_data()], outputs=[plot1, plot2])
```

You can also use the `every` shorthand to attach a `Timer` to a component that has a function value:

```
with gr.Blocks() as demo:
    timer = gr.Timer(5)
    plot1 = gr.BarPlot(get_data, x="time", y="price", every=timer)
    plot2 = gr.BarPlot(get_data, x="time", y="price", color="origin", every=timer)
```

[←

Creating Plots](../guides/creating-plots/) [Filters Tables And Stats

→](../guides/filters-tables-and-stats/)

Status