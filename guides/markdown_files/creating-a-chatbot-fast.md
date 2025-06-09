# Creating A Chatbot Fast

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

Creating A Chatbot Fast

IntroductionNote for OpenAI-API compatible endpointsDefining a chat functionStreaming chatbotsCustomizing the Chat UIMultimodal Chat InterfaceAdditional InputsAdditional OutputsReturning Complex ResponsesModifying the Chatbot Value DirectlyUsing Your Chatbot via APIChat HistoryCollecting User FeedbackWhat's Next?

Chatinterface ExamplesAgents And Tool UsageCreating A Custom Chatbot With BlocksChatbot Specific EventsCreating A Discord Bot From A Gradio AppCreating A Slack Bot From A Gradio AppCreating A Website Widget From A Gradio Chatbot

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
2. Creating A Chatbot Fast

[←

Internationalization](../guides/internationalization/) [Chatinterface Examples

→](../guides/chatinterface-examples/)

# How to Create a Chatbot with Gradio

## Introduction

Chatbots are a popular application of large language models (LLMs). Using Gradio, you can easily build a chat application and share that with your users, or try it yourself using an intuitive UI.

This tutorial uses `gr.ChatInterface()`, which is a high-level abstraction that allows you to create your chatbot UI fast, often with a *few lines of Python*. It can be easily adapted to support multimodal chatbots, or chatbots that require further customization.

**Prerequisites**: please make sure you are using the latest version of Gradio:

```
$ pip install --upgrade gradio
```

## Note for OpenAI-API compatible endpoints

If you have a chat server serving an OpenAI-API compatible endpoint (such as Ollama), you can spin up a ChatInterface in a single line of Python. First, also run `pip install openai`. Then, with your own URL, model, and optional token:

```
import gradio as gr

gr.load_chat("http://localhost:11434/v1/", model="llama3.2", token="***").launch()
```

Read about `gr.load_chat` in the docs. If you have your own model, keep reading to see how to create an application around any chat model in Python!

## Defining a chat function

To create a chat application with `gr.ChatInterface()`, the first thing you should do is define your **chat function**. In the simplest case, your chat function should accept two arguments: `message` and `history` (the arguments can be named anything, but must be in this order).

* `message`: a `str` representing the user's most recent message.
* `history`: a list of openai-style dictionaries with `role` and `content` keys, representing the previous conversation history. May also include additional keys representing message metadata.

For example, the `history` could look like this:

```
[
    {"role": "user", "content": "What is the capital of France?"},
    {"role": "assistant", "content": "Paris"}
]
```

while the next `message` would be:

```
"And what is its largest city?"
```

Your chat function simply needs to return:

* a `str` value, which is the chatbot's response based on the chat `history` and most recent `message`, for example, in this case:

```
Paris is also the largest city.
```

Let's take a look at a few example chat functions:

**Example: a chatbot that randomly responds with yes or no**

Let's write a chat function that responds `Yes` or `No` randomly.

Here's our chat function:

```
import random

def random_response(message, history):
    return random.choice(["Yes", "No"])
```

Now, we can plug this into `gr.ChatInterface()` and call the `.launch()` method to create the web interface:

```
import gradio as gr

gr.ChatInterface(
    fn=random_response,
    type="messages"
).launch()
```

**Tip:**
Always set type="messages" in gr.ChatInterface. The default value (type="tuples") is deprecated and will be removed in a future version of Gradio.

That's it! Here's our running demo, try it out:

**Example: a chatbot that alternates between agreeing and disagreeing**

Of course, the previous example was very simplistic, it didn't take user input or the previous history into account! Here's another simple example showing how to incorporate a user's input as well as the history.

```
import gradio as gr

def alternatingly_agree(message, history):
    if len([h for h in history if h['role'] == "assistant"]) % 2 == 0:
        return f"Yes, I do think that: {message}"
    else:
        return "I don't think so"

gr.ChatInterface(
    fn=alternatingly_agree,
    type="messages"
).launch()
```

We'll look at more realistic examples of chat functions in our next Guide, which shows examples of using `gr.ChatInterface` with popular LLMs.

## Streaming chatbots

In your chat function, you can use `yield` to generate a sequence of partial responses, each replacing the previous ones. This way, you'll end up with a streaming chatbot. It's that simple!

```
import time
import gradio as gr

def slow_echo(message, history):
    for i in range(len(message)):
        time.sleep(0.3)
        yield "You typed: " + message[: i+1]

gr.ChatInterface(
    fn=slow_echo,
    type="messages"
).launch()
```

While the response is streaming, the "Submit" button turns into a "Stop" button that can be used to stop the generator function.

**Tip:**
Even though you are yielding the latest message at each iteration, Gradio only sends the "diff" of each message from the server to the frontend, which reduces latency and data consumption over your network.

## Customizing the Chat UI

If you're familiar with Gradio's `gr.Interface` class, the `gr.ChatInterface` includes many of the same arguments that you can use to customize the look and feel of your Chatbot. For example, you can:

* add a title and description above your chatbot using `title` and `description` arguments.
* add a theme or custom css using `theme` and `css` arguments respectively.
* add `examples` and even enable `cache_examples`, which make your Chatbot easier for users to try it out.
* customize the chatbot (e.g. to change the height or add a placeholder) or textbox (e.g. to add a max number of characters or add a placeholder).

**Adding examples**

You can add preset examples to your `gr.ChatInterface` with the `examples` parameter, which takes a list of string examples. Any examples will appear as "buttons" within the Chatbot before any messages are sent. If you'd like to include images or other files as part of your examples, you can do so by using this dictionary format for each example instead of a string: `{"text": "What's in this image?", "files": ["cheetah.jpg"]}`. Each file will be a separate message that is added to your Chatbot history.

You can change the displayed text for each example by using the `example_labels` argument. You can add icons to each example as well using the `example_icons` argument. Both of these arguments take a list of strings, which should be the same length as the `examples` list.

If you'd like to cache the examples so that they are pre-computed and the results appear instantly, set `cache_examples=True`.

**Customizing the chatbot or textbox component**

If you want to customize the `gr.Chatbot` or `gr.Textbox` that compose the `ChatInterface`, then you can pass in your own chatbot or textbox components. Here's an example of how we to apply the parameters we've discussed in this section:

```
import gradio as gr

def yes_man(message, history):
    if message.endswith("?"):
        return "Yes"
    else:
        return "Ask me anything!"

gr.ChatInterface(
    yes_man,
    type="messages",
    chatbot=gr.Chatbot(height=300),
    textbox=gr.Textbox(placeholder="Ask me a yes or no question", container=False, scale=7),
    title="Yes Man",
    description="Ask Yes Man any question",
    theme="ocean",
    examples=["Hello", "Am I cool?", "Are tomatoes vegetables?"],
    cache_examples=True,
).launch()
```

Here's another example that adds a "placeholder" for your chat interface, which appears before the user has started chatting. The `placeholder` argument of `gr.Chatbot` accepts Markdown or HTML:

```
gr.ChatInterface(
    yes_man,
    type="messages",
    chatbot=gr.Chatbot(placeholder="<strong>Your Personal Yes-Man</strong><br>Ask Me Anything"),
...
```

The placeholder appears vertically and horizontally centered in the chatbot.

## Multimodal Chat Interface

You may want to add multimodal capabilities to your chat interface. For example, you may want users to be able to upload images or files to your chatbot and ask questions about them. You can make your chatbot "multimodal" by passing in a single parameter (`multimodal=True`) to the `gr.ChatInterface` class.

When `multimodal=True`, the signature of your chat function changes slightly: the first parameter of your function (what we referred to as `message` above) should accept a dictionary consisting of the submitted text and uploaded files that looks like this:

```
{
    "text": "user input",
    "files": [
        "updated_file_1_path.ext",
        "updated_file_2_path.ext",
        ...
    ]
}
```

This second parameter of your chat function, `history`, will be in the same openai-style dictionary format as before. However, if the history contains uploaded files, the `content` key for a file will be not a string, but rather a single-element tuple consisting of the filepath. Each file will be a separate message in the history. So after uploading two files and asking a question, your history might look like this:

```
[
    {"role": "user", "content": ("cat1.png")},
    {"role": "user", "content": ("cat2.png")},
    {"role": "user", "content": "What's the difference between these two images?"},
]
```

The return type of your chat function does *not change* when setting `multimodal=True` (i.e. in the simplest case, you should still return a string value). We discuss more complex cases, e.g. returning files below.

If you are customizing a multimodal chat interface, you should pass in an instance of `gr.MultimodalTextbox` to the `textbox` parameter. You can customize the `MultimodalTextbox` further by passing in the `sources` parameter, which is a list of sources to enable. Here's an example that illustrates how to set up and customize and multimodal chat interface:

```
import gradio as gr

def count_images(message, history):
    num_images = len(message["files"])
    total_images = 0
    for message in history:
        if isinstance(message["content"], tuple):
            total_images += 1
    return f"You just uploaded {num_images} images, total uploaded: {total_images+num_images}"

demo = gr.ChatInterface(
    fn=count_images,
    type="messages",
    examples=[
        {"text": "No files", "files": []}
    ],
    multimodal=True,
    textbox=gr.MultimodalTextbox(file_count="multiple", file_types=["image"], sources=["upload", "microphone"])
)

demo.launch()
```

## Additional Inputs

You may want to add additional inputs to your chat function and expose them to your users through the chat UI. For example, you could add a textbox for a system prompt, or a slider that sets the number of tokens in the chatbot's response. The `gr.ChatInterface` class supports an `additional_inputs` parameter which can be used to add additional input components.

The `additional_inputs` parameters accepts a component or a list of components. You can pass the component instances directly, or use their string shortcuts (e.g. `"textbox"` instead of `gr.Textbox()`). If you pass in component instances, and they have *not* already been rendered, then the components will appear underneath the chatbot within a `gr.Accordion()`.

Here's a complete example:

```
import gradio as gr
import time

def echo(message, history, system_prompt, tokens):
    response = f"System prompt: {system_prompt}\n Message: {message}."
    for i in range(min(len(response), int(tokens))):
        time.sleep(0.05)
        yield response[: i + 1]

demo = gr.ChatInterface(
    echo,
    type="messages",
    additional_inputs=[
        gr.Textbox("You are helpful AI.", label="System Prompt"),
        gr.Slider(10, 100),
    ],
)

demo.launch()

```

If the components you pass into the `additional_inputs` have already been rendered in a parent `gr.Blocks()`, then they will *not* be re-rendered in the accordion. This provides flexibility in deciding where to lay out the input components. In the example below, we position the `gr.Textbox()` on top of the Chatbot UI, while keeping the slider underneath.

```
import gradio as gr
import time

def echo(message, history, system_prompt, tokens):
    response = f"System prompt: {system_prompt}\n Message: {message}."
    for i in range(min(len(response), int(tokens))):
        time.sleep(0.05)
        yield response[: i+1]

with gr.Blocks() as demo:
    system_prompt = gr.Textbox("You are helpful AI.", label="System Prompt")
    slider = gr.Slider(10, 100, render=False)

    gr.ChatInterface(
        echo, additional_inputs=[system_prompt, slider], type="messages"
    )

demo.launch()
```

**Examples with additional inputs**

You can also add example values for your additional inputs. Pass in a list of lists to the `examples` parameter, where each inner list represents one sample, and each inner list should be `1 + len(additional_inputs)` long. The first element in the inner list should be the example value for the chat message, and each subsequent element should be an example value for one of the additional inputs, in order. When additional inputs are provided, examples are rendered in a table underneath the chat interface.

If you need to create something even more custom, then its best to construct the chatbot UI using the low-level `gr.Blocks()` API. We have a dedicated guide for that here.

## Additional Outputs

In the same way that you can accept additional inputs into your chat function, you can also return additional outputs. Simply pass in a list of components to the `additional_outputs` parameter in `gr.ChatInterface` and return additional values for each component from your chat function. Here's an example that extracts code and outputs it into a separate `gr.Code` component:

```
import gradio as gr

python_code = """
def fib(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
"""

js_code = """
function fib(n) {
    if (n <= 0) return 0;
    if (n === 1) return 1;
    return fib(n - 1) + fib(n - 2);
}
"""

def chat(message, history):
    if "python" in message.lower():
        return "Type Python or JavaScript to see the code.", gr.Code(language="python", value=python_code)
    elif "javascript" in message.lower():
        return "Type Python or JavaScript to see the code.", gr.Code(language="javascript", value=js_code)
    else:
        return "Please ask about Python or JavaScript.", None

with gr.Blocks() as demo:
    code = gr.Code(render=False)
    with gr.Row():
        with gr.Column():
            gr.Markdown("<center><h1>Write Python or JavaScript</h1></center>")
            gr.ChatInterface(
                chat,
                examples=["Python", "JavaScript"],
                additional_outputs=[code],
                type="messages"
            )
        with gr.Column():
            gr.Markdown("<center><h1>Code Artifacts</h1></center>")
            code.render()

demo.launch()

```

**Note:** unlike the case of additional inputs, the components passed in `additional_outputs` must be already defined in your `gr.Blocks` context -- they are not rendered automatically. If you need to render them after your `gr.ChatInterface`, you can set `render=False` when they are first defined and then `.render()` them in the appropriate section of your `gr.Blocks()` as we do in the example above.

## Returning Complex Responses

We mentioned earlier that in the simplest case, your chat function should return a `str` response, which will be rendered as Markdown in the chatbot. However, you can also return more complex responses as we discuss below:

**Returning files or Gradio components**

Currently, the following Gradio components can be displayed inside the chat interface:

* `gr.Image`
* `gr.Plot`
* `gr.Audio`
* `gr.HTML`
* `gr.Video`
* `gr.Gallery`
* `gr.File`

Simply return one of these components from your function to use it with `gr.ChatInterface`. Here's an example that returns an audio file:

```
import gradio as gr

def music(message, history):
    if message.strip():
        return gr.Audio("https://github.com/gradio-app/gradio/raw/main/test/test_files/audio_sample.wav")
    else:
        return "Please provide the name of an artist"

gr.ChatInterface(
    music,
    type="messages",
    textbox=gr.Textbox(placeholder="Which artist's music do you want to listen to?", scale=7),
).launch()
```

Similarly, you could return image files with `gr.Image`, video files with `gr.Video`, or arbitrary files with the `gr.File` component.

**Returning Multiple Messages**

You can return multiple assistant messages from your chat function simply by returning a `list` of messages, each of which is a valid chat type. This lets you, for example, send a message along with files, as in the following example:

```
import gradio as gr

def echo_multimodal(message, history):
    response = []
    response.append("You wrote: '" + message["text"] + "' and uploaded:")
    if message.get("files"):
        for file in message["files"]:
            response.append(gr.File(value=file))
    return response

demo = gr.ChatInterface(
    echo_multimodal,
    type="messages",
    multimodal=True,
    textbox=gr.MultimodalTextbox(file_count="multiple"),
)

demo.launch()

```

**Displaying intermediate thoughts or tool usage**

The `gr.ChatInterface` class supports displaying intermediate thoughts or tool usage direct in the chatbot.

To do this, you will need to return a `gr.ChatMessage` object from your chat function. Here is the schema of the `gr.ChatMessage` data class as well as two internal typed dictionaries:

```
@dataclass
class ChatMessage:
   content: str | Component
   metadata: MetadataDict = None
   options: list[OptionDict] = None

class MetadataDict(TypedDict):
   title: NotRequired[str]
   id: NotRequired[int | str]
   parent_id: NotRequired[int | str]
   log: NotRequired[str]
   duration: NotRequired[float]
   status: NotRequired[Literal["pending", "done"]]

class OptionDict(TypedDict):
   label: NotRequired[str]
   value: str
```

As you can see, the `gr.ChatMessage` dataclass is similar to the openai-style message format, e.g. it has a "content" key that refers to the chat message content. But it also includes a "metadata" key whose value is a dictionary. If this dictionary includes a "title" key, the resulting message is displayed as an intermediate thought with the title being displayed on top of the thought. Here's an example showing the usage:

```
import gradio as gr
from gradio import ChatMessage
import time

sleep_time = 0.5

def simulate_thinking_chat(message, history):
    start_time = time.time()
    response = ChatMessage(
        content="",
        metadata={"title": "_Thinking_ step-by-step", "id": 0, "status": "pending"}
    )
    yield response

    thoughts = [
        "First, I need to understand the core aspects of the query...",
        "Now, considering the broader context and implications...",
        "Analyzing potential approaches to formulate a comprehensive answer...",
        "Finally, structuring the response for clarity and completeness..."
    ]

    accumulated_thoughts = ""
    for thought in thoughts:
        time.sleep(sleep_time)
        accumulated_thoughts += f"- {thought}\n\n"
        response.content = accumulated_thoughts.strip()
        yield response

    response.metadata["status"] = "done"
    response.metadata["duration"] = time.time() - start_time
    yield response

    response = [
        response,
        ChatMessage(
            content="Based on my thoughts and analysis above, my response is: This dummy repro shows how thoughts of a thinking LLM can be progressively shown before providing its final answer."
        )
    ]
    yield response

demo = gr.ChatInterface(
    simulate_thinking_chat,
    title="Thinking LLM Chat Interface 🤔",
    type="messages",
)

demo.launch()

```

You can even show nested thoughts, which is useful for agent demos in which one tool may call other tools. To display nested thoughts, include "id" and "parent\_id" keys in the "metadata" dictionary. Read our dedicated guide on displaying intermediate thoughts and tool usage for more realistic examples.

**Providing preset responses**

When returning an assistant message, you may want to provide preset options that a user can choose in response. To do this, again, you will again return a `gr.ChatMessage` instance from your chat function. This time, make sure to set the `options` key specifying the preset responses.

As shown in the schema for `gr.ChatMessage` above, the value corresponding to the `options` key should be a list of dictionaries, each with a `value` (a string that is the value that should be sent to the chat function when this response is clicked) and an optional `label` (if provided, is the text displayed as the preset response instead of the `value`).

This example illustrates how to use preset responses:

```
import gradio as gr
import random

example_code = """
Here's an example Python lambda function:

lambda x: x + {}

Is this correct?
"""

def chat(message, history):
    if message == "Yes, that's correct.":
        return "Great!"
    else:
        return gr.ChatMessage(
            content=example_code.format(random.randint(1, 100)),
            options=[
                {"value": "Yes, that's correct.", "label": "Yes"},
                {"value": "No"}
            ]
        )

demo = gr.ChatInterface(
    chat,
    type="messages",
    examples=["Write an example Python lambda function."]
)

demo.launch()

```

## Modifying the Chatbot Value Directly

You may wish to modify the value of the chatbot with your own events, other than those prebuilt in the `gr.ChatInterface`. For example, you could create a dropdown that prefills the chat history with certain conversations or add a separate button to clear the conversation history. The `gr.ChatInterface` supports these events, but you need to use the `gr.ChatInterface.chatbot_value` as the input or output component in such events. In this example, we use a `gr.Radio` component to prefill the the chatbot with certain conversations:

```
import gradio as gr
import random

def prefill_chatbot(choice):
    if choice == "Greeting":
        return [
            {"role": "user", "content": "Hi there!"},
            {"role": "assistant", "content": "Hello! How can I assist you today?"}
        ]
    elif choice == "Complaint":
        return [
            {"role": "user", "content": "I'm not happy with the service."},
            {"role": "assistant", "content": "I'm sorry to hear that. Can you please tell me more about the issue?"}
        ]
    else:
        return []

def random_response(message, history):
    return random.choice(["Yes", "No"])

with gr.Blocks() as demo:
    radio = gr.Radio(["Greeting", "Complaint", "Blank"])
    chat = gr.ChatInterface(random_response, type="messages")
    radio.change(prefill_chatbot, radio, chat.chatbot_value)

demo.launch()

```

## Using Your Chatbot via API

Once you've built your Gradio chat interface and are hosting it on Hugging Face Spaces or somewhere else, then you can query it with a simple API at the `/chat` endpoint. The endpoint just expects the user's message and will return the response, internally keeping track of the message history.

To use the endpoint, you should use either the Gradio Python Client or the Gradio JS client. Or, you can deploy your Chat Interface to other platforms, such as a:

* Discord bot [tutorial]
* Slack bot [tutorial]
* Website widget [tutorial]

## Chat History

You can enable persistent chat history for your ChatInterface, allowing users to maintain multiple conversations and easily switch between them. When enabled, conversations are stored locally and privately in the user's browser using local storage. So if you deploy a ChatInterface e.g. on Hugging Face Spaces, each user will have their own separate chat history that won't interfere with other users' conversations. This means multiple users can interact with the same ChatInterface simultaneously while maintaining their own private conversation histories.

To enable this feature, simply set `gr.ChatInterface(save_history=True)` (as shown in the example in the next section). Users will then see their previous conversations in a side panel and can continue any previous chat or start a new one.

## Collecting User Feedback

To gather feedback on your chat model, set `gr.ChatInterface(flagging_mode="manual")` and users will be able to thumbs-up or thumbs-down assistant responses. Each flagged response, along with the entire chat history, will get saved in a CSV file in the app working directory (this can be configured via the `flagging_dir` parameter).

You can also change the feedback options via `flagging_options` parameter. The default options are "Like" and "Dislike", which appear as the thumbs-up and thumbs-down icons. Any other options appear under a dedicated flag icon. This example shows a ChatInterface that has both chat history (mentioned in the previous section) and user feedback enabled:

```
import time
import gradio as gr

def slow_echo(message, history):
    for i in range(len(message)):
        time.sleep(0.05)
        yield "You typed: " + message[: i + 1]

demo = gr.ChatInterface(
    slow_echo,
    type="messages",
    flagging_mode="manual",
    flagging_options=["Like", "Spam", "Inappropriate", "Other"],
    save_history=True,
)

demo.launch()

```

Note that in this example, we set several flagging options: "Like", "Spam", "Inappropriate", "Other". Because the case-sensitive string "Like" is one of the flagging options, the user will see a thumbs-up icon next to each assistant message. The three other flagging options will appear in a dropdown under the flag icon.

## What's Next?

Now that you've learned about the `gr.ChatInterface` class and how it can be used to create chatbot UIs quickly, we recommend reading one of the following:

* Our next Guide shows examples of how to use `gr.ChatInterface` with popular LLM libraries.
* If you'd like to build very custom chat applications from scratch, you can build them using the low-level Blocks API, as discussed in this Guide.
* Once you've deployed your Gradio Chat Interface, its easy to use it other applications because of the built-in API. Here's a tutorial on how to deploy a Gradio chat interface as a Discord bot.

[←

Internationalization](../guides/internationalization/) [Chatinterface Examples

→](../guides/chatinterface-examples/)

Status