# Documenting Custom Components

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

How do I use it?What gets generated?What do I need to do?Python versionType hintsWhat do I need to add hints to?\_\_init\_\_preprocess and postprocessDocstringsClassesMethods and functionsEventsBuilt-in eventsCustom eventsDemoDemo recommendationsKeep the demo compactKeep the code conciseAvoid external dependenciesEnsure the demo directory is self-containedAdditional URLs

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Custom Components
2. Documenting Custom Components

[←

Multimodal Chatbot Part1](../guides/multimodal-chatbot-part1/) [Getting Started With The Python Client

→](../guides/getting-started-with-the-python-client/)

# Documenting Custom Components

In 4.15, we added a new `gradio cc docs` command to the Gradio CLI to generate rich documentation for your custom component. This command will generate documentation for users automatically, but to get the most out of it, you need to do a few things.

## How do I use it?

The documentation will be generated when running `gradio cc build`. You can pass the `--no-generate-docs` argument to turn off this behaviour.

There is also a standalone `docs` command that allows for greater customisation. If you are running this command manually it should be run *after* the `version` in your `pyproject.toml` has been bumped but before building the component.

All arguments are optional.

```
gradio cc docs
  path # The directory of the custom component.
  --demo-dir # Path to the demo directory.
  --demo-name # Name of the demo file
  --space-url # URL of the Hugging Face Space to link to
  --generate-space # create a documentation space.
  --no-generate-space # do not create a documentation space
  --readme-path # Path to the README.md file.
  --generate-readme # create a REAMDE.md file
  --no-generate-readme # do not create a README.md file
  --suppress-demo-check # suppress validation checks and warnings
```

## What gets generated?

The `gradio cc docs` command will generate an interactive Gradio app and a static README file with various features. You can see an example here:

* Gradio app deployed on Hugging Face Spaces
* README.md rendered by GitHub

The README.md and space both have the following features:

* A description.
* Installation instructions.
* A fully functioning code snippet.
* Optional links to PyPi, GitHub, and Hugging Face Spaces.
* API documentation including:
  + An argument table for component initialisation showing types, defaults, and descriptions.
  + A description of how the component affects the user's predict function.
  + A table of events and their descriptions.
  + Any additional interfaces or classes that may be used during initialisation or in the pre- or post- processors.

Additionally, the Gradio includes:

* A live demo.
* A richer, interactive version of the parameter tables.
* Nicer styling!

## What do I need to do?

The documentation generator uses existing standards to extract the necessary information, namely Type Hints and Docstrings. There are no Gradio-specific APIs for documentation, so following best practices will generally yield the best results.

If you already use type hints and docstrings in your component source code, you don't need to do much to benefit from this feature, but there are some details that you should be aware of.

### Python version

To get the best documentation experience, you need to use Python `3.10` or greater when generating documentation. This is because some introspection features used to generate the documentation were only added in `3.10`.

### Type hints

Python type hints are used extensively to provide helpful information for users.

 What are type hints?

If you need to become more familiar with type hints in Python, they are a simple way to express what Python types are expected for arguments and return values of functions and methods. They provide a helpful in-editor experience, aid in maintenance, and integrate with various other tools. These types can be simple primitives, like `list` `str` `bool`; they could be more compound types like `liststr]`, `str | None` or `tuple[str, float | int]`; or they can be more complex types using utility classed like [`TypedDict`.

Read more about type hints in Python.

#### What do I need to add hints to?

You do not need to add type hints to every part of your code. For the documentation to work correctly, you will need to add type hints to the following component methods:

* `__init__` parameters should be typed.
* `postprocess` parameters and return value should be typed.
* `preprocess` parameters and return value should be typed.

If you are using `gradio cc create`, these types should already exist, but you may need to tweak them based on any changes you make.

##### `__init__`

Here, you only need to type the parameters. If you have cloned a template with `gradio` cc create`, these should already be in place. You will only need to add new hints for anything you have added or changed:

```
def __init__(
  self,
  value: str | None = None,
  *,
  sources: Literal["upload", "microphone"] = "upload,
  every: Timer | float | None = None,
  ...
):
  ...
```

##### `preprocess` and `postprocess`

The `preprocess` and `postprocess` methods determine the value passed to the user function and the value that needs to be returned.

Even if the design of your component is primarily as an input or an output, it is worth adding type hints to both the input parameters and the return values because Gradio has no way of limiting how components can be used.

In this case, we specifically care about:

* The return type of `preprocess`.
* The input type of `postprocess`.

```
def preprocess(
  self, payload: FileData | None # input is optional
) -> tuple[int, str] | str | None:

# user function input  is the preprocess return ▲
# user function output is the postprocess input ▼

def postprocess(
  self, value: tuple[int, str] | None
) -> FileData | bytes | None: # return is optional
  ...
```

### Docstrings

Docstrings are also used extensively to extract more meaningful, human-readable descriptions of certain parts of the API.

 What are docstrings?

If you need to become more familiar with docstrings in Python, they are a way to annotate parts of your code with human-readable decisions and explanations. They offer a rich in-editor experience like type hints, but unlike type hints, they don't have any specific syntax requirements. They are simple strings and can take almost any form. The only requirement is where they appear. Docstrings should be "a string literal that occurs as the first statement in a module, function, class, or method definition".

Read more about Python docstrings.

While docstrings don't have any syntax requirements, we need a particular structure for documentation purposes.

As with type hint, the specific information we care about is as follows:

* `__init__` parameter docstrings.
* `preprocess` return docstrings.
* `postprocess` input parameter docstrings.

Everything else is optional.

Docstrings should always take this format to be picked up by the documentation generator:

#### Classes

```
"""
A description of the class.

This can span multiple lines and can _contain_ *markdown*.
"""
```

#### Methods and functions

Markdown in these descriptions will not be converted into formatted text.

```
"""
Parameters:
    param_one: A description for this parameter.
    param_two: A description for this parameter.
Returns:
    A description for this return value.
"""
```

### Events

In custom components, events are expressed as a list stored on the `events` field of the component class. While we do not need types for events, we *do* need a human-readable description so users can understand the behaviour of the event.

To facilitate this, we must create the event in a specific way.

There are two ways to add events to a custom component.

#### Built-in events

Gradio comes with a variety of built-in events that may be enough for your component. If you are using built-in events, you do not need to do anything as they already have descriptions we can extract:

```
from gradio.events import Events

class ParamViewer(Component):
  ...

  EVENTS = [
    Events.change,
    Events.upload,
  ]
```

#### Custom events

You can define a custom event if the built-in events are unsuitable for your use case. This is a straightforward process, but you must create the event in this way for docstrings to work correctly:

```
from gradio.events import Events, EventListener

class ParamViewer(Component):
  ...

  EVENTS = [
    Events.change,
    EventListener(
        "bingbong",
        doc="This listener is triggered when the user does a bingbong."
      )
  ]
```

### Demo

The `demo/app.py`, often used for developing the component, generates the live demo and code snippet. The only strict rule here is that the `demo.launch()` command must be contained with a `__name__ == "__main__"` conditional as below:

```
if __name__ == "__main__":
  demo.launch()
```

The documentation generator will scan for such a clause and error if absent. If you are *not* launching the demo inside the `demo/app.py`, then you can pass `--suppress-demo-check` to turn off this check.

#### Demo recommendations

Although there are no additional rules, there are some best practices you should bear in mind to get the best experience from the documentation generator.

These are only guidelines, and every situation is unique, but they are sound principles to remember.

##### Keep the demo compact

Compact demos look better and make it easier for users to understand what the demo does. Try to remove as many extraneous UI elements as possible to focus the users' attention on the core use case.

Sometimes, it might make sense to have a `demo/app.py` just for the docs and an additional, more complex app for your testing purposes. You can also create other spaces, showcasing more complex examples and linking to them from the main class docstring or the `pyproject.toml` description.

#### Keep the code concise

The 'getting started' snippet utilises the demo code, which should be as short as possible to keep users engaged and avoid confusion.

It isn't the job of the sample snippet to demonstrate the whole API; this snippet should be the shortest path to success for a new user. It should be easy to type or copy-paste and easy to understand. Explanatory comments should be brief and to the point.

#### Avoid external dependencies

As mentioned above, users should be able to copy-paste a snippet and have a fully working app. Try to avoid third-party library dependencies to facilitate this.

You should carefully consider any examples; avoiding examples that require additional files or that make assumptions about the environment is generally a good idea.

#### Ensure the `demo` directory is self-contained

Only the `demo` directory will be uploaded to Hugging Face spaces in certain instances, as the component will be installed via PyPi if possible. It is essential that this directory is self-contained and any files needed for the correct running of the demo are present.

### Additional URLs

The documentation generator will generate a few buttons, providing helpful information and links to users. They are obtained automatically in some cases, but some need to be explicitly included in the `pyproject.yaml`.

* PyPi Version and link - This is generated automatically.
* GitHub Repository - This is populated via the `pyproject.toml`'s `project.urls.repository`.
* Hugging Face Space - This is populated via the `pyproject.toml`'s `project.urls.space`.

An example `pyproject.toml` urls section might look like this:

```
[project.urls]
repository = "https://github.com/user/repo-name"
space = "https://huggingface.co/spaces/user/space-name"
```

[←

Multimodal Chatbot Part1](../guides/multimodal-chatbot-part1/) [Getting Started With The Python Client

→](../guides/getting-started-with-the-python-client/)

Status