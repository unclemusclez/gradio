# Backend

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

Custom Components In Five MinutesKey Component ConceptsConfigurationBackend

Which Class to Inherit FromThe methods you need to implementpreprocess and postprocessprocess\_exampleapi\_infoexample\_payloadexample\_valueflagread\_from\_flagThe data\_modelHandling FilesAdding Event Triggers To Your ComponentConclusion

FrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Custom Components
2. Backend

[←

Configuration](../guides/configuration/) [Frontend

→](../guides/frontend/)

# The Backend 🐍

This guide will cover everything you need to know to implement your custom component's backend processing.

## Which Class to Inherit From

All components inherit from one of three classes `Component`, `FormComponent`, or `BlockContext`.
You need to inherit from one so that your component behaves like all other gradio components.
When you start from a template with `gradio cc create --template`, you don't need to worry about which one to choose since the template uses the correct one.
For completeness, and in the event that you need to make your own component from scratch, we explain what each class is for.

* `FormComponent`: Use this when you want your component to be grouped together in the same `Form` layout with other `FormComponents`. The `Slider`, `Textbox`, and `Number` components are all `FormComponents`.
* `BlockContext`: Use this when you want to place other components "inside" your component. This enabled `with MyComponent() as component:` syntax.
* `Component`: Use this for all other cases.

  **Tip:**
  If your component supports streaming output, inherit from the `StreamingOutput` class.

  **Tip:**
  If you inherit from `BlockContext`, you also need to set the metaclass to be `ComponentMeta`. See example below.

```
from gradio.blocks import BlockContext
from gradio.component_meta import ComponentMeta

@document()
class Row(BlockContext, metaclass=ComponentMeta):
    pass
```

## The methods you need to implement

When you inherit from any of these classes, the following methods must be implemented.
Otherwise the Python interpreter will raise an error when you instantiate your component!

### `preprocess` and `postprocess`

Explained in the Key Concepts guide.
They handle the conversion from the data sent by the frontend to the format expected by the python function.

```
    def preprocess(self, x: Any) -> Any:
        """
        Convert from the web-friendly (typically JSON) value in the frontend to the format expected by the python function.
        """
        return x

    def postprocess(self, y):
        """
        Convert from the data returned by the python function to the web-friendly (typically JSON) value expected by the frontend.
        """
        return y
```

### `process_example`

Takes in the original Python value and returns the modified value that should be displayed in the examples preview in the app.
If not provided, the `.postprocess()` method is used instead. Let's look at the following example from the `SimpleDropdown` component.

```
def process_example(self, input_data):
    return next((c[0] for c in self.choices if c[1] == input_data), None)
```

Since `self.choices` is a list of tuples corresponding to (`display_name`, `value`), this converts the value that a user provides to the display value (or if the value is not present in `self.choices`, it is converted to `None`).

### `api_info`

A JSON-schema representation of the value that the `preprocess` expects.
This powers api usage via the gradio clients.
You do **not** need to implement this yourself if you components specifies a `data_model`.
The `data_model` in the following section.

```
def api_info(self) -> dict[str, list[str]]:
    """
    A JSON-schema representation of the value that the `preprocess` expects and the `postprocess` returns.
    """
    pass
```

### `example_payload`

An example payload for your component, e.g. something that can be passed into the `.preprocess()` method
of your component. The example input is displayed in the `View API` page of a Gradio app that uses your custom component.
Must be JSON-serializable. If your component expects a file, it is best to use a publicly accessible URL.

```
def example_payload(self) -> Any:
    """
    The example inputs for this component for API usage. Must be JSON-serializable.
    """
    pass
```

### `example_value`

An example value for your component, e.g. something that can be passed into the `.postprocess()` method
of your component. This is used as the example value in the default app that is created in custom component development.

```
def example_payload(self) -> Any:
    """
    The example inputs for this component for API usage. Must be JSON-serializable.
    """
    pass
```

### `flag`

Write the component's value to a format that can be stored in the `csv` or `json` file used for flagging.
You do **not** need to implement this yourself if you components specifies a `data_model`.
The `data_model` in the following section.

```
def flag(self, x: Any | GradioDataModel, flag_dir: str | Path = "") -> str:
    pass
```

### `read_from_flag`

Convert from the format stored in the `csv` or `json` file used for flagging to the component's python `value`.
You do **not** need to implement this yourself if you components specifies a `data_model`.
The `data_model` in the following section.

```
def read_from_flag(
    self,
    x: Any,
) -> GradioDataModel | Any:
    """
    Convert the data from the csv or jsonl file into the component state.
    """
    return x
```

## The `data_model`

The `data_model` is how you define the expected data format your component's value will be stored in the frontend.
It specifies the data format your `preprocess` method expects and the format the `postprocess` method returns.
It is not necessary to define a `data_model` for your component but it greatly simplifies the process of creating a custom component.
If you define a custom component you only need to implement four methods - `preprocess`, `postprocess`, `example_payload`, and `example_value`!

You define a `data_model` by defining a pydantic model that inherits from either `GradioModel` or `GradioRootModel`.

This is best explained with an example. Let's look at the core `Video` component, which stores the video data as a JSON object with two keys `video` and `subtitles` which point to separate files.

```
from gradio.data_classes import FileData, GradioModel

class VideoData(GradioModel):
    video: FileData
    subtitles: Optional[FileData] = None

class Video(Component):
    data_model = VideoData
```

By adding these four lines of code, your component automatically implements the methods needed for API usage, the flagging methods, and example caching methods!
It also has the added benefit of self-documenting your code.
Anyone who reads your component code will know exactly the data it expects.

**Tip:**
If your component expects files to be uploaded from the frontend, your must use the `FileData` model! It will be explained in the following section.

**Tip:**
Read the pydantic docs here.

The difference between a `GradioModel` and a `GradioRootModel` is that the `RootModel` will not serialize the data to a dictionary.
For example, the `Names` model will serialize the data to `{'names': ['freddy', 'pete']}` whereas the `NamesRoot` model will serialize it to `['freddy', 'pete']`.

```
from typing import List

class Names(GradioModel):
    names: List[str]

class NamesRoot(GradioRootModel):
    root: List[str]
```

Even if your component does not expect a "complex" JSON data structure it can be beneficial to define a `GradioRootModel` so that you don't have to worry about implementing the API and flagging methods.

**Tip:**
Use classes from the Python typing library to type your models. e.g. `List` instead of `list`.

## Handling Files

If your component expects uploaded files as input, or returns saved files to the frontend, you **MUST** use the `FileData` to type the files in your `data_model`.

When you use the `FileData`:

* Gradio knows that it should allow serving this file to the frontend. Gradio automatically blocks requests to serve arbitrary files in the computer running the server.
* Gradio will automatically place the file in a cache so that duplicate copies of the file don't get saved.
* The client libraries will automatically know that they should upload input files prior to sending the request. They will also automatically download files.

If you do not use the `FileData`, your component will not work as expected!

## Adding Event Triggers To Your Component

The events triggers for your component are defined in the `EVENTS` class attribute.
This is a list that contains the string names of the events.
Adding an event to this list will automatically add a method with that same name to your component!

You can import the `Events` enum from `gradio.events` to access commonly used events in the core gradio components.

For example, the following code will define `text_submit`, `file_upload` and `change` methods in the `MyComponent` class.

```
from gradio.events import Events
from gradio.components import FormComponent

class MyComponent(FormComponent):

    EVENTS = [
        "text_submit",
        "file_upload",
        Events.change
    ]
```

**Tip:**
Don't forget to also handle these events in the JavaScript code!

## Conclusion

[←

Configuration](../guides/configuration/) [Frontend

→](../guides/frontend/)

Status