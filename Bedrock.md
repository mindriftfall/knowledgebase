Amazon Bedrock provides comprehensive data protection and privacy. Your data used with Amazon Bedrock is not used for service improvement and is not shared with third-party model providers. You can use AWS PrivateLink with Amazon Bedrock to establish private connectivity between your FMs and your virtual private cloud (VPC) without exposing your traffic to the internet.

Your data is encrypted in transit and at rest. You can customize FMs privately so you can control how your data is used and encrypted. Amazon Bedrock makes a separate copy of the base foundation model and trains the private copy of the model.

For content Type.
Converse API:

```python-repl
import boto3
client = boto3.client("bedrock-runtime")
model_id = "amazon.titan-tg1-large"

# Inference parameters to use.
temperature = 0.5
top_p = 0.9

inference_config={"temperature": temperature,"topP": top_p}

# Setup the system prompts and messages to send to the model.
system_prompts = [{"text": "You are a helpful assistance. Please answer the query politely."}]
conversation = [
    {
        "role": "user",
        "content": [{"text": "Hello, what is the capital of France?"}]
    }
]
# Use converse API
response = client.converse(
    modelId=model_id,
    messages=conversation,
    inferenceConfig=inference_config
)
print(response['output']['message']["content"][0]["text"])
```

https://builder.aws.com/content/2dtauBCeDa703x7fDS9Q30MJoBA/a-developers-guide-to-bedrocks-new-converse-api



### Randomness and diversity

Foundation
 models typically support the following parameters to control randomness
 and diversity in the response. To learn about the randomness and
diversity parameters, expand each of the following three tabs.

**Temperature**
 controls randomness in word choice. Lower values lead to more
predictable responses. The following table lists minimum, maximum, and
default values for the temperature parameter.

Note: For users with screen readers, use table mode to read the table.

| **Parameter**`` | **JSON Field Format**`` | **Minimum**`` | **Maximum**`` | **Default**`` |
| ---------------------------- | ------------------------------------ | -------------------------- | -------------------------- | -------------------------- |
| Temperature``         | temperature``                 | 0``                 | 1``                 | 0                          |

**Top K**  limits word choices to the K most probable options. Lower values reduce unusual responses.


**Top P** cuts off low
probability word choices based on cumulative probability. It tightens
overall response distribution. The following table lists minimum,
maximum, and default values for the Top P parameter.

Note: For users with screen readers, use table mode to read the table.

| **Parameter** | **JSON Field Format** | **Minimum** | **Maximum** | **Default** |
| ------------------- | --------------------------- | ----------------- | ----------------- | ----------------- |
| Top P               | topP                        | 0                 | 1                 | 1                 |


### **Length**

Foundation
 models typically support the following parameters to control the length
 of the generated response. To learn about the length parameters, expand
 each of the following three tabs.

**Response length** sets minimum and maximum token
counts. It sets a hard limit on response size. The following table lists
 minimum, maximum, and default values for the response length parameter.
 Maximum response length is dependent on the specific FM.

Note: For users with screen readers, use table mode to read the table.

| **Parameter** | **JSON Field Format** | **Minimum** | **Maximum** | **Default** |
| ------------------- | --------------------------- | ----------------- | ----------------- | ----------------- |
| Response length     | maxTokenCount               | 0                 | 8,000             | 512               |

**Length penalty** encourages more concise responses by penalizing longer ones. It sets a soft limit on size.

**Stop sequences** include
 specific character combinations that signal the model to stop
generating tokens when encountered. It is used for the early termination
 of responses.
