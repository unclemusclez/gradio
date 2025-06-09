# Plot Component For Maps

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For Maps

IntroductionOverviewStep 1 - Loading CSV data 💾Step 2 - Map Figure 🌐Step 3 - Gradio App ⚡️Step 4 - Deployment 🤗Conclusion 🎉

Running Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Other Tutorials
2. Plot Component For Maps

[←

Named Entity Recognition](../guides/named-entity-recognition/) [Running Background Tasks

→](../guides/running-background-tasks/)

# How to Use the Plot Component for Maps

## Introduction

This guide explains how you can use Gradio to plot geographical data on a map using the `gradio.Plot` component. The Gradio `Plot` component works with Matplotlib, Bokeh and Plotly. Plotly is what we will be working with in this guide. Plotly allows developers to easily create all sorts of maps with their geographical data. Take a look here for some examples.

## Overview

We will be using the New York City Airbnb dataset, which is hosted on kaggle here. I've uploaded it to the Hugging Face Hub as a dataset here for easier use and download. Using this data we will plot Airbnb locations on a map output and allow filtering based on price and location. Below is the demo that we will be building. ⚡️

## Step 1 - Loading CSV data 💾

Let's start by loading the Airbnb NYC data from the Hugging Face Hub.

```
from datasets import load_dataset

dataset = load_dataset("gradio/NYC-Airbnb-Open-Data", split="train")
df = dataset.to_pandas()

def filter_map(min_price, max_price, boroughs):
    new_df = df[(df['neighbourhood_group'].isin(boroughs)) &
            (df['price'] > min_price) & (df['price'] < max_price)]
    names = new_df["name"].tolist()
    prices = new_df["price"].tolist()
    text_list = [(names[i], prices[i]) for i in range(0, len(names))]
```

In the code above, we first load the csv data into a pandas dataframe. Let's begin by defining a function that we will use as the prediction function for the gradio app. This function will accept the minimum price and maximum price range as well as the list of boroughs to filter the resulting map. We can use the passed in values (`min_price`, `max_price`, and list of `boroughs`) to filter the dataframe and create `new_df`. Next we will create `text_list` of the names and prices of each Airbnb to use as labels on the map.

## Step 2 - Map Figure 🌐

Plotly makes it easy to work with maps. Let's take a look below how we can create a map figure.

```
import plotly.graph_objects as go

fig = go.Figure(go.Scattermapbox(
            customdata=text_list,
            lat=new_df['latitude'].tolist(),
            lon=new_df['longitude'].tolist(),
            mode='markers',
            marker=go.scattermapbox.Marker(
                size=6
            ),
            hoverinfo="text",
            hovertemplate='<b>Name</b>: %{customdata[0]}<br><b>Price</b>: $%{customdata[1]}'
        ))

fig.update_layout(
    mapbox_style="open-street-map",
    hovermode='closest',
    mapbox=dict(
        bearing=0,
        center=go.layout.mapbox.Center(
            lat=40.67,
            lon=-73.90
        ),
        pitch=0,
        zoom=9
    ),
)
```

Above, we create a scatter plot on mapbox by passing it our list of latitudes and longitudes to plot markers. We also pass in our custom data of names and prices for additional info to appear on every marker we hover over. Next we use `update_layout` to specify other map settings such as zoom, and centering.

More info here on scatter plots using Mapbox and Plotly.

## Step 3 - Gradio App ⚡️

We will use two `gr.Number` components and a `gr.CheckboxGroup` to allow users of our app to specify price ranges and borough locations. We will then use the `gr.Plot` component as an output for our Plotly + Mapbox map we created earlier.

```
with gr.Blocks() as demo:
    with gr.Column():
        with gr.Row():
            min_price = gr.Number(value=250, label="Minimum Price")
            max_price = gr.Number(value=1000, label="Maximum Price")
        boroughs = gr.CheckboxGroup(choices=["Queens", "Brooklyn", "Manhattan", "Bronx", "Staten Island"], value=["Queens", "Brooklyn"], label="Select Boroughs:")
        btn = gr.Button(value="Update Filter")
        map = gr.Plot()
    demo.load(filter_map, [min_price, max_price, boroughs], map)
    btn.click(filter_map, [min_price, max_price, boroughs], map)
```

We layout these components using the `gr.Column` and `gr.Row` and we'll also add event triggers for when the demo first loads and when our "Update Filter" button is clicked in order to trigger the map to update with our new filters.

This is what the full demo code looks like:

```

import gradio as gr
import plotly.graph_objects as go
from datasets import load_dataset

dataset = load_dataset("gradio/NYC-Airbnb-Open-Data", split="train")
df = dataset.to_pandas()

def filter_map(min_price, max_price, boroughs):

    filtered_df = df[(df['neighbourhood_group'].isin(boroughs)) &
          (df['price'] > min_price) & (df['price'] < max_price)]
    names = filtered_df["name"].tolist()
    prices = filtered_df["price"].tolist()
    text_list = [(names[i], prices[i]) for i in range(0, len(names))]
    fig = go.Figure(go.Scattermapbox(
            customdata=text_list,
            lat=filtered_df['latitude'].tolist(),
            lon=filtered_df['longitude'].tolist(),
            mode='markers',
            marker=go.scattermapbox.Marker(
                size=6
            ),
            hoverinfo="text",
            hovertemplate='<b>Name</b>: %{customdata[0]}<br><b>Price</b>: $%{customdata[1]}'
        ))

    fig.update_layout(
        mapbox_style="open-street-map",
        hovermode='closest',
        mapbox=dict(
            bearing=0,
            center=go.layout.mapbox.Center(
                lat=40.67,
                lon=-73.90
            ),
            pitch=0,
            zoom=9
        ),
    )

    return fig

with gr.Blocks() as demo:
    with gr.Column():
        with gr.Row():
            min_price = gr.Number(value=250, label="Minimum Price")
            max_price = gr.Number(value=1000, label="Maximum Price")
        boroughs = gr.CheckboxGroup(choices=["Queens", "Brooklyn", "Manhattan", "Bronx", "Staten Island"], value=["Queens", "Brooklyn"], label="Select Boroughs:")
        btn = gr.Button(value="Update Filter")
        map = gr.Plot()
    demo.load(filter_map, [min_price, max_price, boroughs], map)
    btn.click(filter_map, [min_price, max_price, boroughs], map)

demo.launch()

```

## Step 4 - Deployment 🤗

If you run the code above, your app will start running locally.
You can even get a temporary shareable link by passing the `share=True` parameter to `launch`.

But what if you want to a permanent deployment solution?
Let's deploy our Gradio app to the free HuggingFace Spaces platform.

If you haven't used Spaces before, follow the previous guide here.

## Conclusion 🎉

And you're all done! That's all the code you need to build a map demo.

Here's a link to the demo Map demo and complete code (on Hugging Face Spaces)

[←

Named Entity Recognition](../guides/named-entity-recognition/) [Running Background Tasks

→](../guides/running-background-tasks/)

Status