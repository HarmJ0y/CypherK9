# Repository: Text-to-Cypher LLM with LLM Augmentation

## Overview

This repository contains code for generating Cypher queries from the user's natural language questions. The code leverages one pre-trained and one fine-tuned language model (LLM) for two main purposes:
1. **LLM Augmentation**: Enhancing the user query with additional context based on the graph schema.
2. **Cypher Query Generation**: Converting the enhanced query into a Cypher query using a fine-tuned model.

## How It Works

### 1. User Input Parsing
The script begins by taking user inputs through command-line arguments. These inputs include the user's question, the graph database name, the models to be used for augmentation and Cypher query generation, and database connection details.

### 2. Graph Schema Template Generation
The script connects to a Neo4j graph database to retrieve the schema structure (It can be replaced by other graph databases). This schema is then processed into a structured template using the `generate_graph_schema_template` function. The template will later be used to help the LLM understand the context of the graph. 

### 3. LLM Augmentation
Using a pre-trained language model (specified by the user), the script generates an augmented version of the user's query. This augmentation enriches the query with information derived from the graph schema, making it more contextually aware. 

### 4. Cypher Query Generation
The augmented query is then passed to a Cypher-specific model, which has been fine-tuned to generate Cypher queries. The `generate_cypher` function handles the conversion of the augmented query into a Cypher query.

### 5. GPU Memory Management
If the script is running on a laptop (as specified by the user), it clears the GPU memory after the LLM augmentation step to free up resources for the Cypher model. The `clear_model_memory` and `print_gpu_utilization` functions are used for this purpose.

### 6. Cypher Query Output
Finally, the generated Cypher query is printed as the output.

## Major Components

- **Huggingface Hub Integration**: The script logs into the Huggingface Hub to access the specified models.
- **Neo4j Schema Extraction**: The `Neo4jSchema` class is used to connect to the Neo4j database and extract the schema. (This can be exchanged for specific needs)
- **Model Handling**: The script handles two models: one for LLM augmentation and another fine-tuned for Cypher query generation. The `BitsAndBytesConfig` is used to optimize the models for efficient GPU usage.
- **Pipeline Construction**: A text generation pipeline is constructed using the specified LLM, allowing for efficient and scalable query augmentation.

## Running the Code

To run the script, use the following command:

```bash
python script_name.py --query "Your question here" --graph "graph_name" --augmentModel "model_name" --cypherModel "model_name" --graphURL "neo4j://your-graph-url" --graphUser "username" --graphPWD "password" --running_on_laptop True/False
```

Replace the placeholders with your actual data. The script will output the generated Cypher query.

## Dependencies
Refer to the requirements file for all the dependencies. Major dependencies are,

- Python 3.8+
- `transformers`
- `torch`
- `huggingface_hub`
- `unsloth`
- `GPUtil`
- `neo4j`

Ensure all dependencies are installed before running the script.

## API 
The tool can be hosted behind an API call using LiteLLM and FastAPI. A working prototype can be found in the [API folder](https://github.com/gneeraj97/CypherK9/tree/main/api). 

## License

This project is licensed under the MIT License. See the LICENSE file for more details.
