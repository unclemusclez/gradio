# Dynamic Apps With Render Decorator

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

Blocks And Event ListenersControlling LayoutState In BlocksDynamic Apps With Render Decorator

Dynamic Number of ComponentsDynamic Event ListenersCloser Look at keys= parameterPutting it Together

More Blocks FeaturesCustom CSS And JSUsing Blocks Like Functions

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

1. Building With Blocks
2. Dynamic Apps With Render Decorator

[←

State In Blocks](../guides/state-in-blocks/) [More Blocks Features

→](../guides/more-blocks-features/)

# Dynamic Apps with the Render Decorator

The components and event listeners you define in a Blocks so far have been fixed - once the demo was launched, new components and listeners could not be added, and existing one could not be removed.

The `@gr.render` decorator introduces the ability to dynamically change this. Let's take a look.

## Dynamic Number of Components

In the example below, we will create a variable number of Textboxes. When the user edits the input Textbox, we create a Textbox for each letter in the input. Try it out below:

```
import gradio as gr

with gr.Blocks() as demo:
    input_text = gr.Textbox(label="input")

    @gr.render(inputs=input_text)
    def show_split(text):
        if len(text) == 0:
            gr.Markdown("## No Input Provided")
        else:
            for letter in text:
                gr.Textbox(letter)

demo.launch()

```

See how we can now create a variable number of Textboxes using our custom logic - in this case, a simple `for` loop. The `@gr.render` decorator enables this with the following steps:

1. Create a function and attach the @gr.render decorator to it.
2. Add the input components to the `inputs=` argument of @gr.render, and create a corresponding argument in your function for each component. This function will automatically re-run on any change to a component.
3. Add all components inside the function that you want to render based on the inputs.

Now whenever the inputs change, the function re-runs, and replaces the components created from the previous function run with the latest run. Pretty straightforward! Let's add a little more complexity to this app:

```
import gradio as gr

with gr.Blocks() as demo:
    input_text = gr.Textbox(label="input")
    mode = gr.Radio(["textbox", "button"], value="textbox")

    @gr.render(inputs=[input_text, mode], triggers=[input_text.submit])
    def show_split(text, mode):
        if len(text) == 0:
            gr.Markdown("## No Input Provided")
        else:
            for letter in text:
                if mode == "textbox":
                    gr.Textbox(letter)
                else:
                    gr.Button(letter)

demo.launch()

```

By default, `@gr.render` re-runs are triggered by the `.load` listener to the app and the `.change` listener to any input component provided. We can override this by explicitly setting the triggers in the decorator, as we have in this app to only trigger on `input_text.submit` instead.
If you are setting custom triggers, and you also want an automatic render at the start of the app, make sure to add `demo.load` to your list of triggers.

## Dynamic Event Listeners

If you're creating components, you probably want to attach event listeners to them as well. Let's take a look at an example that takes in a variable number of Textbox as input, and merges all the text into a single box.

```
import gradio as gr

with gr.Blocks() as demo:
    text_count = gr.State(1)
    add_btn = gr.Button("Add Box")
    add_btn.click(lambda x: x + 1, text_count, text_count)

    @gr.render(inputs=text_count)
    def render_count(count):
        boxes = []
        for i in range(count):
            box = gr.Textbox(key=i, label=f"Box {i}")
            boxes.append(box)

        def merge(*args):
            return " ".join(args)

        merge_btn.click(merge, boxes, output)

    merge_btn = gr.Button("Merge")
    output = gr.Textbox(label="Merged Output")

demo.launch()

```

Let's take a look at what's happening here:

1. The state variable `text_count` is keeping track of the number of Textboxes to create. By clicking on the Add button, we increase `text_count` which triggers the render decorator.
2. Note that in every single Textbox we create in the render function, we explicitly set a `key=` argument. This key allows us to preserve the value of this Component between re-renders. If you type in a value in a textbox, and then click the Add button, all the Textboxes re-render, but their values aren't cleared because the `key=` maintains the the value of a Component across a render.
3. We've stored the Textboxes created in a list, and provide this list as input to the merge button event listener. Note that **all event listeners that use Components created inside a render function must also be defined inside that render function**. The event listener can still reference Components outside the render function, as we do here by referencing `merge_btn` and `output` which are both defined outside the render function.

Just as with Components, whenever a function re-renders, the event listeners created from the previous render are cleared and the new event listeners from the latest run are attached.

This allows us to create highly customizable and complex interactions!

## Closer Look at `keys=` parameter

The `key=` argument is used to let Gradio know that the same component is being generated when your render function re-runs. This does two things:

1. The same element in the browser is re-used from the previous render for this Component. This gives browser performance gains - as there's no need to destroy and rebuild a component on a render - and preserves any browser attributes that the Component may have had. If your Component is nested within layout items like `gr.Row`, make sure they are keyed as well because the keys of the parents must also match.
2. Properties that may be changed by the user or by other event listeners are preserved. By default, only the "value" of Component is preserved, but you can specify any list of properties to preserve using the `preserved_by_key=` kwarg.

See the example below:

```
import gradio as gr
import random

with gr.Blocks() as demo:
    number_of_boxes = gr.Slider(1, 5, step=1, value=3, label="Number of Boxes")

    @gr.render(inputs=[number_of_boxes])
    def create_boxes(number_of_boxes):
        for i in range(number_of_boxes):
            with gr.Row(key=f'row-{i}'):
                number_box = gr.Textbox(
                    label=f"Default Label",
                    info="Default Info",
                    key=f"box-{i}",
                    preserved_by_key=["label", "value"],
                    interactive=True
                )
                change_label_btn = gr.Button("Change Label", key=f"btn-{i}")

                change_label_btn.click(
                    lambda: gr.Textbox(
                        label=random.choice("ABCDE"),
                        info=random.choice("ABCDE")),
                        outputs=number_box
                )

demo.launch()
```

You'll see in this example, when you change the `number_of_boxes` slider, there's a new re-render to update the number of box rows. If you click the "Change Label" buttons, they change the `label` and `info` properties of the corresponding textbox. You can also enter text in any textbox to change its value. If you change number of boxes after this, the re-renders "reset" the `info`, but the `label` and any entered `value` is still preserved.

Note you can also key any event listener, e.g. `button.click(key=...)` if the same listener is being recreated with the same inputs and outputs across renders. This gives performance benefits, and also prevents errors from occuring if an event was triggered in a previous render, then a re-render occurs, and then the previous event finishes processing. By keying your listener, Gradio knows where to send the data properly.

## Putting it Together

Let's look at two examples that use all the features above. First, try out the to-do list app below:

```
import gradio as gr

with gr.Blocks() as demo:

    tasks = gr.State([])
    new_task = gr.Textbox(label="Task Name", autofocus=True)

    def add_task(tasks, new_task_name):
        return tasks + [{"name": new_task_name, "complete": False}], ""

    new_task.submit(add_task, [tasks, new_task], [tasks, new_task])

    @gr.render(inputs=tasks)
    def render_todos(task_list):
        complete = [task for task in task_list if task["complete"]]
        incomplete = [task for task in task_list if not task["complete"]]
        gr.Markdown(f"### Incomplete Tasks ({len(incomplete)})")
        for task in incomplete:
            with gr.Row():
                gr.Textbox(task['name'], show_label=False, container=False)
                done_btn = gr.Button("Done", scale=0)
                def mark_done(task=task):
                    task["complete"] = True
                    return task_list
                done_btn.click(mark_done, None, [tasks])

                delete_btn = gr.Button("Delete", scale=0, variant="stop")
                def delete(task=task):
                    task_list.remove(task)
                    return task_list
                delete_btn.click(delete, None, [tasks])

        gr.Markdown(f"### Complete Tasks ({len(complete)})")
        for task in complete:
            gr.Textbox(task['name'], show_label=False, container=False)

demo.launch()

```

Note that almost the entire app is inside a single `gr.render` that reacts to the tasks `gr.State` variable. This variable is a nested list, which presents some complexity. If you design a `gr.render` to react to a list or dict structure, ensure you do the following:

1. Any event listener that modifies a state variable in a manner that should trigger a re-render must set the state variable as an output. This lets Gradio know to check if the variable has changed behind the scenes.
2. In a `gr.render`, if a variable in a loop is used inside an event listener function, that variable should be "frozen" via setting it to itself as a default argument in the function header. See how we have `task=task` in both `mark_done` and `delete`. This freezes the variable to its "loop-time" value.

Let's take a look at one last example that uses everything we learned. Below is an audio mixer. Provide multiple audio tracks and mix them together.

```
import gradio as gr

with gr.Blocks() as demo:
    track_count = gr.State(1)
    add_track_btn = gr.Button("Add Track")

    add_track_btn.click(lambda count: count + 1, track_count, track_count)

    @gr.render(inputs=track_count)
    def render_tracks(count):
        audios = []
        volumes = []
        with gr.Row():
            for i in range(count):
                with gr.Column(variant="panel", min_width=200):
                    gr.Textbox(placeholder="Track Name", key=f"name-{i}", show_label=False)
                    track_audio = gr.Audio(label=f"Track {i}", key=f"track-{i}")
                    track_volume = gr.Slider(0, 100, value=100, label="Volume", key=f"volume-{i}")
                    audios.append(track_audio)
                    volumes.append(track_volume)

            def merge(data):
                sr, output = None, None
                for audio, volume in zip(audios, volumes):
                    sr, audio_val = data[audio]
                    volume_val = data[volume]
                    final_track = audio_val * (volume_val / 100)
                    if output is None:
                        output = final_track
                    else:
                        min_shape = tuple(min(s1, s2) for s1, s2 in zip(output.shape, final_track.shape))
                        trimmed_output = output[:min_shape[0], ...][:, :min_shape[1], ...] if output.ndim > 1 else output[:min_shape[0]]
                        trimmed_final = final_track[:min_shape[0], ...][:, :min_shape[1], ...] if final_track.ndim > 1 else final_track[:min_shape[0]]
                        output += trimmed_output + trimmed_final
                return (sr, output)

            merge_btn.click(merge, set(audios + volumes), output_audio)

    merge_btn = gr.Button("Merge Tracks")
    output_audio = gr.Audio(label="Output", interactive=False)

demo.launch()

```

Two things to note in this app:

1. Here we provide `key=` to all the components! We need to do this so that if we add another track after setting the values for an existing track, our input values to the existing track do not get reset on re-render.
2. When there are lots of components of different types and arbitrary counts passed to an event listener, it is easier to use the set and dictionary notation for inputs rather than list notation. Above, we make one large set of all the input `gr.Audio` and `gr.Slider` components when we pass the inputs to the `merge` function. In the function body we query the component values as a dict.

The `gr.render` expands gradio capabilities extensively - see what you can make out of it!

[←

State In Blocks](../guides/state-in-blocks/) [More Blocks Features

→](../guides/more-blocks-features/)

Status