# Queuing

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

Queuing

Configuring the QueueNotes

Streaming OutputsStreaming InputsAlertsProgress BarsBatch FunctionsSharing Your AppFile AccessMultipage AppsEnvironment VariablesResource CleanupThemesClient Side FunctionsView Api PageInternationalization

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
2. Queuing

[←

Using Blocks Like Functions](../guides/using-blocks-like-functions/) [Streaming Outputs

→](../guides/streaming-outputs/)

# Queuing

Every Gradio app comes with a built-in queuing system that can scale to thousands of concurrent users. Because many of your event listeners may involve heavy processing, Gradio automatically creates a queue to handle every event listener in the backend. Every event listener in your app automatically has a queue to process incoming events.

## Configuring the Queue

By default, each event listener has its own queue, which handles one request at a time. This can be configured via two arguments:

* `concurrency_limit`: This sets the maximum number of concurrent executions for an event listener. By default, the limit is 1 unless configured otherwise in `Blocks.queue()`. You can also set it to `None` for no limit (i.e., an unlimited number of concurrent executions). For example:

```
import gradio as gr

with gr.Blocks() as demo:
    prompt = gr.Textbox()
    image = gr.Image()
    generate_btn = gr.Button("Generate Image")
    generate_btn.click(image_gen, prompt, image, concurrency_limit=5)
```

In the code above, up to 5 requests can be processed simultaneously for this event listener. Additional requests will be queued until a slot becomes available.

If you want to manage multiple event listeners using a shared queue, you can use the `concurrency_id` argument:

* `concurrency_id`: This allows event listeners to share a queue by assigning them the same ID. For example, if your setup has only 2 GPUs but multiple functions require GPU access, you can create a shared queue for all those functions. Here's how that might look:

```
import gradio as gr

with gr.Blocks() as demo:
    prompt = gr.Textbox()
    image = gr.Image()
    generate_btn_1 = gr.Button("Generate Image via model 1")
    generate_btn_2 = gr.Button("Generate Image via model 2")
    generate_btn_3 = gr.Button("Generate Image via model 3")
    generate_btn_1.click(image_gen_1, prompt, image, concurrency_limit=2, concurrency_id="gpu_queue")
    generate_btn_2.click(image_gen_2, prompt, image, concurrency_id="gpu_queue")
    generate_btn_3.click(image_gen_3, prompt, image, concurrency_id="gpu_queue")
```

In this example, all three event listeners share a queue identified by `"gpu_queue"`. The queue can handle up to 2 concurrent requests at a time, as defined by the `concurrency_limit`.

### Notes

* To ensure unlimited concurrency for an event listener, set `concurrency_limit=None`. This is useful if your function is calling e.g. an external API which handles the rate limiting of requests itself.
* The default concurrency limit for all queues can be set globally using the `default_concurrency_limit` parameter in `Blocks.queue()`.

These configurations make it easy to manage the queuing behavior of your Gradio app.

[←

Using Blocks Like Functions](../guides/using-blocks-like-functions/) [Streaming Outputs

→](../guides/streaming-outputs/)

Status