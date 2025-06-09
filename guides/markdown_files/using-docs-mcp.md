# Using Docs Mcp

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs Mcp

PrerequisitesWhy an MCP Server?Installing in the ClientsCursorClaude DesktopTools

Gradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Other Tutorials
2. Using Docs Mcp

[←

Building Mcp Server With Gradio](../guides/building-mcp-server-with-gradio/) [Gradio And Comet

→](../guides/Gradio-and-Comet/)

# Using the Gradio Docs MCP Server

In this guide, we will describe how to use the official Gradio Docs MCP Server.

### Prerequisites

You will need an LLM application that supports tool calling using the MCP protocol, such as Claude Desktop, Cursor, or Cline (these are known as "MCP Clients").

## Why an MCP Server?

If you're using LLMs in your workflow, adding this server will augment them with just the right context on gradio - which makes your experience a lot faster and smoother.

 

The server is running on Spaces and was launched entirely using Gradio, you can see all the code here. For more on building an mcp server with gradio, see the previous guide.

## Installing in the Clients

For clients that support SSE (e.g. Cursor, Windsurf, Cline), simply add the following configuration to your MCP config:

```
{
  "mcpServers": {
    "gradio": {
      "url": "https://gradio-docs-mcp.hf.space/gradio_api/mcp/sse"
    }
  }
}
```

We've included step-by-step instructions for Cursor below, but you can consult the docs for Windsurf here, and Cline here which are similar to set up.

### Cursor

1. Make sure you're using the latest version of Cursor, and go to Cursor > Settings > Cursor Settings > MCP
2. Click on '+ Add new global MCP server'
3. Copy paste this json into the file that opens and then save it.

```
{
  "mcpServers": {
    "gradio": {
      "url": "https://gradio-docs-mcp.hf.space/gradio_api/mcp/sse"
    }
  }
}
```

4. That's it! You should see the tools load and the status go green in the settings page. You may have to click the refresh icon or wait a few seconds.

### Claude Desktop

1. Since Claude Desktop only supports stdio, you will need to install Node.js to get this to work.
2. Make sure you're using the latest version of Claude Desktop, and go to Claude > Settings > Developer > Edit Config
3. Open the file with your favorite editor and copy paste this json, then save the file.

```
{
  "mcpServers": {
    "gradio": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://gradio-docs-mcp.hf.space/gradio_api/mcp/sse",
        "--transport",
        "sse-only"
      ]
    }
  }
}
```

4. Quit and re-open Claude Desktop, and you should be good to go. You should see it loaded in the Search and Tools icon or on the developer settings page.

## Tools

There are currently only two tools in the server: `gradio_docs_mcp_load_gradio_docs` and `gradio_docs_mcp_search_gradio_docs`.

1. `gradio_docs_mcp_load_gradio_docs`: This tool takes no arguments and will load an /llms.txt style summary of Gradio's latest, full documentation. Very useful context the LLM can parse before answering questions or generating code.
2. `gradio_docs_mcp_search_gradio_docs`: This tool takes a query as an argument and will run embedding search on Gradio's docs, guides, and demos to return the most useful context for the LLM to parse.

[←

Building Mcp Server With Gradio](../guides/building-mcp-server-with-gradio/) [Gradio And Comet

→](../guides/Gradio-and-Comet/)

Status