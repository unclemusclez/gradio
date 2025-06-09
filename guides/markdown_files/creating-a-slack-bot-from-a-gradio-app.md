# Creating A Slack Bot From A Gradio App

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

Creating A Chatbot FastChatinterface ExamplesAgents And Tool UsageCreating A Custom Chatbot With BlocksChatbot Specific EventsCreating A Discord Bot From A Gradio AppCreating A Slack Bot From A Gradio App

How does it work?Prerequisites1. Create a Slack App2. Write a Slack bot3. Add the bot to your Slack Workplace4. That's it!

Creating A Website Widget From A Gradio Chatbot

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

1. Chatbots
2. Creating A Slack Bot From A Gradio App

[←

Creating A Discord Bot From A Gradio App](../guides/creating-a-discord-bot-from-a-gradio-app/) [Creating A Website Widget From A Gradio Chatbot

→](../guides/creating-a-website-widget-from-a-gradio-chatbot/)

# 🚀 Creating a Slack Bot from a Gradio App 🚀

You can make your Gradio app available as a Slack bot to let users in your Slack workspace interact with it directly.

## How does it work?

The Slack bot will listen to messages mentioning it in channels. When it receives a message (which can include text as well as files), it will send it to your Gradio app via Gradio's built-in API. Your bot will reply with the response it receives from the API.

Because Gradio's API is very flexible, you can create Slack bots that support text, images, audio, streaming, chat history, and a wide variety of other features very easily.

## Prerequisites

* Install the latest version of `gradio` and the `slack-bolt` library:

```
pip install --upgrade gradio slack-bolt~=1.0
```

* Have a running Gradio app. This app can be running locally or on Hugging Face Spaces. In this example, we will be using the Gradio Playground Space, which takes in an image and/or text and generates the code to generate the corresponding Gradio app.

Now, we are ready to get started!

### 1. Create a Slack App

1. Go to api.slack.com/apps and click "Create New App"
2. Choose "From scratch" and give your app a name
3. Select the workspace where you want to develop your app
4. Under "OAuth & Permissions", scroll to "Scopes" and add these Bot Token Scopes:
   * `app_mentions:read`
   * `chat:write`
   * `files:read`
   * `files:write`
5. In the same "OAuth & Permissions" page, scroll back up and click the button to install the app to your workspace.
6. Note the "Bot User OAuth Token" (starts with `xoxb-`) that appears as we'll need it later
7. Click on "Socket Mode" in the menu bar. When the page loads, click the toggle to "Enable Socket Mode"
8. Give your token a name, such as `socket-token` and copy the token that is generated (starts with `xapp-`) as we'll need it later.
9. Finally, go to the "Event Subscription" option in the menu bar. Click the toggle to "Enable Events" and subscribe to the `app_mention` bot event.

### 2. Write a Slack bot

Let's start by writing a very simple Slack bot, just to make sure that everything is working. Write the following Python code in a file called `bot.py`, pasting the two tokens from step 6 and step 8 in the previous section.

```
from slack_bolt import App
from slack_bolt.adapter.socket_mode import SocketModeHandler

SLACK_BOT_TOKEN = # PASTE YOUR SLACK BOT TOKEN HERE
SLACK_APP_TOKEN = # PASTE YOUR SLACK APP TOKEN HERE

app = App(token=SLACK_BOT_TOKEN)

@app.event("app_mention")
def handle_app_mention_events(body, say):
    user_id = body["event"]["user"]
    say(f"Hi <@{user_id}>! You mentioned me and said: {body['event']['text']}")

if __name__ == "__main__":
    handler = SocketModeHandler(app, SLACK_APP_TOKEN)
    handler.start()
```

If that is working, we are ready to add Gradio-specific code. We will be using the Gradio Python Client to query the Gradio Playground Space mentioned above. Here's the updated `bot.py` file:

```
from slack_bolt import App
from slack_bolt.adapter.socket_mode import SocketModeHandler

SLACK_BOT_TOKEN = # PASTE YOUR SLACK BOT TOKEN HERE
SLACK_APP_TOKEN = # PASTE YOUR SLACK APP TOKEN HERE

app = App(token=SLACK_BOT_TOKEN)
gradio_client = Client("abidlabs/gradio-playground-bot")

def download_image(url, filename):
    headers = {"Authorization": f"Bearer {SLACK_BOT_TOKEN}"}
    response = httpx.get(url, headers=headers)
    image_path = f"./images/{filename}"
    os.makedirs("./images", exist_ok=True)
    with open(image_path, "wb") as f:
        f.write(response.content)
    return image_path

def slackify_message(message):
    # Replace markdown links with slack format and remove code language specifier after triple backticks
    pattern = r'\[(.*?)\]\((.*?)\)'
    cleaned = re.sub(pattern, r'<\2|\1>', message)
    cleaned = re.sub(r'```\w+\n', '```', cleaned)
    return cleaned.strip()

@app.event("app_mention")
def handle_app_mention_events(body, say):
    # Extract the message content without the bot mention
    text = body["event"]["text"]
    bot_user_id = body["authorizations"][0]["user_id"]
    clean_message = text.replace(f"<@{bot_user_id}>", "").strip()

    # Handle images if present
    files = []
    if "files" in body["event"]:
        for file in body["event"]["files"]:
            if file["filetype"] in ["png", "jpg", "jpeg", "gif", "webp"]:
                image_path = download_image(file["url_private_download"], file["name"])
                files.append(handle_file(image_path))
                break

    # Submit to Gradio and send responses back to Slack
    for response in gradio_client.submit(
        message={"text": clean_message, "files": files},
    ):
        cleaned_response = slackify_message(response[-1])
        say(cleaned_response)

if __name__ == "__main__":
    handler = SocketModeHandler(app, SLACK_APP_TOKEN)
    handler.start()
```

### 3. Add the bot to your Slack Workplace

Now, create a new channel or navigate to an existing channel in your Slack workspace where you want to use the bot. Click the "+" button next to "Channels" in your Slack sidebar and follow the prompts to create a new channel.

Finally, invite your bot to the channel:

1. In your new channel, type `/invite @YourBotName`
2. Select your bot from the dropdown
3. Click "Invite to Channel"

### 4. That's it](#4-thats-it)

Now you can mention your bot in any channel it's in, optionally attach an image, and it will respond with generated Gradio app code!

The bot will:

1. Listen for mentions
2. Process any attached images
3. Send the text and images to your Gradio app
4. Stream the responses back to the Slack channel

This is just a basic example - you can extend it to handle more types of files, add error handling, or integrate with different Gradio apps!

If you build a Slack bot from a Gradio app, feel free to share it on X and tag the Gradio account, and we are happy to help you amplify!

[←

Creating A Discord Bot From A Gradio App](../guides/creating-a-discord-bot-from-a-gradio-app/) [Creating A Website Widget From A Gradio Chatbot

→](../guides/creating-a-website-widget-from-a-gradio-chatbot/)

Status