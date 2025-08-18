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

## LangChain memory
LangChain memory provides the mechanism to store and summarize (if needed) prior conversational elements that are included in the context on subsequent invocations. LangChain provides components in the form of helper utilities for managing and manipulating previous chat messages. These utilities are modular. You can chain them with other components and interact with different types of abstractions to build powerful chatbots.

In LangChain, there are different ways you can implement conversational memory for a chatbot as follows:

    •

ConversationBufferMemory: The ConversationBufferMemory is the most common type of memory in LangChain. It includes past conversations that happened between the user and the LLM.
•

ConversationChain: The ConversationBufferMemory is built on top of ConversationChain, which is designed for managing conversations. For a complete list of supported memory types, refer to Memory Types

```python
from langchain.chains import ConversationChain
from langchain_aws import BedrockLLM
from langchain.memory import ConversationBufferMemory
bedrock_client = boto3.client('bedrock-runtime',region_name="us-east-1")


titan_llm = BedrockLLM(model_id="amazon.titan-tg1-large", client=bedrock_client)
memory = ConversationBufferMemory()

conversation = ConversationChain(
    llm=titan_llm, verbose=True, memory=memory
)

print(conversation.predict(input="Hi! I am in Los Angeles. What are some of the popular sightseeing places?"))
```
## Processing large amounts of data
Chains are also helpful when processing more data than can be contained in the context. For example, you might do a document summarization against a very large document. The chain will chunk up the document and make multiple calls to the LLM to produce a single summary.

Chains refer to sequences of calls - whether to an LLM, a tool, or a data preprocessing step.  LangChain Expression Language(opens in a new tab) (LCEL) is the primary way to interact with chains, and is great for constructing chains. But there are two types of off-the-shelf chains that LangChain supports:

    Chain that are built using LCEL.

    Legacy chains constructed by subclassing from a legacy ‘Chain’ class i.e. LLMChain etc.

For the complete list of supported chains, refer to Chains(opens in a new tab).

The following example demonstrates how to use chains to call an LLM multiple times in a sequence using LLMChain.
```python
from langchain import PromptTemplate
from langchain.chains import LLMChain
from langchain_aws import ChatBedrock as Bedrock

chat = Bedrock(
     region_name = "us-east-1",
     model_kwargs={"temperature":1,"top_k":250,"top_p":0.999,"anthropic_version":"bedrock-2023-05-31"},
     model_id="anthropic.claude-3-sonnet-20240229-v1:0"
)

multi_var_prompt = PromptTemplate(
     input_variables=["company"],
     template="Create a list with the names of the main metrics tracked in the reports of {company}?",
)
 
chain = LLMChain(llm=chat, prompt=multi_var_prompt)
answers = chain.invoke("Amazon")
print(answers)

answers = chain.invoke("AWS")
print(answers)
```

## Managing External Resources with LangChain Agents
LangChain agents can interact with external sources, such as search engines, calculators, APIs, or databases. The agents can also run code to perform actions to assist the LLMs in generating accurate responses. LangChain provides the chain interface to sequence multiple components to build an application. An LLMChain is an example of a basic chain.

The following mechanisms use basic chains:

A RAG application can use an LLMChain to return a response. The response is based on two pieces of information: the user query and a context supplied as a set of documents retrieved from a vector store.
•

A RouterChain is an example of a complex chain. For example, you can use a RouterChain to select one prompt template from the available prompt templates based on user input. 
•

The LangChain agents act as reasoning engines for the LLMs to decide the actions to take and the order in which to take the actions. An action can be a tool that uses the results from a search engine or a math calculator.

LangChain agent features 

The LangChain agent follows a sequence of actions :

    1 LangChain provides tools and toolkits (a group of three to five different tools) in the form of functions for the agent to call. The agent is formed with access to an LLM, a set of tools, and a stopping condition.
    2The agent uses the ReAct (reasoning and acting) prompting framework to choose the most relevant tool from the set of tools provided based on the user input.
    3 The agent repeatedly decides the action, runs the action, and observes the output of the tool until the stopping condition is met. For more information, refer to ReAct(opens in a new tab).

```python
from langchain.agents import load_tools
from langchain.agents import initialize_agent, Tool
from langchain.agents import AgentType
from langchain import LLMMathChain
from langchain_aws import ChatBedrock
from langchain.agents import AgentExecutor, create_react_agent

chat = ChatBedrock(model_id="anthropic.claude-3-sonnet-20240229-v1:0", model_kwargs={"temperature":0.1})

prompt_template = """Answer the following questions as best you can.
You have access to the following tools:\n\n{tools}\n\n
Use the following format:\n\nQuestion: the input question you must answer\n
Thought: you should always think about what to do\n
Action: the action to take, should be one of [{tool_names}]\n
Action Input: the input to the action\nObservation: the result of the action\n...
(this Thought/Action/Action Input/Observation can repeat N times)\n
Thought: I now know the final answer\n
Final Answer: the final answer to the original input question\n\nBegin!\n\n
Question: {input}\nThought:{agent_scratchpad}
"""
modelId = "anthropic.claude-3-sonnet-20240229-v1:0"

react_agent_llm = ChatBedrock(model_id=modelId, client=bedrock_client)
math_chain_llm = ChatBedrock(model_id=modelId, client=bedrock_client)

tools = load_tools([], llm=react_agent_llm)

llm_math_chain = LLMMathChain.from_llm(llm=math_chain_llm, verbose=True)

llm_math_chain.llm_chain.prompt.template = """Human: Given a question with a math problem, provide only a single line mathematical expression that solves the problem in the following format. Don't solve the expression only create a parsable expression.
```text
{{single line mathematical expression that solves the problem}}
```

Assistant:
Here is an example response with a single line mathematical expression for solving a math problem:
```text
37593**(1/5)
```

Human: {question}
Assistant:"""

tools.append(
    Tool.from_function(
         func=llm_math_chain.(opens in a new tab)run(opens in a new tab),
         name="Calculator",
         description="Useful for when you need to answer questions about math.",
    )
)

react_agent = create_react_agent(react_agent_llm,
    tools,
    PromptTemplate.from_template(prompt_template)
         # max_iteration=2,
         # return_intermediate_steps=True,
         # handle_parsing_errors=True,
    )

agent_executor = AgentExecutor(
agent=react_agent,
tools=tools,
verbose=True,
handle_parsing_errors=True,
max_iterations = 10 # useful when agent is stuck in a loop
)

agent_executor.invoke({"input": "What is the distance between San Francisco and Los Angeles? If I travel from San Francisco to Los Angeles with the speed of 40MPH how long will it take to reach?"})
```


