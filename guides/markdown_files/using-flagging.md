# Using Flagging

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

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing Flagging

IntroductionThe Flag button in gradio.InterfaceWhat happens to flagged data?Flagging with BlocksPrivacyThat's all! Happy building :)

Using Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Other Tutorials
2. Using Flagging

[←

Understanding Gradio Share Links](../guides/understanding-gradio-share-links/) [Using Gradio For Tabular Workflows

→](../guides/using-gradio-for-tabular-workflows/)

Related Spaces:

gradio/calculator-flagging-crowdsourced

gradio/calculator-flagging-options

gradio/calculator-flagging-basic

# Using Flagging

## Introduction

When you demo a machine learning model, you might want to collect data from users who try the model, particularly data points in which the model is not behaving as expected. Capturing these "hard" data points is valuable because it allows you to improve your machine learning model and make it more reliable and robust.

Gradio simplifies the collection of this data by including a **Flag** button with every `Interface`. This allows a user or tester to easily send data back to the machine where the demo is running. In this Guide, we discuss more about how to use the flagging feature, both with `gradio.Interface` as well as with `gradio.Blocks`.

## The **Flag** button in `gradio.Interface`

Flagging with Gradio's `Interface` is especially easy. By default, underneath the output components, there is a button marked **Flag**. When a user testing your model sees input with interesting output, they can click the flag button to send the input and output data back to the machine where the demo is running. The sample is saved to a CSV log file (by default). If the demo involves images, audio, video, or other types of files, these are saved separately in a parallel directory and the paths to these files are saved in the CSV file.

There are four parameters in `gradio.Interface` that control how flagging works. We will go over them in greater detail.

* `flagging_mode`: this parameter can be set to either `"manual"` (default), `"auto"`, or `"never"`.
  + `manual`: users will see a button to flag, and samples are only flagged when the button is clicked.
  + `auto`: users will not see a button to flag, but every sample will be flagged automatically.
  + `never`: users will not see a button to flag, and no sample will be flagged.
* `flagging_options`: this parameter can be either `None` (default) or a list of strings.
  + If `None`, then the user simply clicks on the **Flag** button and no additional options are shown.
  + If a list of strings are provided, then the user sees several buttons, corresponding to each of the strings that are provided. For example, if the value of this parameter is `["Incorrect", "Ambiguous"]`, then buttons labeled **Flag as Incorrect** and **Flag as Ambiguous** appear. This only applies if `flagging_mode` is `"manual"`.
  + The chosen option is then logged along with the input and output.
* `flagging_dir`: this parameter takes a string.
  + It represents what to name the directory where flagged data is stored.
* `flagging_callback`: this parameter takes an instance of a subclass of the `FlaggingCallback` class
  + Using this parameter allows you to write custom code that gets run when the flag button is clicked
  + By default, this is set to an instance of `gr.JSONLogger`

## What happens to flagged data?

Within the directory provided by the `flagging_dir` argument, a JSON file will log the flagged data.

Here's an example: The code below creates the calculator interface embedded below it:

```
import gradio as gr

def calculator(num1, operation, num2):
    if operation == "add":
        return num1 + num2
    elif operation == "subtract":
        return num1 - num2
    elif operation == "multiply":
        return num1 * num2
    elif operation == "divide":
        return num1 / num2

iface = gr.Interface(
    calculator,
    ["number", gr.Radio(["add", "subtract", "multiply", "divide"]), "number"],
    "number",
    flagging_mode="manual"
)

iface.launch()
```

When you click the flag button above, the directory where the interface was launched will include a new flagged subfolder, with a csv file inside it. This csv file includes all the data that was flagged.

```
+-- flagged/
|   +-- logs.csv
```

*flagged/logs.csv*

```
num1,operation,num2,Output,timestamp
5,add,7,12,2022-01-31 11:40:51.093412
6,subtract,1.5,4.5,2022-01-31 03:25:32.023542
```

If the interface involves file data, such as for Image and Audio components, folders will be created to store those flagged data as well. For example an `image` input to `image` output interface will create the following structure.

```
+-- flagged/
|   +-- logs.csv
|   +-- image/
|   |   +-- 0.png
|   |   +-- 1.png
|   +-- Output/
|   |   +-- 0.png
|   |   +-- 1.png
```

*flagged/logs.csv*

```
im,Output timestamp
im/0.png,Output/0.png,2022-02-04 19:49:58.026963
im/1.png,Output/1.png,2022-02-02 10:40:51.093412
```

If you wish for the user to provide a reason for flagging, you can pass a list of strings to the `flagging_options` argument of Interface. Users will have to select one of these choices when flagging, and the option will be saved as an additional column to the CSV.

If we go back to the calculator example, the following code will create the interface embedded below it.

```
iface = gr.Interface(
    calculator,
    ["number", gr.Radio(["add", "subtract", "multiply", "divide"]), "number"],
    "number",
    flagging_mode="manual",
    flagging_options=["wrong sign", "off by one", "other"]
)

iface.launch()
```

When users click the flag button, the csv file will now include a column indicating the selected option.

*flagged/logs.csv*

```
num1,operation,num2,Output,flag,timestamp
5,add,7,-12,wrong sign,2022-02-04 11:40:51.093412
6,subtract,1.5,3.5,off by one,2022-02-04 11:42:32.062512
```

## Flagging with Blocks

What about if you are using `gradio.Blocks`? On one hand, you have even more flexibility
with Blocks -- you can write whatever Python code you want to run when a button is clicked,
and assign that using the built-in events in Blocks.

At the same time, you might want to use an existing `FlaggingCallback` to avoid writing extra code.
This requires two steps:

1. You have to run your callback's `.setup()` somewhere in the code prior to the
   first time you flag data
2. When the flagging button is clicked, then you trigger the callback's `.flag()` method,
   making sure to collect the arguments correctly and disabling the typical preprocessing.

Here is an example with an image sepia filter Blocks demo that lets you flag
data using the default `CSVLogger`:

```
import numpy as np
import gradio as gr

def sepia(input_img, strength):
    sepia_filter = strength * np.array(
        [[0.393, 0.769, 0.189], [0.349, 0.686, 0.168], [0.272, 0.534, 0.131]]
    ) + (1-strength) * np.identity(3)
    sepia_img = input_img.dot(sepia_filter.T)
    sepia_img /= sepia_img.max()
    return sepia_img

callback = gr.CSVLogger()

with gr.Blocks() as demo:
    with gr.Row():
        with gr.Column():
            img_input = gr.Image()
            strength = gr.Slider(0, 1, 0.5)
        img_output = gr.Image()
    with gr.Row():
        btn = gr.Button("Flag")

    # This needs to be called at some point prior to the first call to callback.flag()
    callback.setup([img_input, strength, img_output], "flagged_data_points")

    img_input.change(sepia, [img_input, strength], img_output)
    strength.change(sepia, [img_input, strength], img_output)

    # We can choose which components to flag -- in this case, we'll flag all of them
    btn.click(lambda *args: callback.flag(list(args)), [img_input, strength, img_output], None, preprocess=False)

demo.launch()

```

## Privacy

Important Note: please make sure your users understand when the data they submit is being saved, and what you plan on doing with it. This is especially important when you use `flagging_mode=auto` (when all of the data submitted through the demo is being flagged)

### That's all! Happy building :)

[←

Understanding Gradio Share Links](../guides/understanding-gradio-share-links/) [Using Gradio For Tabular Workflows

→](../guides/using-gradio-for-tabular-workflows/)

Status