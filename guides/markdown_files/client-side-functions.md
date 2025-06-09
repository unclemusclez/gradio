# Client Side Functions

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

QueuingStreaming OutputsStreaming InputsAlertsProgress BarsBatch FunctionsSharing Your AppFile AccessMultipage AppsEnvironment VariablesResource CleanupThemesClient Side Functions

When to Use Client Side FunctionsLimitationsComplete ExampleBehind the Scenes

View Api PageInternationalization

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Additional Features
2. Client Side Functions

[←

Themes](../guides/themes/) [View Api Page

→](../guides/view-api-page/)

# Client Side Functions

Gradio allows you to run certain "simple" functions directly in the browser by setting `js=True` in your event listeners. This will **automatically convert your Python code into JavaScript**, which significantly improves the responsiveness of your app by avoiding a round trip to the server for simple UI updates.

The difference in responsiveness is most noticeable on hosted applications (like Hugging Face Spaces), when the server is under heavy load, with high-latency connections, or when many users are accessing the app simultaneously.

## When to Use Client Side Functions

Client side functions are ideal for updating component properties (like visibility, placeholders, interactive state, or styling).

Here's a basic example:

```
import gradio as gr

with gr.Blocks() as demo:
    with gr.Row() as row:
        btn = gr.Button("Hide this row")

    # This function runs in the browser without a server roundtrip
    btn.click(
        lambda: gr.Row(visible=False),
        None,
        row,
        js=True
    )

demo.launch()
```

## Limitations

Client side functions have some important restrictions:

* They can only update component properties (not values)
* They cannot take any inputs

Here are some functions that will work with `js=True`:

```
# Simple property updates
lambda: gr.Textbox(lines=4)

# Multiple component updates
lambda: [gr.Textbox(lines=4), gr.Button(interactive=False)]

# Using gr.update() for property changes
lambda: gr.update(visible=True, interactive=False)
```

We are working to increase the space of functions that can be transpiled to JavaScript so that they can be run in the browser. Follow the Groovy library for more info.

## Complete Example

Here's a more complete example showing how client side functions can improve the user experience:

```
"""
This is a simple todo list app that allows you to edit tasks and mark tasks as complete.
All actions are performed on the client side.
"""
import gradio as gr

tasks = ["Get a job", "Marry rich", "", "", "", ""]
textboxes = []
buttons = []
with gr.Blocks() as demo:
    with gr.Row():
        with gr.Column(scale=3):
            gr.Markdown("# A Simple Interactive Todo List")
        with gr.Column(scale=2):
            with gr.Row():
                freeze_button = gr.Button("Freeze tasks", variant="stop")
                edit_button = gr.Button("Edit tasks")
    for i in range(6):
        with gr.Row() as r:
            t = gr.Textbox(tasks[i], placeholder="Enter a task", show_label=False, container=False, scale=7, interactive=True)
            b = gr.Button("✔️", interactive=bool(tasks[i]), variant="primary" if tasks[i] else "secondary")
            textboxes.append(t)
            buttons.append(b)
        t.change(lambda : gr.Button(interactive=True, variant="primary"), None, b, js=True)
        b.click(lambda : gr.Row(visible=False), None, r, js=True)
    freeze_button.click(lambda : [gr.Textbox(interactive=False), gr.Textbox(interactive=False), gr.Textbox(interactive=False), gr.Textbox(interactive=False), gr.Textbox(interactive=False), gr.Textbox(interactive=False)], None, textboxes, js=True)
    edit_button.click(lambda : [gr.Textbox(interactive=True), gr.Textbox(interactive=True), gr.Textbox(interactive=True), gr.Textbox(interactive=True), gr.Textbox(interactive=True), gr.Textbox(interactive=True)], None, textboxes, js=True)
    freeze_button.click(lambda : [gr.Button(visible=False), gr.Button(visible=False), gr.Button(visible=False), gr.Button(visible=False), gr.Button(visible=False), gr.Button(visible=False)], None, buttons, js=True)
    edit_button.click(lambda : [gr.Button(visible=True), gr.Button(visible=True), gr.Button(visible=True), gr.Button(visible=True), gr.Button(visible=True), gr.Button(visible=True)], None, buttons, js=True)

demo.launch()

```

## Behind the Scenes

When you set `js=True`, Gradio:

1. Transpiles your Python function to JavaScript
2. Runs the function directly in the browser
3. Still sends the request to the server (for consistency and to handle any side effects)

This provides immediate visual feedback while ensuring your application state remains consistent.

[←

Themes](../guides/themes/) [View Api Page

→](../guides/view-api-page/)

Status