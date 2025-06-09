# Object Detection From Video

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

Streaming Ai Generated AudioObject Detection From Webcam With WebrtcObject Detection From Video

Setting up the ModelThe Inference FunctionThe Gradio DemoConclusion

Conversational ChatbotReal Time Speech RecognitionAutomatic Voice Detection

Custom Components

Custom Components In Five MinutesKey Component ConceptsConfigurationBackendFrontendFrequently Asked QuestionsPdf Component ExampleMultimodal Chatbot Part1Documenting Custom Components

Gradio Clients And Lite

Getting Started With The Python ClientGetting Started With The Js ClientQuerying Gradio Apps With CurlGradio And Llm AgentsGradio LiteGradio Lite And Transformers JsFastapi App With The Gradio Client

Other Tutorials [ show ]

Using Hugging Face IntegrationsBuilding An Mcp Client With GradioBuilding Mcp Server With GradioUsing Docs McpGradio And CometGradio And ONNX On Hugging FaceGradio And Wandb IntegrationCreate Your Own Friends With A GanCreating A Dashboard From Bigquery DataCreating A Dashboard From Supabase DataCreating A Realtime Dashboard From Google SheetsDeploying Gradio With DockerDeveloping Faster With Reload ModeHow To Use 3D Model ComponentImage Classification In PytorchImage Classification With Vision TransformersInstalling Gradio In A Virtual EnvironmentNamed Entity RecognitionPlot Component For MapsRunning Background TasksRunning Gradio On Your Web Server With NginxSetting Up A Demo For Maximum PerformanceStyling The Gradio DataframeTheming GuideUnderstanding Gradio Share LinksUsing FlaggingUsing Gradio For Tabular WorkflowsUsing Gradio In Other Programming LanguagesWrapping Layouts

1. Streaming
2. Object Detection From Video

[←

Object Detection From Webcam With Webrtc](../guides/object-detection-from-webcam-with-webrtc/) [Conversational Chatbot

→](../guides/conversational-chatbot/)

# Streaming Object Detection from Video

In this guide we'll use the RT-DETR model to detect objects in a user uploaded video. We'll stream the results from the server using the new video streaming features introduced in Gradio 5.0.

## Setting up the Model

First, we'll install the following requirements in our system:

```
opencv-python
torch
transformers>=4.43.0
spaces
```

Then, we'll download the model from the Hugging Face Hub:

```
from transformers import RTDetrForObjectDetection, RTDetrImageProcessor

image_processor = RTDetrImageProcessor.from_pretrained("PekingU/rtdetr_r50vd")
model = RTDetrForObjectDetection.from_pretrained("PekingU/rtdetr_r50vd").to("cuda")
```

We're moving the model to the GPU. We'll be deploying our model to Hugging Face Spaces and running the inference in the free ZeroGPU cluster.

## The Inference Function

Our inference function will accept a video and a desired confidence threshold.
Object detection models identify many objects and assign a confidence score to each object. The lower the confidence, the higher the chance of a false positive. So we will let our users set the conference threshold.

Our function will iterate over the frames in the video and run the RT-DETR model over each frame.
We will then draw the bounding boxes for each detected object in the frame and save the frame to a new output video.
The function will yield each output video in chunks of two seconds.

In order to keep inference times as low as possible on ZeroGPU (there is a time-based quota),
we will halve the original frames-per-second in the output video and resize the input frames to be half the original
size before running the model.

The code for the inference function is below - we'll go over it piece by piece.

```
import spaces
import cv2
from PIL import Image
import torch
import time
import numpy as np
import uuid

from draw_boxes import draw_bounding_boxes

SUBSAMPLE = 2

@spaces.GPU
def stream_object_detection(video, conf_threshold):
    cap = cv2.VideoCapture(video)

    # This means we will output mp4 videos
    video_codec = cv2.VideoWriter_fourcc(*"mp4v") # type: ignore
    fps = int(cap.get(cv2.CAP_PROP_FPS))

    desired_fps = fps // SUBSAMPLE
    width  = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH)) // 2
    height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT)) // 2

    iterating, frame = cap.read()

    n_frames = 0

    # Use UUID to create a unique video file
    output_video_name = f"output_{uuid.uuid4()}.mp4"

    # Output Video
    output_video = cv2.VideoWriter(output_video_name, video_codec, desired_fps, (width, height)) # type: ignore
    batch = []

    while iterating:
        frame = cv2.resize( frame, (0,0), fx=0.5, fy=0.5)
        frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
        if n_frames % SUBSAMPLE == 0:
            batch.append(frame)
        if len(batch) == 2 * desired_fps:
            inputs = image_processor(images=batch, return_tensors="pt").to("cuda")

            with torch.no_grad():
                outputs = model(**inputs)

            boxes = image_processor.post_process_object_detection(
                outputs,
                target_sizes=torch.tensor([(height, width)] * len(batch)),
                threshold=conf_threshold)

            for i, (array, box) in enumerate(zip(batch, boxes)):
                pil_image = draw_bounding_boxes(Image.fromarray(array), box, model, conf_threshold)
                frame = np.array(pil_image)
                # Convert RGB to BGR
                frame = frame[:, :, ::-1].copy()
                output_video.write(frame)

            batch = []
            output_video.release()
            yield output_video_name
            output_video_name = f"output_{uuid.uuid4()}.mp4"
            output_video = cv2.VideoWriter(output_video_name, video_codec, desired_fps, (width, height)) # type: ignore

        iterating, frame = cap.read()
        n_frames += 1
```

1. **Reading from the Video**

One of the industry standards for creating videos in python is OpenCV so we will use it in this app.

The `cap` variable is how we will read from the input video. Whenever we call `cap.read()`, we are reading the next frame in the video.

In order to stream video in Gradio, we need to yield a different video file for each "chunk" of the output video.
We create the next video file to write to with the `output_video = cv2.VideoWriter(output_video_name, video_codec, desired_fps, (width, height))` line. The `video_codec` is how we specify the type of video file. Only "mp4" and "ts" files are supported for video sreaming at the moment.

2. **The Inference Loop**

For each frame in the video, we will resize it to be half the size. OpenCV reads files in `BGR` format, so will convert to the expected `RGB` format of transfomers. That's what the first two lines of the while loop are doing.

We take every other frame and add it to a `batch` list so that the output video is half the original FPS. When the batch covers two seconds of video, we will run the model. The two second threshold was chosen to keep the processing time of each batch small enough so that video is smoothly displayed in the server while not requiring too many separate forward passes. In order for video streaming to work properly in Gradio, the batch size should be at least 1 second.

We run the forward pass of the model and then use the `post_process_object_detection` method of the model to scale the detected bounding boxes to the size of the input frame.

We make use of a custom function to draw the bounding boxes (source here). We then have to convert from `RGB` to `BGR` before writing back to the output video.

Once we have finished processing the batch, we create a new output video file for the next batch.

## The Gradio Demo

The UI code is pretty similar to other kinds of Gradio apps.
We'll use a standard two-column layout so that users can see the input and output videos side by side.

In order for streaming to work, we have to set `streaming=True` in the output video. Setting the video
to autoplay is not necessary but it's a better experience for users.

```
import gradio as gr

with gr.Blocks() as app:
    gr.HTML(
        """
    <h1 style='text-align: center'>
    Video Object Detection with <a href='https://huggingface.co/PekingU/rtdetr_r101vd_coco_o365' target='_blank'>RT-DETR</a>
    </h1>
    """)
    with gr.Row():
        with gr.Column():
            video = gr.Video(label="Video Source")
            conf_threshold = gr.Slider(
                label="Confidence Threshold",
                minimum=0.0,
                maximum=1.0,
                step=0.05,
                value=0.30,
            )
        with gr.Column():
            output_video = gr.Video(label="Processed Video", streaming=True, autoplay=True)

    video.upload(
        fn=stream_object_detection,
        inputs=[video, conf_threshold],
        outputs=[output_video],
    )

```

## Conclusion

You can check out our demo hosted on Hugging Face Spaces here.

It is also embedded on this page below

[←

Object Detection From Webcam With Webrtc](../guides/object-detection-from-webcam-with-webrtc/) [Conversational Chatbot

→](../guides/conversational-chatbot/)

Status