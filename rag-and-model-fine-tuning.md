**RAG(Retrieval-Augmented Generation):**
RAG is a technique for building generative AI applications that makes use of enterprise data sources and vector databases to overcome knowledge limitations. 
RAG works by using a retriever module to find relevant information from an external data store in response to a user's prompt. This retrieved data is used as context, 
combined with the original prompt, to create an expanded prompt that is passed to the language model. The language model then generates a response that incorporates the enterprise knowledge.

With RAG, language models can go beyond their original training data to use up-to-date, real-world information. 
RAG addresses the challenge of frequent data changes because it retrieves updated and relevant information instead of relying on potentially outdated data.
**Limitations of RAG and how fine-tuning can address them**

RAG is useful for enterprise use cases, but relying solely on RAG has some limitations. For example, the retrieval is limited to the enterprise datasets that are embedded into the vector stores at the time of the retrieval. The model remains static. With large context windows, LLM latency can be a problem.

Model fine-tuning can change the underlying FM as little or as much as you want. The model can learn to perform better on a specific task with the help of a labeled dataset. Think of this as a permanent change to the underlying model. By comparison, RAG makes the model intelligent only temporarily by supplying context from relevant document chunks.

There are two broad categories of fine-tuning: customization (also known as prompt-based learning) and continued pretraining (also called domain adaptation).
**prompt-based learning**
Fine-tuning the underlying FM for a specific task is accomplished through prompt-based learning. This involves pointing the model toward a labeled dataset of examples that you want the model to learn from. The labeled examples are formatted as prompt and response pairs, and phrased as instructions. The prompt-based fine-tuning process modifies the weights of the model. It is usually lightweight and involves a few epochs to tune. 
Because the fine-tuning is specific to one particular task, it can’t be generalized across multiple tasks.
<img width="677" height="426" alt="image" src="https://github.com/user-attachments/assets/5f4f81a6-4962-4b8e-b05f-c8c7235e1137" />

**domain adaptation**
With domain adaptation fine-tuning, you can use pretrained FMs and adapt them to multiple tasks using limited domain-specific data. You can fine-tune the FM with as little or as much of your domain-specific, unlabeled data. Depending on the amount of data used for fine-tuning, 
it will use the language of your enterprise such as the technical terms specific to your domain.To perform the fine-tuning, you need a ML environment that can handle the complete process of fine-tuning.
 It also needs access to the appropriate compute instances for fine-tuning.
<img width="715" height="407" alt="image" src="https://github.com/user-attachments/assets/53a0ecb4-8112-4f59-bdf3-96786dc43600" />
<img width="962" height="630" alt="image" src="https://github.com/user-attachments/assets/354f563b-2be0-4b38-90a0-06812702e691" />





