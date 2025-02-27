# github_models
Github models


# Github models
1. https://github.com/marketplace/models


# Github Playground
1. https://github.com/marketplace/models/azureml/Phi-3-small-8k-instruct/playground

<img width="1701" alt="image" src="https://github.com/user-attachments/assets/be9ca8c3-993f-437b-a6e2-f515a90ea114" />


# Use this Model

##@ Get started
1. Below are example code snippets for a few use cases. For additional information about Azure AI Inference SDK, see full documentation and samples.

### 1. Create a personal access token

To authenticate with the model you will need to generate a personal access token (PAT) in your GitHub settings or set up an Azure production key.


### GitHub Free

Access AI inference with your GitHub PAT. Learn more about limits based on your plan.


### Azure AI by Azure Pay as you go

Access pay-as-you-go inference and more AI services on Azure.

You do not need to give any permissions to the token. Note that the token will be sent to a Microsoft service.

To use the code snippets below, create an environment variable to set your token as the key for the client code.


If you're using bash:

export GITHUB_TOKEN="<your-github-token-goes-here>"

If you're in powershell:

$Env:GITHUB_TOKEN="<your-github-token-goes-here>"

If you're using Windows command prompt:

set GITHUB_TOKEN=<your-github-token-goes-here>

### 2. Install dependencies

Install the Azure AI Inference SDK using pip (Requires: Python >=3.8):

pip install azure-ai-inference

### 3. Run a basic code sample

This sample demonstrates a basic call to the chat completion API. It is leveraging the GitHub AI model inference endpoint and your GitHub token. The call is synchronous.

``
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

endpoint = "https://models.inference.ai.azure.com"
model_name = "Phi-3-small-8k-instruct"
token = os.environ["GITHUB_TOKEN"]

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

response = client.complete(
    messages=[
        UserMessage("What is the capital of France?"),
    ],
    temperature=1.0,
    top_p=1.0,
    max_tokens=1000,
    model=model_name
)

print(response.choices[0].message.content)

``

## 4. Explore more samples

Run a multi-turn conversation

This sample demonstrates a multi-turn conversation with the chat completion API. When using the model for a chat application, you'll need to manage the history of that conversation and send the latest messages to the model.

``
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import AssistantMessage, SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

token = os.environ["GITHUB_TOKEN"]
endpoint = "https://models.inference.ai.azure.com"
model_name = "Phi-3-small-8k-instruct"

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

messages = [
    UserMessage("What is the capital of France?"),
    AssistantMessage("The capital of France is Paris."),
    UserMessage("What about Spain?"),
]

response = client.complete(messages=messages, model=model_name)

print(response.choices[0].message.content)

``

## Stream the output

For a better user experience, you will want to stream the response of the model so that the first token shows up early and you avoid waiting for long responses.

``

import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

token = os.environ["GITHUB_TOKEN"]
endpoint = "https://models.inference.ai.azure.com"
model_name = "Phi-3-small-8k-instruct"

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

response = client.complete(
    stream=True,
    messages=[
        UserMessage("Give me 5 good reasons why I should exercise every day."),
    ],
    model_extras = {'stream_options': {'include_usage': True}},
    model=model_name,
)

usage = {}
for update in response:
    if update.choices and update.choices[0].delta:
        print(update.choices[0].delta.content or "", end="")
    if update.usage:
        usage = update.usage

if usage:
    print("\n")
    for k, v in usage.items():
        print(f"{k} = {v}")


client.close()

``

## 5. Going beyond rate limits

The rate limits for the playground and free API usage are intended to help you experiment with models and prototype your AI application. For use beyond those limits, and to bring your application to scale, you must provision resources from an Azure account, and authenticate from there instead of your GitHub personal access token. You don't need to change anything else in your code. Use this link to discover how to go beyond the free tier limits in Azure AI.
