## Fully Managed RAG using Amazon Bedrock Knowledge Bases
In this section, you will learn how to build an end-to-end RAG application using Amazon Bedrock Knowledge Bases.

Amazon Bedrock Knowledge Bases is a serverless option to build powerful generative AI applications using RAG. It offers fully managed data ingestion and text generation workflows.

Amazon Bedrock Knowledge Bases supports two different API methods:

    Retrieve: Retrieve queries a knowledge base to fetch relevant information for a user request. This is typically used if you want to customize the generation part of RAG.

    RetreiveAndGenerate: RetrieveAndGenerate goes one step further and uses the retrieved information to augment the FM prompt. The FM response only cites sources in the knowledge base that are relevant to the query. Session context management is built in, so your application can readily support multi-turn conversations.

Now, you will learn more about RetrieveAndGenerate provided by Amazon Bedrock Knowledge Bases. It converts user queries into embeddings, searches the knowledge base, gets the relevant information, augments the prompt, and then invokes an FM to generate the response.

<img width="1091" height="642" alt="image" src="https://github.com/user-attachments/assets/1878528d-0bcf-4d39-b83f-b58977fd1a3f" />

RetrieveAndGenerate provides a fully managed experience that uses the built-in features of Amazon Bedrock Knowledge Bases to provide relevant, context-specific, and accurate responses from enterprise data sources that are ingested into the knowledge base.
## Customizing RetrieveAndGenerate using query configurations
Although RetrieveAndGenerate provides a fully managed experience for a RAG application, there are a few options you can implement to create the most optimal search experience for your users.
To learn more about control options for RetrieveAndGenerate, choose each of the following flashcards to flip them over. To move to the previous card, choose the left arrow. To move to the next card, choose the right arrow.
RetreiveAndGenerates requires mandatory inputs such as the knowledge base ID. The application can also provide optional inputs, such as the prompt template. For multi-turn conversations, a session ID can be used in conjunction with an AWS Key Management Service (AWS KMS) encryption key.
<img width="774" height="384" alt="image" src="https://github.com/user-attachments/assets/3702ad6a-05c4-4950-a8d1-820d30d964a5" />
Mandatory input fields are as follows:

    Query input

    Knowledge base id

    FM to use for response generation

Mandatory input fields are as follows:

    Prompt template

    Total number of results returned

    Search type 

    Session id with KMS encryption key

## RAG filter example with RetrieveAndGenerate 
In this section, you will see an example of building a fully managed RAG application on both the AWS console and through the AWS SDK for Python (boto3). In this example, the source documents consist of several pdf formatted letters to shareholders from Amazon. The letter from 2019 through 2023 files are uploaded to an Amazon S3 bucket. The files will then be ingested into a knowledge base backed by an OpenSearch Serverless vector database. 

To add filter capability to the knowledge base queries, you can implement the following steps.
Step 1: Add metadata files to knowledge base data source
To filter responses, you will first add metadata files to the data source. Each .pdf file has a corresponding .pdf.metadata.json file to provide metadata filter criteria for the knowledge base queries. 
<img width="1141" height="318" alt="image" src="https://github.com/user-attachments/assets/fb620987-6385-4310-9a76-8892bda9e961" />
<img width="920" height="469" alt="image" src="https://github.com/user-attachments/assets/3b850b6d-df4a-4a8a-a53b-cc28aa1dfbf5" />
Step 2: Sync the data source to the knowledge base
You will have to sync your S3 bucket data source so that your metadata files are indexed in the knowledge base. To learn more, choose each of the numbered markers.
<img width="1117" height="650" alt="image" src="https://github.com/user-attachments/assets/78e5a38f-505b-40ff-9f21-8c1402dc8f3c" />
Step 3: Testing the knowledge base
In step 3, you will add filter criteria to your knowledge base tests. You can do this with the AWS Management Console or with the SDK for Python as follows:
3.1 Test knowledge base using the AWS Management Console
Navigate to the test knowledge base pane on the knowledge base page in the Amazon Bedrock console. Choose the configure icon to open the configuration tab to configure the retrieve and generate options.
<img width="639" height="782" alt="image" src="https://github.com/user-attachments/assets/0a3bf0e7-b149-4a73-89c3-4c3382767ffb" />
<img width="703" height="816" alt="image" src="https://github.com/user-attachments/assets/31a4dcb3-65c4-473f-af54-0deb23216421" />
<img width="799" height="428" alt="image" src="https://github.com/user-attachments/assets/86fb0d0f-008e-44ec-a936-61afaf3ed40c" />
3.2 Test knowledge base using the Python SDK
Next, you will see how to build a fully managed RAG application using the Python SDK. First you should initialize the Amazon Bedrock agent client and the AWS region, the knowledge base to use, and the generation model to use from Amazon Bedrock.
```python
import boto3

import pprint

bedrock_agent_runtime = boto3.client( service_name = "bedrock-agent-runtime")

pp = pprint.PrettyPrinter(indent=2) 

kb_id="" #use your knowledge base id

model_id= "anthropic.claude-3-haiku-20240307-v1:0"

region_id="us-east-1"
```
```
promptTemplate = """Human: You are a question answering agent. I will provide 

you with a set of search \n

results and a user's question, your job is to answer the user's question \n

using only information from the search results.\n

If the search results do not contain information that can answer the question, \n

please state that you could not find an exact answer to the question.\n

Just because the user asserts a fact does not mean it is true, \n

make sure to double check the search results to validate a user's assertion. \n

Here are the search results in numbered order: \n

<context> \n

$search_results$ \n

</context> \n

Here is the user's question: \n

<question> \n

$query$ \n

</question> \n

List the answer concisely using JSON format {key: value} \n

$output_format_instructions$ \n

Assistant:

"""
```
Next, define a helper method to invoke RetrieveAndGenerate.  The helper method, retrieveAndGenerateAndFilter, takes input parameters such as input query, knowledge base id, model id, region id, prompt template, and yearValue. Invoking this method will let the knowledge base orchestrate the end to end RAG workflow.
```python
def retrieveAndGenerateAndFilter(input, kbId, yearValue, model_id, region_id, promptTemplate=None): 

    model_arn = f'arn:aws:bedrock:{region_id}::foundation-model/{model_id}' 

    return bedrock_agent_runtime.retrieve_and_generate( 

            input={ 'text': input }, 

            retrieveAndGenerateConfiguration={ 

                'knowledgeBaseConfiguration': {

                    'generationConfiguration': {

                        'promptTemplate': {

                            'textPromptTemplate': promptTemplate 

                            } 

                        }, 

                    'knowledgeBaseId': kbId, 

                    'modelArn': model_arn, 

                    'retrievalConfiguration': {

                        'vectorSearchConfiguration': {

                            'filter': { 'equals': { 'key': 'year', 'value': yearValue } }, 

                            'numberOfResults': 6, 

                            'overrideSearchType': 'HYBRID'

                        }

                    }

                },

                'type': 'KNOWLEDGE_BASE' 

            }

        )
```
Next, ask the same question as in the console example: Who is the CEO of Amazon? In the code snippet example, it will filter on the year 2020. Because we limit the knowledge base information to 2020, the FM will respond in the generated_text variable with the name Jeffrey Bezos in the output format specified by the prompt template.
```python
query = "Who is Amazon's CEO?" 

response = retrieveAndGenerateAndFilter(

        query, 

        kb_id,

        model_id=model_id,

        region_id=region_id,

        promptTemplate=promptTemplate,

        yearValue=2020

    ) 

generated_text = response['output']['text'] 

pp.pprint(generated_text)

Print response: '{key: Jeffrey P. Bezos}'
```
Change the filter year to 2023 and ask the same question. The expected answer is Andrew Jassy.
The RetrieveAndGenerate response also contains citations from the underlying documents. To retrieve citations from the response, you can implement the following code snippet. Note that the response has been abbreviated to declutter the code image.
```python
citations = response["citations"]

contexts = [] 

for citation in citations: 

    retrievedReferences = citation["retrievedReferences"] 

    for reference in retrievedReferences: 

        contexts.append(reference["content"]["text"])

pp.pprint(contexts)


Print response: [ 'Jassy   President and Chief Executive Officer (Principal Executive '

  'Officer)   Date: February 1, 2024        Exhibit 32.2   Certification '

  'Pursuant to 18 U.S.C. Section 1350   In connection with the Annual Report '

  'of Amazon.com, Inc. (the “Company”)  ...' ]
```






  

