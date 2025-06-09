# Creating A Website Widget From A Gradio Chatbot

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

How does it work?Prerequisites1. Create and Style the Chat Widget2. Add the JavaScript3. That's it!Customization

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
2. Creating A Website Widget From A Gradio Chatbot

[←

Creating A Slack Bot From A Gradio App](../guides/creating-a-slack-bot-from-a-gradio-app/) [Creating Plots

→](../guides/creating-plots/)

# 🚀 Creating a Website Chat Widget with Gradio 🚀

You can make your Gradio Chatbot available as an embedded chat widget on your website, similar to popular customer service widgets like Intercom. This is particularly useful for:

* Adding AI assistance to your documentation pages
* Providing interactive help on your portfolio or product website
* Creating a custom chatbot interface for your Gradio app

## How does it work?

The chat widget appears as a small button in the corner of your website. When clicked, it opens a chat interface that communicates with your Gradio app via the JavaScript Client API. Users can ask questions and receive responses directly within the widget.

## Prerequisites

* A running Gradio app (local or on Hugging Face Spaces). In this example, we'll use the Gradio Playground Space, which helps generate code for Gradio apps based on natural language descriptions.

### 1. Create and Style the Chat Widget

First, add this HTML and CSS to your website:

```
<div id="chat-widget" class="chat-widget">
    <button id="chat-toggle" class="chat-toggle">💬</button>
    <div id="chat-container" class="chat-container hidden">
        <div id="chat-header">
            <h3>Gradio Assistant</h3>
            <button id="close-chat">×</button>
        </div>
        <div id="chat-messages"></div>
        <div id="chat-input-area">
            <input type="text" id="chat-input" placeholder="Ask a question...">
            <button id="send-message">Send</button>
        </div>
    </div>
</div>

<style>
.chat-widget {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 1000;
}

.chat-toggle {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: #007bff;
    border: none;
    color: white;
    font-size: 24px;
    cursor: pointer;
}

.chat-container {
    position: fixed;
    bottom: 80px;
    right: 20px;
    width: 300px;
    height: 400px;
    background: white;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
    display: flex;
    flex-direction: column;
}

.chat-container.hidden {
    display: none;
}

#chat-header {
    padding: 10px;
    background: #007bff;
    color: white;
    border-radius: 10px 10px 0 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

#chat-messages {
    flex-grow: 1;
    overflow-y: auto;
    padding: 10px;
}

#chat-input-area {
    padding: 10px;
    border-top: 1px solid #eee;
    display: flex;
}

#chat-input {
    flex-grow: 1;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-right: 8px;
}

.message {
    margin: 8px 0;
    padding: 8px;
    border-radius: 4px;
}

.user-message {
    background: #e9ecef;
    margin-left: 20px;
}

.bot-message {
    background: #f8f9fa;
    margin-right: 20px;
}
</style>
```

### 2. Add the JavaScript

Then, add the following JavaScript code (which uses the Gradio JavaScript Client to connect to the Space) to your website by including this in the `<head>` section of your website:

```
<script type="module">
    import { Client } from "https://cdn.jsdelivr.net/npm/@gradio/client/dist/index.min.js";

    async function initChatWidget() {
        const client = await Client.connect("https://abidlabs-gradio-playground-bot.hf.space");

        const chatToggle = document.getElementById('chat-toggle');
        const chatContainer = document.getElementById('chat-container');
        const closeChat = document.getElementById('close-chat');
        const chatInput = document.getElementById('chat-input');
        const sendButton = document.getElementById('send-message');
        const messagesContainer = document.getElementById('chat-messages');

        chatToggle.addEventListener('click', () => {
            chatContainer.classList.remove('hidden');
        });

        closeChat.addEventListener('click', () => {
            chatContainer.classList.add('hidden');
        });

        async function sendMessage() {
            const userMessage = chatInput.value.trim();
            if (!userMessage) return;

            appendMessage(userMessage, 'user');
            chatInput.value = '';

            try {
                const result = await client.predict("/chat", {
                    message: {"text": userMessage, "files": []}
                });
                const message = result.data[0];
                console.log(result.data[0]);
                const botMessage = result.data[0].join('\n');
                appendMessage(botMessage, 'bot');
            } catch (error) {
                console.error('Error:', error);
                appendMessage('Sorry, there was an error processing your request.', 'bot');
            }
        }

        function appendMessage(text, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${sender}-message`;

            if (sender === 'bot') {
                messageDiv.innerHTML = marked.parse(text);
            } else {
                messageDiv.textContent = text;
            }

            messagesContainer.appendChild(messageDiv);
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        sendButton.addEventListener('click', sendMessage);
        chatInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') sendMessage();
        });
    }

    initChatWidget();
</script>
```

### 3. That's it](#3-thats-it)

Your website now has a chat widget that connects to your Gradio app! Users can click the chat button to open the widget and start interacting with your app.

### Customization

You can customize the appearance of the widget by modifying the CSS. Some ideas:

* Change the colors to match your website's theme
* Adjust the size and position of the widget
* Add animations for opening/closing
* Modify the message styling

If you build a website widget from a Gradio app, feel free to share it on X and tag the Gradio account, and we are happy to help you amplify!

[←

Creating A Slack Bot From A Gradio App](../guides/creating-a-slack-bot-from-a-gradio-app/) [Creating Plots

→](../guides/creating-plots/)

Status