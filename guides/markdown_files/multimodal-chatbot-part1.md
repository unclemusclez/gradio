# Multimodal Chatbot Part1

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

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1

Part 1 - Creating our projectPart 2a - The backend data\_modelPart 2b - The pre and postprocess methodsPart 3a - The Index.svelte filePart 3b - the Chatbot.svelte filePart 4 - The demoPart 5 - Deploying and Conclusion

Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Custom Components
2. Multimodal Chatbot Part1

[←

Pdf Component Example](../guides/pdf-component-example/) [Documenting Custom Components

→](../guides/documenting-custom-components/)

# Build a Custom Multimodal Chatbot - Part 1

This is the first in a two part series where we build a custom Multimodal Chatbot component.
In part 1, we will modify the Gradio Chatbot component to display text and media files (video, audio, image) in the same message.
In part 2, we will build a custom Textbox component that will be able to send multimodal messages (text and media files) to the chatbot.

You can follow along with the author of this post as he implements the chatbot component in the following YouTube video!

Here's a preview of what our multimodal chatbot component will look like:

## Part 1 - Creating our project

For this demo we will be tweaking the existing Gradio `Chatbot` component to display text and media files in the same message.
Let's create a new custom component directory by templating off of the `Chatbot` component source code.

```
gradio cc create MultimodalChatbot --template Chatbot
```

And we're ready to go!

**Tip:**
Make sure to modify the `Author` key in the `pyproject.toml` file.

## Part 2a - The backend data\_model

Open up the `multimodalchatbot.py` file in your favorite code editor and let's get started modifying the backend of our component.

The first thing we will do is create the `data_model` of our component.
The `data_model` is the data format that your python component will receive and send to the javascript client running the UI.
You can read more about the `data_model` in the backend guide.

For our component, each chatbot message will consist of two keys: a `text` key that displays the text message and an optional list of media files that can be displayed underneath the text.

Import the `FileData` and `GradioModel` classes from `gradio.data_classes` and modify the existing `ChatbotData` class to look like the following:

```
class FileMessage(GradioModel):
    file: FileData
    alt_text: Optional[str] = None

class MultimodalMessage(GradioModel):
    text: Optional[str] = None
    files: Optional[List[FileMessage]] = None

class ChatbotData(GradioRootModel):
    root: List[Tuple[Optional[MultimodalMessage], Optional[MultimodalMessage]]]

class MultimodalChatbot(Component):
    ...
    data_model = ChatbotData
```

**Tip:**
The `data\_model`s are implemented using `Pydantic V2`. Read the documentation here.

We've done the hardest part already!

## Part 2b - The pre and postprocess methods

For the `preprocess` method, we will keep it simple and pass a list of `MultimodalMessage`s to the python functions that use this component as input.
This will let users of our component access the chatbot data with `.text` and `.files` attributes.
This is a design choice that you can modify in your implementation!
We can return the list of messages with the `root` property of the `ChatbotData` like so:

```
def preprocess(
    self,
    payload: ChatbotData | None,
) -> List[MultimodalMessage] | None:
    if payload is None:
        return payload
    return payload.root
```

**Tip:**
Learn about the reasoning behind the `preprocess` and `postprocess` methods in the key concepts guide

In the `postprocess` method we will coerce each message returned by the python function to be a `MultimodalMessage` class.
We will also clean up any indentation in the `text` field so that it can be properly displayed as markdown in the frontend.

We can leave the `postprocess` method as is and modify the `_postprocess_chat_messages`

```
def _postprocess_chat_messages(
    self, chat_message: MultimodalMessage | dict | None
) -> MultimodalMessage | None:
    if chat_message is None:
        return None
    if isinstance(chat_message, dict):
        chat_message = MultimodalMessage(**chat_message)
    chat_message.text = inspect.cleandoc(chat_message.text or "")
    for file_ in chat_message.files:
        file_.file.mime_type = client_utils.get_mimetype(file_.file.path)
    return chat_message
```

Before we wrap up with the backend code, let's modify the `example_value` and `example_payload` method to return a valid dictionary representation of the `ChatbotData`:

```
def example_value(self) -> Any:
    return [[{"text": "Hello!", "files": []}, None]]

def example_payload(self) -> Any:
    return [[{"text": "Hello!", "files": []}, None]]
```

Congrats - the backend is complete!

## Part 3a - The Index.svelte file

The frontend for the `Chatbot` component is divided into two parts - the `Index.svelte` file and the `shared/Chatbot.svelte` file.
The `Index.svelte` file applies some processing to the data received from the server and then delegates the rendering of the conversation to the `shared/Chatbot.svelte` file.
First we will modify the `Index.svelte` file to apply processing to the new data type the backend will return.

Let's begin by porting our custom types from our python `data_model` to typescript.
Open `frontend/shared/utils.ts` and add the following type definitions at the top of the file:

```
export type FileMessage = {
	file: FileData;
	alt_text?: string;
};

export type MultimodalMessage = {
	text: string;
	files?: FileMessage[];
}
```

Now let's import them in `Index.svelte` and modify the type annotations for `value` and `_value`.

```
import type { FileMessage, MultimodalMessage } from "./shared/utils";

export let value: [
    MultimodalMessage | null,
    MultimodalMessage | null
][] = [];

let _value: [
    MultimodalMessage | null,
    MultimodalMessage | null
][];
```

We need to normalize each message to make sure each file has a proper URL to fetch its contents from.
We also need to format any embedded file links in the `text` key.
Let's add a `process_message` utility function and apply it whenever the `value` changes.

```
function process_message(msg: MultimodalMessage | null): MultimodalMessage | null {
    if (msg === null) {
        return msg;
    }
    msg.text = redirect_src_url(msg.text);
    msg.files = msg.files.map(normalize_messages);
    return msg;
}

$: _value = value
    ? value.map(([user_msg, bot_msg]) => [
            process_message(user_msg),
            process_message(bot_msg)
        ])
    : [];
```

## Part 3b - the Chatbot.svelte file

Let's begin similarly to the `Index.svelte` file and let's first modify the type annotations.
Import `Mulimodal` message at the top of the `<script>` section and use it to type the `value` and `old_value` variables.

```
import type { MultimodalMessage } from "./utils";

export let value:
    | [
            MultimodalMessage | null,
            MultimodalMessage | null
        ][]
    | null;
let old_value:
    | [
            MultimodalMessage | null,
            MultimodalMessage | null
        ][]
    | null = null;
```

We also need to modify the `handle_select` and `handle_like` functions:

```
function handle_select(
    i: number,
    j: number,
    message: MultimodalMessage | null
): void {
    dispatch("select", {
        index: [i, j],
        value: message
    });
}

function handle_like(
    i: number,
    j: number,
    message: MultimodalMessage | null,
    liked: boolean
): void {
    dispatch("like", {
        index: [i, j],
        value: message,
        liked: liked
    });
}
```

Now for the fun part, actually rendering the text and files in the same message!

You should see some code like the following that determines whether a file or a markdown message should be displayed depending on the type of the message:

```
{#if typeof message === "string"}
    <Markdown
        {message}
        {latex_delimiters}
        {sanitize_html}
        {render_markdown}
        {line_breaks}
        on:load={scroll}
    />
{:else if message !== null && message.file?.mime_type?.includes("audio")}
    <audio
        data-testid="chatbot-audio"
        controls
        preload="metadata"
        ...
```

We will modify this code to always display the text message and then loop through the files and display all of them that are present:

```
<Markdown
    message={message.text}
    {latex_delimiters}
    {sanitize_html}
    {render_markdown}
    {line_breaks}
    on:load={scroll}
/>
{#each message.files as file, k}
    {#if file !== null && file.file.mime_type?.includes("audio")}
        <audio
            data-testid="chatbot-audio"
            controls
            preload="metadata"
            src={file.file?.url}
            title={file.alt_text}
            on:play
            on:pause
            on:ended
        />
    {:else if message !== null && file.file?.mime_type?.includes("video")}
        <video
            data-testid="chatbot-video"
            controls
            src={file.file?.url}
            title={file.alt_text}
            preload="auto"
            on:play
            on:pause
            on:ended
        >
            <track kind="captions" />
        </video>
    {:else if message !== null && file.file?.mime_type?.includes("image")}
        <img
            data-testid="chatbot-image"
            src={file.file?.url}
            alt={file.alt_text}
        />
    {:else if message !== null && file.file?.url !== null}
        <a
            data-testid="chatbot-file"
            href={file.file?.url}
            target="_blank"
            download={window.__is_colab__
                ? null
                : file.file?.orig_name || file.file?.path}
        >
            {file.file?.orig_name || file.file?.path}
        </a>
    {:else if pending_message && j === 1}
        <Pending {layout} />
    {/if}
{/each}
```

We did it! 🎉

## Part 4 - The demo

For this tutorial, let's keep the demo simple and just display a static conversation between a hypothetical user and a bot.
This demo will show how both the user and the bot can send files.
In part 2 of this tutorial series we will build a fully functional chatbot demo!

The demo code will look like the following:

```
import gradio as gr
from gradio_multimodalchatbot import MultimodalChatbot
from gradio.data_classes import FileData

user_msg1 = {"text": "Hello, what is in this image?",
             "files": [{"file": FileData(path="https://gradio-builds.s3.amazonaws.com/diffusion_image/cute_dog.jpg")}]
             }
bot_msg1 = {"text": "It is a very cute dog",
            "files": []}

user_msg2 = {"text": "Describe this audio clip please.",
             "files": [{"file": FileData(path="cantina.wav")}]}
bot_msg2 = {"text": "It is the cantina song from Star Wars",
            "files": []}

user_msg3 = {"text": "Give me a video clip please.",
             "files": []}
bot_msg3 = {"text": "Here is a video clip of the world",
            "files": [{"file": FileData(path="world.mp4")},
                      {"file": FileData(path="cantina.wav")}]}

conversation = [[user_msg1, bot_msg1], [user_msg2, bot_msg2], [user_msg3, bot_msg3]]

with gr.Blocks() as demo:
    MultimodalChatbot(value=conversation, height=800)

demo.launch()
```

**Tip:**
Change the filepaths so that they correspond to files on your machine. Also, if you are running in development mode, make sure the files are located in the top level of your custom component directory.

## Part 5 - Deploying and Conclusion

Let's build and deploy our demo with `gradio cc build` and `gradio cc deploy`!

You can check out our component deployed to HuggingFace Spaces and all of the source code is available here.

See you in the next installment of this series!

[←

Pdf Component Example](../guides/pdf-component-example/) [Documenting Custom Components

→](../guides/documenting-custom-components/)

Status