# Chatbot Specific Events

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

Creating A Chatbot FastChatinterface ExamplesAgents And Tool UsageCreating A Custom Chatbot With BlocksChatbot Specific Events

The UIThe Undo EventThe Retry EventThe Like EventThe Edit EventThe Clear EventConclusion

Creating A Discord Bot From A Gradio AppCreating A Slack Bot From A Gradio AppCreating A Website Widget From A Gradio Chatbot

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
2. Chatbot Specific Events

[←

Creating A Custom Chatbot With Blocks](../guides/creating-a-custom-chatbot-with-blocks/) [Creating A Discord Bot From A Gradio App

→](../guides/creating-a-discord-bot-from-a-gradio-app/)

# Chatbot-Specific Events

Users expect modern chatbot UIs to let them easily interact with individual chat messages: for example, users might want to retry message generations, undo messages, or click on a like/dislike button to upvote or downvote a generated message.

Thankfully, the Gradio Chatbot exposes several events, such as `.retry`, `.undo`, `.like`, and `.clear`, to let you build this functionality into your application. As an application developer, you can attach functions to any of these event, allowing you to run arbitrary Python functions e.g. when a user interacts with a message.

In this demo, we'll build a UI that implements these events. You can see our finished demo deployed on Hugging Face spaces here:

**Tip:**
`gr.ChatInterface` automatically uses the `retry` and `.undo` events so it's best to start there in order get a fully working application quickly.

## The UI

First, we'll build the UI without handling these events and build from there.
We'll use the Hugging Face InferenceClient in order to get started without setting up
any API keys.

This is what the first draft of our application looks like:

```
from huggingface_hub import InferenceClient
import gradio as gr

client = InferenceClient()

def respond(
    prompt: str,
    history,
):
    if not history:
        history = [{"role": "system", "content": "You are a friendly chatbot"}]
    history.append({"role": "user", "content": prompt})

    yield history

    response = {"role": "assistant", "content": ""}
    for message in client.chat_completion(
        history,
        temperature=0.95,
        top_p=0.9,
        max_tokens=512,
        stream=True,
        model="HuggingFaceH4/zephyr-7b-beta"
    ):
        response["content"] += message.choices[0].delta.content or ""

        yield history + [response]

with gr.Blocks() as demo:
    gr.Markdown("# Chat with Hugging Face Zephyr 7b 🤗")
    chatbot = gr.Chatbot(
        label="Agent",
        type="messages",
        avatar_images=(
            None,
            "https://em-content.zobj.net/source/twitter/376/hugging-face_1f917.png",
        ),
    )
    prompt = gr.Textbox(max_lines=1, label="Chat Message")
    prompt.submit(respond, [prompt, chatbot], [chatbot])
    prompt.submit(lambda: "", None, [prompt])

if __name__ == "__main__":
    demo.launch()
```

## The Undo Event

Our undo event will populate the textbox with the previous user message and also remove all subsequent assistant responses.

In order to know the index of the last user message, we can pass `gr.UndoData` to our event handler function like so:

```
def handle_undo(history, undo_data: gr.UndoData):
    return history[:undo_data.index], history[undo_data.index]['content']
```

We then pass this function to the `undo` event!

```
    chatbot.undo(handle_undo, chatbot, [chatbot, prompt])
```

You'll notice that every bot response will now have an "undo icon" you can use to undo the response -

**Tip:**
You can also access the content of the user message with `undo\_data.value`

## The Retry Event

The retry event will work similarly. We'll use `gr.RetryData` to get the index of the previous user message and remove all the subsequent messages from the history. Then we'll use the `respond` function to generate a new response. We could also get the previous prompt via the `value` property of `gr.RetryData`.

```
def handle_retry(history, retry_data: gr.RetryData):
    new_history = history[:retry_data.index]
    previous_prompt = history[retry_data.index]['content']
    yield from respond(previous_prompt, new_history)

...

chatbot.retry(handle_retry, chatbot, chatbot)
```

You'll see that the bot messages have a "retry" icon now -

**Tip:**
The Hugging Face inference API caches responses, so in this demo, the retry button will not generate a new response.

## The Like Event

By now you should hopefully be seeing the pattern!
To let users like a message, we'll add a `.like` event to our chatbot.
We'll pass it a function that accepts a `gr.LikeData` object.
In this case, we'll just print the message that was either liked or disliked.

```
def handle_like(data: gr.LikeData):
    if data.liked:
        print("You upvoted this response: ", data.value)
    else:
        print("You downvoted this response: ", data.value)

...

chatbot.like(vote, None, None)
```

## The Edit Event

Same idea with the edit listener! with `gr.Chatbot(editable=True)`, you can capture user edits. The `gr.EditData` object tells us the index of the message edited and the new text of the mssage. Below, we use this object to edit the history, and delete any subsequent messages.

```
def handle_edit(history, edit_data: gr.EditData):
    new_history = history[:edit_data.index]
    new_history[-1]['content'] = edit_data.value
    return new_history

...

chatbot.edit(handle_edit, chatbot, chatbot)
```

## The Clear Event

As a bonus, we'll also cover the `.clear()` event, which is triggered when the user clicks the clear icon to clear all messages. As a developer, you can attach additional events that should happen when this icon is clicked, e.g. to handle clearing of additional chatbot state:

```
from uuid import uuid4
import gradio as gr

def clear():
    print("Cleared uuid")
    return uuid4()

def chat_fn(user_input, history, uuid):
    return f"{user_input} with uuid {uuid}"

with gr.Blocks() as demo:
    uuid_state = gr.State(
        uuid4
    )
    chatbot = gr.Chatbot(type="messages")
    chatbot.clear(clear, outputs=[uuid_state])

    gr.ChatInterface(
        chat_fn,
        additional_inputs=[uuid_state],
        chatbot=chatbot,
        type="messages"
    )

demo.launch()
```

In this example, the `clear` function, bound to the `chatbot.clear` event, returns a new UUID into our session state, when the chat history is cleared via the trash icon. This can be seen in the `chat_fn` function, which references the UUID saved in our session state.

This example also shows that you can use these events with `gr.ChatInterface` by passing in a custom `gr.Chatbot` object.

## Conclusion

That's it! You now know how you can implement the retry, undo, like, and clear events for the Chatbot.

[←

Creating A Custom Chatbot With Blocks](../guides/creating-a-custom-chatbot-with-blocks/) [Creating A Discord Bot From A Gradio App

→](../guides/creating-a-discord-bot-from-a-gradio-app/)

Status