Enterprises accumulate huge volumes of internal data, such as documents, presentations, user manuals, reports, and transaction summaries. You can supply the enterprise data to the FM as context along with the prompt. This will help the model to return more accurate outputs tailored to the enterprise. However, if you provide too much context, your LLM can get confused, resulting in LLM hallucinations, longer prompts and higher costs. To avoid hallucinations, while keeping latency and costs low, you need to pass the right amount of content along with the prompt.

There are two ways to supply the context. If you want to query the content of a particular document, you can supply that document as context. However, if you want to select one or more documents based on the query from an enterprise data store, you need an efficient way to search the enterprise datasets using the input prompt text. Vector embeddings is a mechanism used to find data in the enterprise dataset that is similar to the prompt information.

**Vector embeddings**

Embedding is the process by which text, images, and audio are given numerical representation in a vector space. There are multiple Embedding FMs available that generate vector embeddings .

Enterprise datasets, such as documents, images and audio, are passed to an embedding model as tokens and are vectorized. These vectors , along with the vector
metadata , are indexed and stored in a purpose-built vector database for fast retrieval. The following diagram provides more details about embeddings.
<img width="971" height="702" alt="image" src="https://github.com/user-attachments/assets/6cc72408-1972-41e6-9399-3f3dea93ffeb" />
For example, consider text modality. The goal of generating embeddings is to capture semantic similarities between text. This means that text with similar meanings is mapped to nearby points in the vector space. Embedding helps to find relevant information based on the user’s prompts.

Amazon Bedrock provides embeddings models such as:
    Amazon Titan Embeddings G1: Text model that can convert text into embeddings,
    Cohere Embed: English and multi-modal - text embedding models for english and multiple languages
    Titan multi-modal embedding: multi-modal embedding for images and text.
**Vector databases **
The core function of vector databases is to compactly store billions of high-dimensional vectors representing words and entities. Vector databases provide efficient similarity searches across billions of vectors in real time.
The most common algorithms used to perform the similarity search are k-nearest neighbors (k-NN) or cosine similarity.
**Amazon Web Services (AWS) offers the following as viable vector database options.**
    Amazon OpenSearch Service (provisioned)
    Amazon OpenSearch Serverless
    pgvector extension in Amazon Relational Database Service (Amazon RDS) for PostgreSQL
    pgvector extension in Amazon Aurora PostgreSQL - Compatible Edition

**Prompt history store**

An AI assistant cannot support multi-turn conversations without access to prompt history. A prompt history store enables contextually aware conversations that are both relevant and coherent. Many FMs have a limited context window, which means you can only pass a fixed length of data as input. Storing state information in a multiple-turn conversation becomes a problem when the conversation exceeds the context window. To solve this problem, 
you can implement a prompt history store. It can persist the conversation state, making it possible to have a long-term history of the conversation.
By storing the prompts and responses history, you can look up prompts from a previous conversation to avoid repetitive requests to the FM.

The prompt history store serves two purposes. It provides data for the audit and compliance teams to ensure adherence to company policies and government regulations.
 An application developer can also use the prompt history store to debug application errors and warnings.
<img width="806" height="407" alt="image" src="https://github.com/user-attachments/assets/96d15f3e-a08b-4bf1-ad14-38f4bc33dbce" />

