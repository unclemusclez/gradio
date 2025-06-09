# Wrapping Layouts

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

IntroductionExampleImplementationLayoutBase ClassApplication ClassConclusion

1. Other Tutorials
2. Wrapping Layouts

[←

Using Gradio In Other Programming Languages](../guides/using-gradio-in-other-programming-languages/)

# Wrapping Layouts

## Introduction

Gradio features blocks to easily layout applications. To use this feature, you need to stack or nest layout components and create a hierarchy with them. This isn't difficult to implement and maintain for small projects, but after the project gets more complex, this component hierarchy becomes difficult to maintain and reuse.

In this guide, we are going to explore how we can wrap the layout classes to create more maintainable and easy-to-read applications without sacrificing flexibility.

## Example

We are going to follow the implementation from this Huggingface Space example:

## Implementation

The wrapping utility has two important classes. The first one is the `LayoutBase` class and the other one is the `Application` class.

We are going to look at the `render` and `attach_event` functions of them for brevity. You can look at the full implementation from the example code.

So let's start with the `LayoutBase` class.

### LayoutBase Class

1. Render Function

   Let's look at the `render` function in the `LayoutBase` class:

```
# other LayoutBase implementations

def render(self) -> None:
    with self.main_layout:
        for renderable in self.renderables:
            renderable.render()

    self.main_layout.render()
```

This is a little confusing at first but if you consider the default implementation you can understand it easily.
Let's look at an example:

In the default implementation, this is what we're doing:

```
with Row():
    left_textbox = Textbox(value="left_textbox")
    right_textbox = Textbox(value="right_textbox")
```

Now, pay attention to the Textbox variables. These variables' `render` parameter is true by default. So as we use the `with` syntax and create these variables, they are calling the `render` function under the `with` syntax.

We know the render function is called in the constructor with the implementation from the `gradio.blocks.Block` class:

```
class Block:
    # constructor parameters are omitted for brevity
    def __init__(self, ...):
        # other assign functions

        if render:
            self.render()
```

So our implementation looks like this:

```
# self.main_layout -> Row()
with self.main_layout:
    left_textbox.render()
    right_textbox.render()
```

What this means is by calling the components' render functions under the `with` syntax, we are actually simulating the default implementation.

So now let's consider two nested `with`s to see how the outer one works. For this, let's expand our example with the `Tab` component:

```
with Tab():
    with Row():
        first_textbox = Textbox(value="first_textbox")
        second_textbox = Textbox(value="second_textbox")
```

Pay attention to the Row and Tab components this time. We have created the Textbox variables above and added them to Row with the `with` syntax. Now we need to add the Row component to the Tab component. You can see that the Row component is created with default parameters, so its render parameter is true, that's why the render function is going to be executed under the Tab component's `with` syntax.

To mimic this implementation, we need to call the `render` function of the `main_layout` variable after the `with` syntax of the `main_layout` variable.

So the implementation looks like this:

```
with tab_main_layout:
    with row_main_layout:
        first_textbox.render()
        second_textbox.render()

    row_main_layout.render()

tab_main_layout.render()
```

The default implementation and our implementation are the same, but we are using the render function ourselves. So it requires a little work.

Now, let's take a look at the `attach_event` function.

2. Attach Event Function

   The function is left as not implemented because it is specific to the class, so each class has to implement its `attach_event` function.

```
    # other LayoutBase implementations

    def attach_event(self, block_dict: Dict[str, Block]) -> None:
        raise NotImplementedError
```

Check out the `block_dict` variable in the `Application` class's `attach_event` function.

### Application Class

1. Render Function

```
    # other Application implementations

    def _render(self):
        with self.app:
            for child in self.children:
                child.render()

        self.app.render()
```

From the explanation of the `LayoutBase` class's `render` function, we can understand the `child.render` part.

So let's look at the bottom part, why are we calling the `app` variable's `render` function? It's important to call this function because if we look at the implementation in the `gradio.blocks.Blocks` class, we can see that it is adding the components and event functions into the root component. To put it another way, it is creating and structuring the gradio application.

2. Attach Event Function

   Let's see how we can attach events to components:

```
    # other Application implementations

    def _attach_event(self):
        block_dict: Dict[str, Block] = {}

        for child in self.children:
            block_dict.update(child.global_children_dict)

        with self.app:
            for child in self.children:
                try:
                    child.attach_event(block_dict=block_dict)
                except NotImplementedError:
                    print(f"{child.name}'s attach_event is not implemented")
```

You can see why the `global_children_list` is used in the `LayoutBase` class from the example code. With this, all the components in the application are gathered into one dictionary, so the component can access all the components with their names.

The `with` syntax is used here again to attach events to components. If we look at the `__exit__` function in the `gradio.blocks.Blocks` class, we can see that it is calling the `attach_load_events` function which is used for setting event triggers to components. So we have to use the `with` syntax to trigger the `__exit__` function.

Of course, we can call `attach_load_events` without using the `with` syntax, but the function needs a `Context.root_block`, and it is set in the `__enter__` function. So we used the `with` syntax here rather than calling the function ourselves.

## Conclusion

In this guide, we saw

* How we can wrap the layouts
* How components are rendered
* How we can structure our application with wrapped layout classes

Because the classes used in this guide are used for demonstration purposes, they may still not be totally optimized or modular. But that would make the guide much longer!

I hope this guide helps you gain another view of the layout classes and gives you an idea about how you can use them for your needs. See the full implementation of our example here.

[←

Using Gradio In Other Programming Languages](../guides/using-gradio-in-other-programming-languages/)

Status