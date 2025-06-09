# Pdf Component Example

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

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component Example

Step 0: PrerequisitesStep 1: Creating the custom componentStep 2: Frontend - modify javascript dependenciesStep 3: Frontend - Launching the Dev ServerStep 4: Frontend - The basic skeletonStep 5: Frontend - Nicer Upload TextStep 6: PDF Rendering logicStep 7: Handling The File Upload And ClearStep 8: Adding buttons to navigate pagesStep 8.5: The Example viewStep 9: The backendStep 10: Add a demo and publish!Conclusion

Multimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Custom Components
2. Pdf Component Example

[←

Frequently Asked Questions](../guides/frequently-asked-questions/) [Multimodal Chatbot Part1

→](../guides/multimodal-chatbot-part1/)

# Case Study: A Component to Display PDFs

Let's work through an example of building a custom gradio component for displaying PDF files.
This component will come in handy for showcasing document question answering models, which typically work on PDF input.
This is a sneak preview of what our finished component will look like:

## Step 0: Prerequisites

Make sure you have gradio 5.0 or higher installed as well as node 20+.
As of the time of publication, the latest release is 4.1.1.
Also, please read the Five Minute Tour of custom components and the Key Concepts guide before starting.

## Step 1: Creating the custom component

Navigate to a directory of your choosing and run the following command:

```
gradio cc create PDF
```

Tip: You should change the name of the component.
Some of the screenshots assume the component is called `PDF` but the concepts are the same!

This will create a subdirectory called `pdf` in your current working directory.
There are three main subdirectories in `pdf`: `frontend`, `backend`, and `demo`.
If you open `pdf` in your code editor, it will look like this:

**Tip:**
For this demo we are not templating off a current gradio component. But you can see the list of available templates with `gradio cc show` and then pass the template name to the `--template` option, e.g. `gradio cc create  --template `

## Step 2: Frontend - modify javascript dependencies

We're going to use the pdfjs javascript library to display the pdfs in the frontend.
Let's start off by adding it to our frontend project's dependencies, as well as adding a couple of other projects we'll need.

From within the `frontend` directory, run `npm install @gradio/client @gradio/upload @gradio/icons @gradio/button` and `npm install --save-dev pdfjs-dist@3.11.174`.
Also, let's uninstall the `@zerodevx/svelte-json-view` dependency by running `npm uninstall @zerodevx/svelte-json-view`.

The complete `package.json` should look like this:

```
{
  "name": "gradio_pdf",
  "version": "0.2.0",
  "description": "Gradio component for displaying PDFs",
  "type": "module",
  "author": "",
  "license": "ISC",
  "private": false,
  "main_changeset": true,
  "exports": {
    ".": "./Index.svelte",
    "./example": "./Example.svelte",
    "./package.json": "./package.json"
  },
  "devDependencies": {
    "pdfjs-dist": "3.11.174"
  },
  "dependencies": {
    "@gradio/atoms": "0.2.0",
    "@gradio/statustracker": "0.3.0",
    "@gradio/utils": "0.2.0",
    "@gradio/client": "0.7.1",
    "@gradio/upload": "0.3.2",
    "@gradio/icons": "0.2.0",
    "@gradio/button": "0.2.3",
    "pdfjs-dist": "3.11.174"
  }
}
```

**Tip:**
Running `npm install` will install the latest version of the package available. You can install a specific version with `npm install package@`. You can find all of the gradio javascript package documentation here. It is recommended you use the same versions as me as the API can change.

Navigate to `Index.svelte` and delete mentions of `JSONView`

```
import { JsonView } from "@zerodevx/svelte-json-view";
```

```
<JsonView json={value} />
```

## Step 3: Frontend - Launching the Dev Server

Run the `dev` command to launch the development server.
This will open the demo in `demo/app.py` in an environment where changes to the `frontend` and `backend` directories will reflect instantaneously in the launched app.

After launching the dev server, you should see a link printed to your console that says `Frontend Server (Go here): ...` .

You should see the following:

Its not impressive yet but we're ready to start coding!

## Step 4: Frontend - The basic skeleton

We're going to start off by first writing the skeleton of our frontend and then adding the pdf rendering logic.
Add the following imports and expose the following properties to the top of your file in the `<script>` tag.
You may get some warnings from your code editor that some props are not used.
That's ok.

```
    import { tick } from "svelte";
    import type { Gradio } from "@gradio/utils";
    import { Block, BlockLabel } from "@gradio/atoms";
    import { File } from "@gradio/icons";
    import { StatusTracker } from "@gradio/statustracker";
    import type { LoadingStatus } from "@gradio/statustracker";
    import type { FileData } from "@gradio/client";
    import { Upload, ModifyUpload } from "@gradio/upload";

	export let elem_id = "";
	export let elem_classes: string[] = [];
	export let visible = true;
	export let value: FileData | null = null;
	export let container = true;
	export let scale: number | null = null;
	export let root: string;
	export let height: number | null = 500;
	export let label: string;
	export let proxy_url: string;
	export let min_width: number | undefined = undefined;
	export let loading_status: LoadingStatus;
	export let gradio: Gradio<{
		change: never;
		upload: never;
	}>;

    let _value = value;
    let old_value = _value;
```

**Tip:**
The `gradio`` object passed in here contains some metadata about the application as well as some utility methods. One of these utilities is a dispatch method. We want to dispatch change and upload events whenever our PDF is changed or updated. This line provides type hints that these are the only events we will be dispatching.

We want our frontend component to let users upload a PDF document if there isn't one already loaded.
If it is loaded, we want to display it underneath a "clear" button that lets our users upload a new document.
We're going to use the `Upload` and `ModifyUpload` components that come with the `@gradio/upload` package to do this.
Underneath the `</script>` tag, delete all the current code and add the following:

```
<Block {visible} {elem_id} {elem_classes} {container} {scale} {min_width}>
    {#if loading_status}
        <StatusTracker
            autoscroll={gradio.autoscroll}
            i18n={gradio.i18n}
            {...loading_status}
        />
    {/if}
    <BlockLabel
        show_label={label !== null}
        Icon={File}
        float={value === null}
        label={label || "File"}
    />
    {#if _value}
        <ModifyUpload i18n={gradio.i18n} absolute />
    {:else}
        <Upload
            filetype={"application/pdf"}
            file_count="single"
            {root}
        >
            Upload your PDF
        </Upload>
    {/if}
</Block>
```

You should see the following when you navigate to your app after saving your current changes:

## Step 5: Frontend - Nicer Upload Text

The `Upload your PDF` text looks a bit small and barebones.
Lets customize it!

Create a new file called `PdfUploadText.svelte` and copy the following code.
Its creating a new div to display our "upload text" with some custom styling.

**Tip:**
Notice that we're leveraging Gradio core's existing css variables here: `var(--size-60)` and `var(--body-text-color-subdued)`. This allows our component to work nicely in light mode and dark mode, as well as with Gradio's built-in themes.

```
<script lang="ts">
	import { Upload as UploadIcon } from "@gradio/icons";
	export let hovered = false;

</script>

<div class="wrap">
	<span class="icon-wrap" class:hovered><UploadIcon /> </span>
    Drop PDF
    <span class="or">- or -</span>
    Click to Upload
</div>

<style>
	.wrap {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		min-height: var(--size-60);
		color: var(--block-label-text-color);
		line-height: var(--line-md);
		height: 100%;
		padding-top: var(--size-3);
	}

	.or {
		color: var(--body-text-color-subdued);
		display: flex;
	}

	.icon-wrap {
		width: 30px;
		margin-bottom: var(--spacing-lg);
	}

	@media (--screen-md) {
		.wrap {
			font-size: var(--text-lg);
		}
	}

	.hovered {
		color: var(--color-accent);
	}
</style>
```

Now import `PdfUploadText.svelte` in your `<script>` and pass it to the `Upload` component!

```
	import PdfUploadText from "./PdfUploadText.svelte";

...

    <Upload
        filetype={"application/pdf"}
        file_count="single"
        {root}
    >
        <PdfUploadText />
    </Upload>
```

After saving your code, the frontend should now look like this:

## Step 6: PDF Rendering logic

This is the most advanced javascript part.
It took me a while to figure it out!
Do not worry if you have trouble, the important thing is to not be discouraged 💪
Ask for help in the gradio discord if you need and ask for help.

With that out of the way, let's start off by importing `pdfjs` and loading the code of the pdf worker from the mozilla cdn.

```
	import pdfjsLib from "pdfjs-dist";
    ...
    pdfjsLib.GlobalWorkerOptions.workerSrc =  "https://cdn.bootcss.com/pdf.js/3.11.174/pdf.worker.js";
```

Also create the following variables:

```
    let pdfDoc;
    let numPages = 1;
    let currentPage = 1;
    let canvasRef;
```

Now, we will use `pdfjs` to render a given page of the PDF onto an `html` document.
Add the following code to `Index.svelte`:

```
    async function get_doc(value: FileData) {
        const loadingTask = pdfjsLib.getDocument(value.url);
        pdfDoc = await loadingTask.promise;
        numPages = pdfDoc.numPages;
        render_page();
    }

    function render_page() {
    // Render a specific page of the PDF onto the canvas
        pdfDoc.getPage(currentPage).then(page => {
            const ctx  = canvasRef.getContext('2d')
            ctx.clearRect(0, 0, canvasRef.width, canvasRef.height);
            let viewport = page.getViewport({ scale: 1 });
            let scale = height / viewport.height;
            viewport = page.getViewport({ scale: scale });

            const renderContext = {
                canvasContext: ctx,
                viewport,
            };
            canvasRef.width = viewport.width;
            canvasRef.height = viewport.height;
            page.render(renderContext);
        });
    }

    // If the value changes, render the PDF of the currentPage
    $: if(JSON.stringify(old_value) != JSON.stringify(_value)) {
        if (_value){
            get_doc(_value);
        }
        old_value = _value;
        gradio.dispatch("change");
    }
```

**Tip:**
The `$:` syntax in svelte is how you declare statements to be reactive. Whenever any of the inputs of the statement change, svelte will automatically re-run that statement.

Now place the `canvas` underneath the `ModifyUpload` component:

```
<div class="pdf-canvas" style="height: {height}px">
    <canvas bind:this={canvasRef}></canvas>
</div>
```

And add the following styles to the `<style>` tag:

```
<style>
    .pdf-canvas {
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
```

## Step 7: Handling The File Upload And Clear

Now for the fun part - actually rendering the PDF when the file is uploaded!
Add the following functions to the `<script>` tag:

```
    async function handle_clear() {
        _value = null;
        await tick();
        gradio.dispatch("change");
    }

    async function handle_upload({detail}: CustomEvent<FileData>): Promise<void> {
        value = detail;
        await tick();
        gradio.dispatch("change");
        gradio.dispatch("upload");
    }
```

**Tip:**
The `gradio.dispatch` method is actually what is triggering the `change` or `upload` events in the backend. For every event defined in the component's backend, we will explain how to do this in Step 9, there must be at least one `gradio.dispatch("")` call. These are called `gradio` events and they can be listended from the entire Gradio application. You can dispatch a built-in `svelte` event with the `dispatch` function. These events can only be listened to from the component's direct parent. Learn about svelte events from the official documentation.

Now we will run these functions whenever the `Upload` component uploads a file and whenever the `ModifyUpload` component clears the current file. The `<Upload>` component dispatches a `load` event with a payload of type `FileData` corresponding to the uploaded file. The `on:load` syntax tells `Svelte` to automatically run this function in response to the event.

```
    <ModifyUpload i18n={gradio.i18n} on:clear={handle_clear} absolute />

    ...

    <Upload
        on:load={handle_upload}
        filetype={"application/pdf"}
        file_count="single"
        {root}
    >
        <PdfUploadText/>
    </Upload>
```

Congratulations! You have a working pdf uploader!

## Step 8: Adding buttons to navigate pages

If a user uploads a PDF document with multiple pages, they will only be able to see the first one.
Let's add some buttons to help them navigate the page.
We will use the `BaseButton` from `@gradio/button` so that they look like regular Gradio buttons.

Import the `BaseButton` and add the following functions that will render the next and previous page of the PDF.

```
    import { BaseButton } from "@gradio/button";

    ...

    function next_page() {
        if (currentPage >= numPages) {
            return;
        }
        currentPage++;
        render_page();
    }

    function prev_page() {
        if (currentPage == 1) {
            return;
        }
        currentPage--;
        render_page();
    }
```

Now we will add them underneath the canvas in a separate `<div>`

```
    ...

    <ModifyUpload i18n={gradio.i18n} on:clear={handle_clear} absolute />
    <div class="pdf-canvas" style="height: {height}px">
        <canvas bind:this={canvasRef}></canvas>
    </div>
    <div class="button-row">
        <BaseButton on:click={prev_page}>
            ⬅️
        </BaseButton>
        <span class="page-count"> {currentPage} / {numPages} </span>
        <BaseButton on:click={next_page}>
            ➡️
        </BaseButton>
    </div>

    ...

<style>
    .button-row {
        display: flex;
        flex-direction: row;
        width: 100%;
        justify-content: center;
        align-items: center;
    }

    .page-count {
        margin: 0 10px;
        font-family: var(--font-mono);
    }
```

Congratulations! The frontend is almost complete 🎉

## Step 8.5: The Example view

We're going to want users of our component to get a preview of the PDF if its used as an `example` in a `gr.Interface` or `gr.Examples`.

To do so, we're going to add some of the pdf rendering logic in `Index.svelte` to `Example.svelte`.

```
<script lang="ts">
	export let value: string;
	export let type: "gallery" | "table";
	export let selected = false;
	import pdfjsLib from "pdfjs-dist";
	pdfjsLib.GlobalWorkerOptions.workerSrc =  "https://cdn.bootcss.com/pdf.js/3.11.174/pdf.worker.js";

	let pdfDoc;
	let canvasRef;

	async function get_doc(url: string) {
		const loadingTask = pdfjsLib.getDocument(url);
		pdfDoc = await loadingTask.promise;
		renderPage();
		}

	function renderPage() {
		// Render a specific page of the PDF onto the canvas
			pdfDoc.getPage(1).then(page => {
				const ctx  = canvasRef.getContext('2d')
				ctx.clearRect(0, 0, canvasRef.width, canvasRef.height);

				const viewport = page.getViewport({ scale: 0.2 });

				const renderContext = {
					canvasContext: ctx,
					viewport
				};
				canvasRef.width = viewport.width;
				canvasRef.height = viewport.height;
				page.render(renderContext);
			});
		}

	$: get_doc(value);
</script>

<div
	class:table={type === "table"}
	class:gallery={type === "gallery"}
	class:selected
	style="justify-content: center; align-items: center; display: flex; flex-direction: column;"
>
	<canvas bind:this={canvasRef}></canvas>
</div>

<style>
	.gallery {
		padding: var(--size-1) var(--size-2);
	}
</style>
```

**Tip:**
Exercise for the reader - reduce the code duplication between `Index.svelte` and `Example.svelte` 😊

You will not be able to render examples until we make some changes to the backend code in the next step!

## Step 9: The backend

The backend changes needed are smaller.
We're almost done!

What we're going to do is:

* Add `change` and `upload` events to our component.
* Add a `height` property to let users control the height of the PDF.
* Set the `data_model` of our component to be `FileData`. This is so that Gradio can automatically cache and safely serve any files that are processed by our component.
* Modify the `preprocess` method to return a string corresponding to the path of our uploaded PDF.
* Modify the `postprocess` to turn a path to a PDF created in an event handler to a `FileData`.

When all is said an done, your component's backend code should look like this:

```
from __future__ import annotations
from typing import Any, Callable, TYPE_CHECKING

from gradio.components.base import Component
from gradio.data_classes import FileData
from gradio import processing_utils
if TYPE_CHECKING:
    from gradio.components import Timer

class PDF(Component):

    EVENTS = ["change", "upload"]

    data_model = FileData

    def __init__(self, value: Any = None, *,
                 height: int | None = None,
                 label: str | I18nData | None = None,
                 info: str | I18nData | None = None,
                 show_label: bool | None = None,
                 container: bool = True,
                 scale: int | None = None,
                 min_width: int | None = None,
                 interactive: bool | None = None,
                 visible: bool = True,
                 elem_id: str | None = None,
                 elem_classes: list[str] | str | None = None,
                 render: bool = True,
                 load_fn: Callable[..., Any] | None = None,
                 every: Timer | float | None = None):
        super().__init__(value, label=label, info=info,
                         show_label=show_label, container=container,
                         scale=scale, min_width=min_width,
                         interactive=interactive, visible=visible,
                         elem_id=elem_id, elem_classes=elem_classes,
                         render=render, load_fn=load_fn, every=every)
        self.height = height

    def preprocess(self, payload: FileData) -> str:
        return payload.path

    def postprocess(self, value: str | None) -> FileData:
        if not value:
            return None
        return FileData(path=value)

    def example_payload(self):
        return "https://gradio-builds.s3.amazonaws.com/assets/pdf-guide/fw9.pdf"

    def example_value(self):
        return "https://gradio-builds.s3.amazonaws.com/assets/pdf-guide/fw9.pdf"
```

## Step 10: Add a demo and publish](#step-10-add-a-demo-and-publish)

To test our backend code, let's add a more complex demo that performs Document Question and Answering with huggingface transformers.

In our `demo` directory, create a `requirements.txt` file with the following packages

```
torch
transformers
pdf2image
pytesseract
```

**Tip:**
Remember to install these yourself and restart the dev server! You may need to install extra non-python dependencies for `pdf2image`. See here. Feel free to write your own demo if you have trouble.

```
import gradio as gr
from gradio_pdf import PDF
from pdf2image import convert_from_path
from transformers import pipeline
from pathlib import Path

dir_ = Path(__file__).parent

p = pipeline(
    "document-question-answering",
    model="impira/layoutlm-document-qa",
)

def qa(question: str, doc: str) -> str:
    img = convert_from_path(doc)[0]
    output = p(img, question)
    return sorted(output, key=lambda x: x["score"], reverse=True)[0]['answer']

demo = gr.Interface(
    qa,
    [gr.Textbox(label="Question"), PDF(label="Document")],
    gr.Textbox(),
)

demo.launch()
```

See our demo in action below!

[

](https://gradio-builds.s3.amazonaws.com/assets/pdf-guide/PDFDemo.mov)

Finally lets build our component with `gradio cc build` and publish it with the `gradio cc publish` command!
This will guide you through the process of uploading your component to PyPi and HuggingFace Spaces.

**Tip:**
You may need to add the following lines to the `Dockerfile` of your HuggingFace Space.

```
RUN mkdir -p /tmp/cache/
RUN chmod a+rwx -R /tmp/cache/
RUN apt-get update && apt-get install -y poppler-utils tesseract-ocr

ENV TRANSFORMERS_CACHE=/tmp/cache/
```

## Conclusion

In order to use our new component in **any** gradio 4.0 app, simply install it with pip, e.g. `pip install gradio-pdf`. Then you can use it like the built-in `gr.File()` component (except that it will only accept and display PDF files).

Here is a simple demo with the Blocks api:

```
import gradio as gr
from gradio_pdf import PDF

with gr.Blocks() as demo:
    pdf = PDF(label="Upload a PDF", interactive=True)
    name = gr.Textbox()
    pdf.upload(lambda f: f, pdf, name)

demo.launch()
```

I hope you enjoyed this tutorial!
The complete source code for our component is here.
Please don't hesitate to reach out to the gradio community on the HuggingFace Discord if you get stuck.

[←

Frequently Asked Questions](../guides/frequently-asked-questions/) [Multimodal Chatbot Part1

→](../guides/multimodal-chatbot-part1/)

Status