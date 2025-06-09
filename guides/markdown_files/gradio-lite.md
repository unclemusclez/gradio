# Gradio Lite

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

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio Lite

What is @gradio/lite?Getting Started1. Import JS and CSS2. Create the &lt;gradio-lite&gt; tags3. Write your Gradio app inside of the tagsMore Examples: Adding Additional Files and RequirementsMultiple FilesAdditional RequirementsSharedWorker modeCode and Demo PlaygroundBenefits of Using @gradio/lite1. Serverless Deployment2. Low Latency3. Privacy and SecurityLimitationsTry it out!

Gradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Gradio Clients And Lite
2. Gradio Lite

[←

Gradio And Llm Agents](../guides/gradio-and-llm-agents/) [Gradio Lite And Transformers Js

→](../guides/gradio-lite-and-transformers-js/)

# Gradio-Lite: Serverless Gradio Running Entirely in Your Browser

Gradio is a popular Python library for creating interactive machine learning apps. Traditionally, Gradio applications have relied on server-side infrastructure to run, which can be a hurdle for developers who need to host their applications.

Enter Gradio-lite (`@gradio/lite`): a library that leverages Pyodide to bring Gradio directly to your browser. In this blog post, we'll explore what `@gradio/lite` is, go over example code, and discuss the benefits it offers for running Gradio applications.

## What is `@gradio/lite`?

`@gradio/lite` is a JavaScript library that enables you to run Gradio applications directly within your web browser. It achieves this by utilizing Pyodide, a Python runtime for WebAssembly, which allows Python code to be executed in the browser environment. With `@gradio/lite`, you can **write regular Python code for your Gradio applications**, and they will **run seamlessly in the browser** without the need for server-side infrastructure.

## Getting Started

Let's build a "Hello World" Gradio app in `@gradio/lite`

### 1. Import JS and CSS

Start by creating a new HTML file, if you don't have one already. Importing the JavaScript and CSS corresponding to the `@gradio/lite` package by using the following code:

```
<html>
	<head>
		<script type="module" crossorigin src="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.js"></script>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.css" />
	</head>
</html>
```

Note that you should generally use the latest version of `@gradio/lite` that is available. You can see the versions available here.

### 2. Create the `<gradio-lite>` tags

Somewhere in the body of your HTML page (wherever you'd like the Gradio app to be rendered), create opening and closing `<gradio-lite>` tags.

```
<html>
	<head>
		<script type="module" crossorigin src="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.js"></script>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.css" />
	</head>
	<body>
		<gradio-lite>
		</gradio-lite>
	</body>
</html>
```

Note: you can add the `theme` attribute to the `<gradio-lite>` tag to force the theme to be dark or light (by default, it respects the system theme). E.g.

```
<gradio-lite theme="dark">
...
</gradio-lite>
```

### 3. Write your Gradio app inside of the tags

Now, write your Gradio app as you would normally, in Python! Keep in mind that since this is Python, whitespace and indentations matter.

```
<html>
	<head>
		<script type="module" crossorigin src="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.js"></script>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.css" />
	</head>
	<body>
		<gradio-lite>
		import gradio as gr

		def greet(name):
			return "Hello, " + name + "!"

		gr.Interface(greet, "textbox", "textbox").launch()
		</gradio-lite>
	</body>
</html>
```

And that's it! You should now be able to open your HTML page in the browser and see the Gradio app rendered! Note that it may take a little while for the Gradio app to load initially since Pyodide can take a while to install in your browser.

**Note on debugging**: to see any errors in your Gradio-lite application, open the inspector in your web browser. All errors (including Python errors) will be printed there.

## More Examples: Adding Additional Files and Requirements

What if you want to create a Gradio app that spans multiple files? Or that has custom Python requirements? Both are possible with `@gradio/lite`!

### Multiple Files

Adding multiple files within a `@gradio/lite` app is very straightforward: use the `<gradio-file>` tag. You can have as many `<gradio-file>` tags as you want, but each one needs to have a `name` attribute and the entry point to your Gradio app should have the `entrypoint` attribute.

Here's an example:

```
<gradio-lite>

<gradio-file name="app.py" entrypoint>
import gradio as gr
from utils import add

demo = gr.Interface(fn=add, inputs=["number", "number"], outputs="number")

demo.launch()
</gradio-file>

<gradio-file name="utils.py" >
def add(a, b):
	return a + b
</gradio-file>

</gradio-lite>

```

### Additional Requirements

If your Gradio app has additional requirements, it is usually possible to install them in the browser using micropip. We've created a wrapper to make this paticularly convenient: simply list your requirements in the same syntax as a `requirements.txt` and enclose them with `<gradio-requirements>` tags.

Here, we install `transformers_js_py` to run a text classification model directly in the browser!

```
<gradio-lite>

<gradio-requirements>
transformers_js_py
</gradio-requirements>

<gradio-file name="app.py" entrypoint>
from transformers_js import import_transformers_js
import gradio as gr

transformers = await import_transformers_js()
pipeline = transformers.pipeline
pipe = await pipeline('sentiment-analysis')

async def classify(text):
	return await pipe(text)

demo = gr.Interface(classify, "textbox", "json")
demo.launch()
</gradio-file>

</gradio-lite>

```

**Try it out**: You can see this example running in this Hugging Face Static Space, which lets you host static (serverless) web applications for free. Visit the page and you'll be able to run a machine learning model without internet access!

### SharedWorker mode

By default, Gradio-Lite executes Python code in a Web Worker with Pyodide runtime, and each Gradio-Lite app has its own worker.
It has some benefits such as environment isolation.

However, when there are many Gradio-Lite apps in the same page, it may cause performance issues such as high memory usage because each app has its own worker and Pyodide runtime.
In such cases, you can use the **SharedWorker mode** to share a single Pyodide runtime in a SharedWorker among multiple Gradio-Lite apps. To enable the SharedWorker mode, set the `shared-worker` attribute to the `<gradio-lite>` tag.

```
<!-- These two Gradio-Lite apps share a single worker -->

<gradio-lite shared-worker>
import gradio as gr
# ...
</gradio-lite>

<gradio-lite shared-worker>
import gradio as gr
# ...
</gradio-lite>
```

When using the SharedWorker mode, you should be aware of the following points:

* The apps share the same Python environment, which means that they can access the same modules and objects. If, for example, one app makes changes to some modules, the changes will be visible to other apps.
* The file system is shared among the apps, while each app's files are mounted in each home directory, so each app can access the files of other apps.

### Code and Demo Playground

If you'd like to see the code side-by-side with the demo just pass in the `playground` attribute to the gradio-lite element. This will create an interactive playground that allows you to change the code and update the demo! If you're using playground, you can also set layout to either 'vertical' or 'horizontal' which will determine if the code editor and preview are side-by-side or on top of each other (by default it's reposnsive with the width of the page).

```
<gradio-lite playground layout="horizontal">
import gradio as gr

gr.Interface(fn=lambda x: x,
			inputs=gr.Textbox(),
			outputs=gr.Textbox()
		).launch()
</gradio-lite>
```

## Benefits of Using `@gradio/lite`

### 1. Serverless Deployment

The primary advantage of @gradio/lite is that it eliminates the need for server infrastructure. This simplifies deployment, reduces server-related costs, and makes it easier to share your Gradio applications with others.

### 2. Low Latency

By running in the browser, @gradio/lite offers low-latency interactions for users. There's no need for data to travel to and from a server, resulting in faster responses and a smoother user experience.

### 3. Privacy and Security

Since all processing occurs within the user's browser, `@gradio/lite` enhances privacy and security. User data remains on their device, providing peace of mind regarding data handling.

### Limitations

* Currently, the biggest limitation in using `@gradio/lite` is that your Gradio apps will generally take more time (usually 5-15 seconds) to load initially in the browser. This is because the browser needs to load the Pyodide runtime before it can render Python code.
* Not every Python package is supported by Pyodide. While `gradio` and many other popular packages (including `numpy`, `scikit-learn`, and `transformers-js`) can be installed in Pyodide, if your app has many dependencies, its worth checking whether whether the dependencies are included in Pyodide, or can be installed with `micropip`.

## Try it out](#try-it-out)

You can immediately try out `@gradio/lite` by copying and pasting this code in a local `index.html` file and opening it with your browser:

```
<html>
	<head>
		<script type="module" crossorigin src="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.js"></script>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@gradio/lite/dist/lite.css" />
	</head>
	<body>
		<gradio-lite>
		import gradio as gr

		def greet(name):
			return "Hello, " + name + "!"

		gr.Interface(greet, "textbox", "textbox").launch()
		</gradio-lite>
	</body>
</html>
```

We've also created a playground on the Gradio website that allows you to interactively edit code and see the results immediately!

Playground: <https://www.gradio.app/playground>

[←

Gradio And Llm Agents](../guides/gradio-and-llm-agents/) [Gradio Lite And Transformers Js

→](../guides/gradio-lite-and-transformers-js/)

Status