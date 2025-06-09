# Creating A Dashboard From Supabase Data

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase Data

Create a table in SupabaseWrite data to SupabaseVisualize the Data in a Real-Time Gradio DashboardConclusion

Creating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Other Tutorials
2. Creating A Dashboard From Supabase Data

[←

Creating A Dashboard From Bigquery Data](../guides/creating-a-dashboard-from-bigquery-data/) [Creating A Realtime Dashboard From Google Sheets

→](../guides/creating-a-realtime-dashboard-from-google-sheets/)

# Create a Dashboard from Supabase Data

Supabase is a cloud-based open-source backend that provides a PostgreSQL database, authentication, and other useful features for building web and mobile applications. In this tutorial, you will learn how to read data from Supabase and plot it in **real-time** on a Gradio Dashboard.

**Prerequisites:** To start, you will need a free Supabase account, which you can sign up for here: <https://app.supabase.com/>

In this end-to-end guide, you will learn how to:

* Create tables in Supabase
* Write data to Supabase using the Supabase Python Client
* Visualize the data in a real-time dashboard using Gradio

If you already have data on Supabase that you'd like to visualize in a dashboard, you can skip the first two sections and go directly to visualizing the data!

## Create a table in Supabase

First of all, we need some data to visualize. Following this excellent guide, we'll create fake commerce data and put it in Supabase.

1. Start by creating a new project in Supabase. Once you're logged in, click the "New Project" button

2. Give your project a name and database password. You can also choose a pricing plan (for our purposes, the Free Tier is sufficient!)

3. You'll be presented with your API keys while the database spins up (can take up to 2 minutes).

4. Click on "Table Editor" (the table icon) in the left pane to create a new table. We'll create a single table called `Product`, with the following schema:

|  |  |
| --- | --- |
| product\_id | int8 |
| inventory\_count | int8 |
| price | float8 |
| product\_name | varchar |

5. Click Save to save the table schema.

Our table is now ready!

## Write data to Supabase

The next step is to write data to a Supabase dataset. We will use the Supabase Python library to do this.

6. Install `supabase` by running the following command in your terminal:

```
pip install supabase
```

7. Get your project URL and API key. Click the Settings (gear icon) on the left pane and click 'API'. The URL is listed in the Project URL box, while the API key is listed in Project API keys (with the tags `service_role`, `secret`)

8. Now, run the following Python script to write some fake data to the table (note you have to put the values of `SUPABASE_URL` and `SUPABASE_SECRET_KEY` from step 7):

```
import supabase

# Initialize the Supabase client
client = supabase.create_client('SUPABASE_URL', 'SUPABASE_SECRET_KEY')

# Define the data to write
import random

main_list = []
for i in range(10):
    value = {'product_id': i,
             'product_name': f"Item {i}",
             'inventory_count': random.randint(1, 100),
             'price': random.random()*100
            }
    main_list.append(value)

# Write the data to the table
data = client.table('Product').insert(main_list).execute()
```

Return to your Supabase dashboard and refresh the page, you should now see 10 rows populated in the `Product` table!

## Visualize the Data in a Real-Time Gradio Dashboard

Finally, we will read the data from the Supabase dataset using the same `supabase` Python library and create a realtime dashboard using `gradio`.

Note: We repeat certain steps in this section (like creating the Supabase client) in case you did not go through the previous sections. As described in Step 7, you will need the project URL and API Key for your database.

9. Write a function that loads the data from the `Product` table and returns it as a pandas Dataframe:

```
import supabase
import pandas as pd

client = supabase.create_client('SUPABASE_URL', 'SUPABASE_SECRET_KEY')

def read_data():
    response = client.table('Product').select("*").execute()
    df = pd.DataFrame(response.data)
    return df
```

10. Create a small Gradio Dashboard with 2 Barplots that plots the prices and inventories of all of the items every minute and updates in real-time:

```
import gradio as gr

with gr.Blocks() as dashboard:
    with gr.Row():
        gr.BarPlot(read_data, x="product_id", y="price", title="Prices", every=gr.Timer(60))
        gr.BarPlot(read_data, x="product_id", y="inventory_count", title="Inventory", every=gr.Timer(60))

dashboard.queue().launch()
```

Notice that by passing in a function to `gr.BarPlot()`, we have the BarPlot query the database as soon as the web app loads (and then again every 60 seconds because of the `every` parameter). Your final dashboard should look something like this:

## Conclusion

That's it! In this tutorial, you learned how to write data to a Supabase dataset, and then read that data and plot the results as bar plots. If you update the data in the Supabase database, you'll notice that the Gradio dashboard will update within a minute.

Try adding more plots and visualizations to this example (or with a different dataset) to build a more complex dashboard!

[←

Creating A Dashboard From Bigquery Data](../guides/creating-a-dashboard-from-bigquery-data/) [Creating A Realtime Dashboard From Google Sheets

→](../guides/creating-a-realtime-dashboard-from-google-sheets/)

Status