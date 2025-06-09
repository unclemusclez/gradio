# Streaming Ai Generated Audio

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

Streaming Ai Generated Audio

The OverviewThe UIThe LogicConclusion

Object Detection From Webcam With WebrtcObject Detection From VideoConversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Streaming
2. Streaming Ai Generated Audio

[←

Connecting To A Database](../guides/connecting-to-a-database/) [Object Detection From Webcam With Webrtc

→](../guides/object-detection-from-webcam-with-webrtc/)

# Streaming AI Generated Audio

In this guide, we'll build a novel AI application to showcase Gradio's audio output streaming. We're going to a build a talking Magic 8 Ball 🎱

A Magic 8 Ball is a toy that answers any question after you shake it. Our application will do the same but it will also speak its response!

We won't cover all the implementation details in this blog post but the code is freely available on Hugging Face Spaces.

## The Overview

Just like the classic Magic 8 Ball, a user should ask it a question orally and then wait for a response. Under the hood, we'll use Whisper to transcribe the audio and then use an LLM to generate a magic-8-ball-style answer. Finally, we'll use Parler TTS to read the response aloud.

## The UI

First let's define the UI and put placeholders for all the python logic.

```
import gradio as gr

with gr.Blocks() as block:
    gr.HTML(
        f"""
        <h1 style='text-align: center;'> Magic 8 Ball 🎱 </h1>
        <h3 style='text-align: center;'> Ask a question and receive wisdom </h3>
        <p style='text-align: center;'> Powered by <a href="https://github.com/huggingface/parler-tts"> Parler-TTS</a>
        """
    )
    with gr.Group():
        with gr.Row():
            audio_out = gr.Audio(label="Spoken Answer", streaming=True, autoplay=True)
            answer = gr.Textbox(label="Answer")
            state = gr.State()
        with gr.Row():
            audio_in = gr.Audio(label="Speak your question", sources="microphone", type="filepath")

    audio_in.stop_recording(generate_response, audio_in, [state, answer, audio_out])\
        .then(fn=read_response, inputs=state, outputs=[answer, audio_out])

block.launch()
```

We're placing the output Audio and Textbox components and the input Audio component in separate rows. In order to stream the audio from the server, we'll set `streaming=True` in the output Audio component. We'll also set `autoplay=True` so that the audio plays as soon as it's ready.
We'll be using the Audio input component's `stop_recording` event to trigger our application's logic when a user stops recording from their microphone.

We're separating the logic into two parts. First, `generate_response` will take the recorded audio, transcribe it and generate a response with an LLM. We're going to store the response in a `gr.State` variable that then gets passed to the `read_response` function that generates the audio.

We're doing this in two parts because only `read_response` will require a GPU. Our app will run on Hugging Faces ZeroGPU which has time-based quotas. Since generating the response can be done with Hugging Face's Inference API, we shouldn't include that code in our GPU function as it will needlessly use our GPU quota.

## The Logic

As mentioned above, we'll use Hugging Face's Inference API to transcribe the audio and generate a response from an LLM. After instantiating the client, I use the `automatic_speech_recognition` method (this automatically uses Whisper running on Hugging Face's Inference Servers) to transcribe the audio. Then I pass the question to an LLM (Mistal-7B-Instruct) to generate a response. We are prompting the LLM to act like a magic 8 ball with the system message.

Our `generate_response` function will also send empty updates to the output textbox and audio components (returning `None`).
This is because I want the Gradio progress tracker to be displayed over the components but I don't want to display the answer until the audio is ready.

```
from huggingface_hub import InferenceClient

client = InferenceClient(token=os.getenv("HF_TOKEN"))

def generate_response(audio):
    gr.Info("Transcribing Audio", duration=5)
    question = client.automatic_speech_recognition(audio).text

    messages = [{"role": "system", "content": ("You are a magic 8 ball."
                                              "Someone will present to you a situation or question and your job "
                                              "is to answer with a cryptic adage or proverb such as "
                                              "'curiosity killed the cat' or 'The early bird gets the worm'."
                                              "Keep your answers short and do not include the phrase 'Magic 8 Ball' in your response. If the question does not make sense or is off-topic, say 'Foolish questions get foolish answers.'"
                                              "For example, 'Magic 8 Ball, should I get a dog?', 'A dog is ready for you but are you ready for the dog?'")},
                {"role": "user", "content": f"Magic 8 Ball please answer this question -  {question}"}]

    response = client.chat_completion(messages, max_tokens=64, seed=random.randint(1, 5000),
                                      model="mistralai/Mistral-7B-Instruct-v0.3")

    response = response.choices[0].message.content.replace("Magic 8 Ball", "").replace(":", "")
    return response, None, None
```

Now that we have our text response, we'll read it aloud with Parler TTS. The `read_response` function will be a python generator that yields the next chunk of audio as it's ready.

We'll be using the Mini v0.1 for the feature extraction but the Jenny fine tuned version for the voice. This is so that the voice is consistent across generations.

Streaming audio with transformers requires a custom Streamer class. You can see the implementation here. Additionally, we'll convert the output to bytes so that it can be streamed faster from the backend.

```
from streamer import ParlerTTSStreamer
from transformers import AutoTokenizer, AutoFeatureExtractor, set_seed
import numpy as np
import spaces
import torch
from threading import Thread

device = "cuda:0" if torch.cuda.is_available() else "mps" if torch.backends.mps.is_available() else "cpu"
torch_dtype = torch.float16 if device != "cpu" else torch.float32

repo_id = "parler-tts/parler_tts_mini_v0.1"

jenny_repo_id = "ylacombe/parler-tts-mini-jenny-30H"

model = ParlerTTSForConditionalGeneration.from_pretrained(
    jenny_repo_id, torch_dtype=torch_dtype, low_cpu_mem_usage=True
).to(device)

tokenizer = AutoTokenizer.from_pretrained(repo_id)
feature_extractor = AutoFeatureExtractor.from_pretrained(repo_id)

sampling_rate = model.audio_encoder.config.sampling_rate
frame_rate = model.audio_encoder.config.frame_rate

@spaces.GPU
def read_response(answer):

    play_steps_in_s = 2.0
    play_steps = int(frame_rate * play_steps_in_s)

    description = "Jenny speaks at an average pace with a calm delivery in a very confined sounding environment with clear audio quality."
    description_tokens = tokenizer(description, return_tensors="pt").to(device)

    streamer = ParlerTTSStreamer(model, device=device, play_steps=play_steps)
    prompt = tokenizer(answer, return_tensors="pt").to(device)

    generation_kwargs = dict(
        input_ids=description_tokens.input_ids,
        prompt_input_ids=prompt.input_ids,
        streamer=streamer,
        do_sample=True,
        temperature=1.0,
        min_new_tokens=10,
    )

    set_seed(42)
    thread = Thread(target=model.generate, kwargs=generation_kwargs)
    thread.start()

    for new_audio in streamer:
        print(f"Sample of length: {round(new_audio.shape[0] / sampling_rate, 2)} seconds")
        yield answer, numpy_to_mp3(new_audio, sampling_rate=sampling_rate)
```

## Conclusion

You can see our final application here!

[←

Connecting To A Database](../guides/connecting-to-a-database/) [Object Detection From Webcam With Webrtc

→](../guides/object-detection-from-webcam-with-webrtc/)

Status