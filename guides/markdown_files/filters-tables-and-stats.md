# Filters Tables And Stats

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

Creating PlotsTime PlotsFilters Tables And Stats

FiltersTables and Stats

Connecting To A Database

Streaming

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Data Science And Plots
2. Filters Tables And Stats

[←

Time Plots](../guides/time-plots/) [Connecting To A Database

→](../guides/connecting-to-a-database/)

# Filters, Tables and Stats

Your dashboard will likely consist of more than just plots. Let's take a look at some of the other common components of a dashboard.

## Filters

Use any of the standard Gradio form components to filter your data. You can do this via event listeners or function-as-value syntax. Let's look at the event listener approach first:

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    with gr.Row():
        origin = gr.Dropdown(["All", "DFW", "DAL", "HOU"], value="All", label="Origin")
        destination = gr.Dropdown(["All", "JFK", "LGA", "EWR"], value="All", label="Destination")
        max_price = gr.Slider(0, 1000, value=1000, label="Max Price")

    plt = gr.ScatterPlot(df, x="time", y="price", inputs=[origin, destination, max_price])

    @gr.on(inputs=[origin, destination, max_price], outputs=plt)
    def filtered_data(origin, destination, max_price):
        _df = df[df["price"] <= max_price]
        if origin != "All":
            _df = _df[_df["origin"] == origin]
        if destination != "All":
            _df = _df[_df["destination"] == destination]
        return _df

demo.launch()
```

And this would be the function-as-value approach for the same demo.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    with gr.Row():
        origin = gr.Dropdown(["All", "DFW", "DAL", "HOU"], value="All", label="Origin")
        destination = gr.Dropdown(["All", "JFK", "LGA", "EWR"], value="All", label="Destination")
        max_price = gr.Slider(0, 1000, value=1000, label="Max Price")

    def filtered_data(origin, destination, max_price):
        _df = df[df["price"] <= max_price]
        if origin != "All":
            _df = _df[_df["origin"] == origin]
        if destination != "All":
            _df = _df[_df["destination"] == destination]
        return _df

    gr.ScatterPlot(filtered_data, x="time", y="price", inputs=[origin, destination, max_price])

demo.launch()
```

## Tables and Stats

Add `gr.DataFrame` and `gr.Label` to your dashboard for some hard numbers.

```
import gradio as gr
from data import df

with gr.Blocks() as demo:
    with gr.Row():
        gr.Label(len(df), label="Flight Count")
        gr.Label(f"${df['price'].min()}", label="Cheapest Flight")
    gr.DataFrame(df)

demo.launch()
```

[←

Time Plots](../guides/time-plots/) [Connecting To A Database

→](../guides/connecting-to-a-database/)

Status