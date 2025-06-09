# Deploying Gradio With Docker

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With Docker

IntroductionHow to Dockerize a Gradio AppStep 1: Create Your Gradio AppStep 2: Create a DockerfileStep 3: Build and Run Your Docker ContainerImportant ConsiderationsRunning the Gradio app on "0.0.0.0" and exposing port 7860Enable Stickiness for Multiple ReplicasDeploying Behind a Proxy

Developing Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Other Tutorials
2. Deploying Gradio With Docker

[←

Creating A Realtime Dashboard From Google Sheets](../guides/creating-a-realtime-dashboard-from-google-sheets/) [Developing Faster With Reload Mode

→](../guides/developing-faster-with-reload-mode/)

# Deploying a Gradio app with Docker

### Introduction

Gradio is a powerful and intuitive Python library designed for creating web apps that showcase machine learning models. These web apps can be run locally, or deployed on Hugging Face Spaces for free. Or, you can deploy them on your servers in Docker containers. Dockerizing Gradio apps offers several benefits:

* **Consistency**: Docker ensures that your Gradio app runs the same way, irrespective of where it is deployed, by packaging the application and its environment together.
* **Portability**: Containers can be easily moved across different systems or cloud environments.
* **Scalability**: Docker works well with orchestration systems like Kubernetes, allowing your app to scale up or down based on demand.

## How to Dockerize a Gradio App

Let's go through a simple example to understand how to containerize a Gradio app using Docker.

#### Step 1: Create Your Gradio App

First, we need a simple Gradio app. Let's create a Python file named `app.py` with the following content:

```
import gradio as gr

def greet(name):
    return f"Hello {name}!"

iface = gr.Interface(fn=greet, inputs="text", outputs="text").launch()
```

This app creates a simple interface that greets the user by name.

#### Step 2: Create a Dockerfile

Next, we'll create a Dockerfile to specify how our app should be built and run in a Docker container. Create a file named `Dockerfile` in the same directory as your app with the following content:

```
FROM python:3.10-slim

WORKDIR /usr/src/app
COPY . .
RUN pip install --no-cache-dir gradio
EXPOSE 7860
ENV GRADIO_SERVER_NAME="0.0.0.0"

CMD ["python", "app.py"]
```

This Dockerfile performs the following steps:

* Starts from a Python 3.10 slim image.
* Sets the working directory and copies the app into the container.
* Installs Gradio (you should install all other requirements as well).
* Exposes port 7860 (Gradio's default port).
* Sets the `GRADIO_SERVER_NAME` environment variable to ensure Gradio listens on all network interfaces.
* Specifies the command to run the app.

#### Step 3: Build and Run Your Docker Container

With the Dockerfile in place, you can build and run your container:

```
docker build -t gradio-app .
docker run -p 7860:7860 gradio-app
```

Your Gradio app should now be accessible at `http://localhost:7860`.

## Important Considerations

When running Gradio applications in Docker, there are a few important things to keep in mind:

#### Running the Gradio app on `"0.0.0.0"` and exposing port 7860

In the Docker environment, setting `GRADIO_SERVER_NAME="0.0.0.0"` as an environment variable (or directly in your Gradio app's `launch()` function) is crucial for allowing connections from outside the container. And the `EXPOSE 7860` directive in the Dockerfile tells Docker to expose Gradio's default port on the container to enable external access to the Gradio app.

#### Enable Stickiness for Multiple Replicas

When deploying Gradio apps with multiple replicas, such as on AWS ECS, it's important to enable stickiness with `sessionAffinity: ClientIP`. This ensures that all requests from the same user are routed to the same instance. This is important because Gradio's communication protocol requires multiple separate connections from the frontend to the backend in order for events to be processed correctly. (If you use Terraform, you'll want to add a stickiness block into your target group definition.)

#### Deploying Behind a Proxy

If you're deploying your Gradio app behind a proxy, like Nginx, it's essential to configure the proxy correctly. Gradio provides a Guide that walks through the necessary steps. This setup ensures your app is accessible and performs well in production environments.

[←

Creating A Realtime Dashboard From Google Sheets](../guides/creating-a-realtime-dashboard-from-google-sheets/) [Developing Faster With Reload Mode

→](../guides/developing-faster-with-reload-mode/)

Status