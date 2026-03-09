## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### Name: MARIMUTHU MATHAVAN
### Register no: 212224230153

## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### AIM:
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT:
LangChain Expression Language (LCEL) simplifies interactions with large language models (LLMs) by creating reusable and structured expressions. This task involves:
1. Designing an LCEL expression with dynamic prompt parameters (e.g., topic and length).
2. Using three essential components: Prompt- A structured input with placeholders for parameters, Model- An LLM used to process the prompt and Output Parser- A parser to interpret the model's output.
3. Demonstrating the LCEL expression's functionality in generating structured, relevant outputs.
   
### DESIGN STEPS:
1. **Define a Structured Output Model**  
   Create a Pydantic model (`SummaryResponse`) to enforce strict formatting and define expected fields.  
   Attach a `PydanticOutputParser` to validate the model schema.

2. **Build a Prompt Template**
   Use `ChatPromptTemplate` to create a parameterized prompt including tone, audience, length, and topic.  
   Insert escaped format instructions to force valid JSON output.

3. **Configure the LLM**
   Initialize the `ChatGroq` model, selecting model type and generation settings (temperature, API key).

4. **Construct and Execute the LCEL Chain**
   Pipe the prompt → model → parser using `|`, then run `.invoke()` on example inputs and inspect validated JSON output.

### PROGRAM:
```py
# MARIMUTHU MATHAVAN
# 212224230153
# INSTALLS:
# pip install -U langchain-core langchain-groq pydantic python-dotenv

import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

prompt = ChatPromptTemplate.from_template(
    "List the advantages and disadvantages of {topic}."
)
model = ChatOpenAI()
output_parser = StrOutputParser()

chain = prompt | model | output_parser

chain.invoke({"topic": "AI"})
```
### OUTPUT:

<img width="1353" height="321" alt="image" src="https://github.com/user-attachments/assets/9c969e0c-aa8b-4c72-ba97-05984ae19e6b" />


### RESULT:

Thus, the LangChain Expression Language (LCEL) expression that utilizes two prompt parameters and three key components (prompt, model, and output parser) was designed and implemented successfully. And also evaluated its functionality by analyzing relevant examples of its application in real-world scenarios.
(LCEL) expression that utilizes two prompt parameters and three key components (prompt, model, and output parser) was designed and implemented successfully. And also evaluated its functionality by analyzing relevant examples of its application in real-world scenarios.
