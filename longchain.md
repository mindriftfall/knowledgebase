Text generation pattern using LangChain

For text generation, you can also use a LangChain, an open source library, for specific text generation use cases. You can pair LangChain with the text generation FMs on Amazon Bedrock to support both text generation and to develop conversational, AI Assistants.

LangChain is a framework for developing applications powered by LLMs. LangChain provides the software building blocks to reduce the complexity of building functionality from scratch. LangChain simplifies every stage of the LLM application lifecycle, including development, productionization, and deployment. You can use it to take full advantage of the power of the LLMs.

LangChain supports integration for Amazon Bedrock with the langchain.aws and langchain-community packages.  Amazon Bedrock currently supports Titan Text models from Amazon, Jurassic models from AI21, Claude from Anthropic, Cohere Command and Embed, Llama models from Meta, Stable Diffusion models from Stability AI, and Mistral models from Mistral AI.


<img width="1014" height="454" alt="image" src="https://github.com/user-attachments/assets/26724062-34e3-4b3c-b7a4-96f23356250b" />


LangChain acts as an orchestration layer between the prompt input request and the generated output from the Amazon Bedrock FM. The architecture pattern for text generation with LangChain is illustrated in the following diagram. 

- https://docs.langchain.com/oss/python/overview
## AWS LongChain
- https://python.langchain.com/v0.2/docs/integrations/platforms/aws/

LLMs take text as input and generate text as output, and LangChain provides LLM components to interact with different language models. The LLM class is an abstraction for working across different providers and is used by LangChain to interact with an LLM model.

The following example demonstrates how to create an instance of the Amazon Bedrock class and invoke an Amazon Titan LLM from the LangChain LLM module. The model_id is the value of the selected model available in Amazon Bedrock.

```python
import boto3
from langchain_aws import BedrockLLM
bedrock_client = boto3.client('bedrock-runtime',region_name="us-east-1")
inference_modifiers = {"temperature": 0.3, "maxTokenCount": 512}
llm = BedrockLLM(
    client = bedrock_client,
    model_id="amazon.titan-tg1-large",
    model_kwargs =inference_modifiers
    streaming=True,
)
response = llm.invoke("What is the largest city in Vermont?")
print(response)
```

## Chat models 
Conversational interfaces, such as chatbots and virtual assistants, can lower the cost of customer support, while improving customer experience. LangChain provides a chat models component to build conversational applications. This component accepts content in the form of messages that contain input text.

The following example demonstrates how you can get a response from an LLM by passing a user request to the LLM.
```python
from langchain_aws import ChatBedrock as Bedrock
from langchain.schema import HumanMessage
chat = Bedrock(model_id="anthropic.claude-3-sonnet-20240229-v1:0", model_kwargs={"temperature":0.1})

messages = [
     HumanMessage(
          content="I would like to try Indian food, what do you suggest should I try?"
     )
]
chat.invoke(messages)
```
Response:
AIMessage(content="Here are some delicious and popular Indian dish recommendations for someone trying Indian food for the first time:\n\n- Butter Chicken (Murgh Makhani) - Tender chicken in an aromatic tomato-based curry with butter and cream. It's flavorful but not too spicy.\n\n- Chicken Tikka Masala - Chunks of roasted marinated chicken in a creamy tomato-based sauce.\n\n- Palak Paneer - A vegetarian dish with paneer (fresh cheese cubes) cooked in a thick spinach-based curry.\n\n- Chana Masala - A vegetarian chickpea curry flavored with spices like cumin, coriander and garam masala.\n\n- Samosas - Fried or baked pastry pockets stuffed with a savory potato/vegetable filling.\n\n- Naan - Leavened oven-baked flatbread, perfect for soaking up curries. Try garlic or butter naan.\n\n- Biryani - A flavorful mixed rice dish with meat/vegetables and aromatic spices.\n\nI'd recommend getting a combination platter or thali to sample a variety of dishes on your first visit. Start with milder dishes and work your way up if you want more heat/spice. Indian food has incredible depth of flavor!", additional_kwargs={'usage': {'prompt_tokens': 23, 'completion_tokens': 300, 'total_tokens': 323}}, response_metadata={'model_id': 'anthropic.claude-3-sonnet-20240229-v1:0', 'usage': {'prompt_tokens': 23, 'completion_tokens': 300, 'total_tokens': 323}}, id='run-09f0ef8a-90c0-47c3-b172-546b084d1288-0')


