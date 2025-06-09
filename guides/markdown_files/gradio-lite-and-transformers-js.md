# Gradio Lite And Transformers Js

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

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers Js

Libraries UsedGradio-LiteTransformers.js and Transformers.js.pySample CodeCustomizing the Model or PipelineCustomizing the UIConclusion

Fastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Gradio Clients And Lite
2. Gradio Lite And Transformers Js

[←

Gradio Lite](../guides/gradio-lite/) [Fastapi App With The Gradio Client

→](../guides/fastapi-app-with-the-gradio-client/)

# Building Serverless Machine Learning Apps with Gradio-Lite and Transformers.js

Gradio and Transformers are a powerful combination for building machine learning apps with a web interface. Both libraries have serverless versions that can run entirely in the browser: Gradio-Lite and Transformers.js.
In this document, we will introduce how to create a serverless machine learning application using Gradio-Lite and Transformers.js.
You will just write Python code within a static HTML file and host it without setting up a server-side Python runtime.

## Libraries Used

### Gradio-Lite

Gradio-Lite is the serverless version of Gradio, allowing you to build serverless web UI applications by embedding Python code within HTML. For a detailed introduction to Gradio-Lite itself, please read this Guide.

### Transformers.js and Transformers.js.py

Transformers.js is the JavaScript version of the Transformers library that allows you to run machine learning models entirely in the browser.
Since Transformers.js is a JavaScript library, it cannot be directly used from the Python code of Gradio-Lite applications. To address this, we use a wrapper library called Transformers.js.py.
The name Transformers.js.py may sound unusual, but it represents the necessary technology stack for using Transformers.js from Python code within a browser environment. The regular Transformers library is not compatible with browser environments.

## Sample Code

Here's an example of how to use Gradio-Lite and Transformers.js together.
Please create an HTML file and paste the following code:

```
<html>
	<head>
		<script type="module" crossorigin src="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.js"></script>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.css" />
	</head>
	<body>
		<gradio-lite>
import gradio as gr
from transformers_js_py import pipeline

pipe = await pipeline('sentiment-analysis')

demo = gr.Interface.from_pipeline(pipe)

demo.launch()

			<gradio-requirements>
transformers-js-py
			</gradio-requirements>
		</gradio-lite>
	</body>
</html>
```

Here is a running example of the code above (after the app has loaded, you could disconnect your Internet connection and the app will still work since its running entirely in your browser):

import gradio as gr
from transformers\_js\_py import pipeline
pipe = await pipeline('sentiment-analysis')
demo = gr.Interface.from\_pipeline(pipe)
demo.launch()
transformers-js-py

And you you can open your HTML file in a browser to see the Gradio app running!

The Python code inside the `<gradio-lite>` tag is the Gradio application code. For more details on this part, please refer to this article.
The `<gradio-requirements>` tag is used to specify packages to be installed in addition to Gradio-Lite and its dependencies. In this case, we are using Transformers.js.py (`transformers-js-py`), so it is specified here.

Let's break down the code:

`pipe = await pipeline('sentiment-analysis')` creates a Transformers.js pipeline.
In this example, we create a sentiment analysis pipeline.
For more information on the available pipeline types and usage, please refer to the Transformers.js documentation.

`demo = gr.Interface.from_pipeline(pipe)` creates a Gradio app instance. By passing the Transformers.js.py pipeline to `gr.Interface.from_pipeline()`, we can create an interface that utilizes that pipeline with predefined input and output components.

Finally, `demo.launch()` launches the created app.

## Customizing the Model or Pipeline

You can modify the line `pipe = await pipeline('sentiment-analysis')` in the sample above to try different models or tasks.

For example, if you change it to `pipe = await pipeline('sentiment-analysis', 'Xenova/bert-base-multilingual-uncased-sentiment')`, you can test the same sentiment analysis task but with a different model. The second argument of the `pipeline` function specifies the model name.
If it's not specified like in the first example, the default model is used. For more details on these specs, refer to the Transformers.js documentation.

import gradio as gr
from transformers\_js\_py import pipeline
pipe = await pipeline('sentiment-analysis', 'Xenova/bert-base-multilingual-uncased-sentiment')
demo = gr.Interface.from\_pipeline(pipe)
demo.launch()
transformers-js-py

As another example, changing it to `pipe = await pipeline('image-classification')` creates a pipeline for image classification instead of sentiment analysis.
In this case, the interface created with `demo = gr.Interface.from_pipeline(pipe)` will have a UI for uploading an image and displaying the classification result. The `gr.Interface.from_pipeline` function automatically creates an appropriate UI based on the type of pipeline.

import gradio as gr
from transformers\_js\_py import pipeline
pipe = await pipeline('image-classification')
demo = gr.Interface.from\_pipeline(pipe)
demo.launch()
transformers-js-py

**Note**: If you use an audio pipeline, such as `automatic-speech-recognition`, you will need to put `transformers-js-py[audio]` in your `<gradio-requirements>` as there are additional requirements needed to process audio files.

## Customizing the UI

Instead of using `gr.Interface.from_pipeline()`, you can define the user interface using Gradio's regular API.
Here's an example where the Python code inside the `<gradio-lite>` tag has been modified from the previous sample:

```
<html>
	<head>
		<script type="module" crossorigin src="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.js"></script>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.css" />
	</head>
	<body>
		<gradio-lite>
import gradio as gr
from transformers_js_py import pipeline

pipe = await pipeline('sentiment-analysis')

async def fn(text):
	result = await pipe(text)
	return result

demo = gr.Interface(
	fn=fn,
	inputs=gr.Textbox(),
	outputs=gr.JSON(),
)

demo.launch()

			<gradio-requirements>
transformers-js-py
			</gradio-requirements>
		</gradio-lite>
	</body>
</html>
```

In this example, we modified the code to construct the Gradio user interface manually so that we could output the result as JSON.

import gradio as gr
from transformers\_js\_py import pipeline
pipe = await pipeline('sentiment-analysis')
async def fn(text):
result = await pipe(text)
return result
demo = gr.Interface(
fn=fn,
inputs=gr.Textbox(),
outputs=gr.JSON(),
)
demo.launch()
transformers-js-py

## Conclusion

By combining Gradio-Lite and Transformers.js (and Transformers.js.py), you can create serverless machine learning applications that run entirely in the browser.

Gradio-Lite provides a convenient method to create an interface for a given Transformers.js pipeline, `gr.Interface.from_pipeline()`.
This method automatically constructs the interface based on the pipeline's task type.

Alternatively, you can define the interface manually using Gradio's regular API, as shown in the second example.

By using these libraries, you can build and deploy machine learning applications without the need for server-side Python setup or external dependencies.

[←

Gradio Lite](../guides/gradio-lite/) [Fastapi App With The Gradio Client

→](../guides/fastapi-app-with-the-gradio-client/)

Status