# View Api Page

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

QueuingStreaming OutputsStreaming InputsAlertsProgress BarsBatch FunctionsSharing Your AppFile AccessMultipage AppsEnvironment VariablesResource CleanupThemesClient Side FunctionsView Api Page

Configuring the API PageThe ClientsThe API Recorder 🪄MCP ServerOpenAPI Specification

Internationalization

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
2. View Api Page

[←

Client Side Functions](../guides/client-side-functions/) [Internationalization

→](../guides/internationalization/)

# API Page

You can use almost any Gradio app programmatically via the built-in API! In the footer of any Gradio app, you'll see a "Use via API" link. Clicking on the link opens up a detailed documentation page for the API that Gradio generates based on the function signatures in your Gradio app.

## Configuring the API Page

**API endpoint names**

When you create a Gradio application, the API endpoint names are automatically generated based on the function names. You can change this by using the `api_name` parameter in `gr.Interface` or `gr.ChatInterface`. If you are using Gradio `Blocks`, you can name each event listener, like this:

```
btn.click(add, [num1, num2], output, api_name="addition")
```

**Hiding API endpoints**

When building a complex Gradio app, you might want to hide certain API endpoints from appearing on the view API page, e.g. if they correspond to functions that simply update the UI. You can set the `show_api` parameter to `False` in any `Blocks` event listener to achieve this, e.g.

```
btn.click(add, [num1, num2], output, show_api=False)
```

**Disabling API endpoints**

Hiding the API endpoint doesn't disable it. A user can still programmatically call the API endpoint if they know the name. If you want to disable an API endpoint altogether, set `api_name=False`, e.g.

```
btn.click(add, [num1, num2], output, api_name=False)
```

Note: setting an `api_name=False` also means that downstream apps will not be able to load your Gradio app using `gr.load()` as this function uses the Gradio API under the hood.

**Adding API endpoints**

You can also add new API routes to your Gradio application that do not correspond to events in your UI.

For example, in this Gradio applicaiton, we add a new route that adds numbers and slices a list:

```
import gradio as gr
with gr.Blocks() as demo:
    with gr.Row():
        input = gr.Textbox()
        button = gr.Button("Submit")
    output = gr.Textbox()
    def fn(a: int, b: int, c: list[str]) -> tuple[int, str]:
        return a + b, c[a:b]
    gr.api(fn, api_name="add_and_slice")

_, url, _ = demo.launch()
```

This will create a new route `/add_and_slice` which will show up in the "view API" page. It can be programmatically called by the Python or JS Clients (discussed below) like this:

```
from gradio_client import Client

client = Client(url)
result = client.predict(
        a=3,
        b=5,
        c=[1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
        api_name="/add_and_slice"
)
print(result)
```

## The Clients

This API page not only lists all of the endpoints that can be used to query the Gradio app, but also shows the usage of both the Gradio Python client, and the Gradio JavaScript client.

For each endpoint, Gradio automatically generates a complete code snippet with the parameters and their types, as well as example inputs, allowing you to immediately test an endpoint. Here's an example showing an image file input and `str` output:

## The API Recorder 🪄

Instead of reading through the view API page, you can also use Gradio's built-in API recorder to generate the relevant code snippet. Simply click on the "API Recorder" button, use your Gradio app via the UI as you would normally, and then the API Recorder will generate the code using the Clients to recreate your all of your interactions programmatically.

## MCP Server

The API page also includes instructions on how to use the Gradio app as an Model Context Protocol (MCP) server, which is a standardized way to expose functions as tools so that they can be used by LLMs.

For the MCP sever, each tool, its description, and its parameters are listed, along with instructions on how to integrate with popular MCP Clients. Read more about Gradio's MCP integration here.

## OpenAPI Specification

You can access the complete OpenAPI (formerly Swagger) specification of your Gradio app's API at the endpoint `<your-gradio-app-url>/gradio_api/openapi.json`. The OpenAPI specification is a standardized, language-agnostic interface description for REST APIs that enables both humans and computers to discover and understand the capabilities of your service.

[←

Client Side Functions](../guides/client-side-functions/) [Internationalization

→](../guides/internationalization/)

Status