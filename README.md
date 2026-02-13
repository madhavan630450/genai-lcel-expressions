## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### Name: MARIMUTHU MATHAVAN
### Register no: 212224230153

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
# INSTALLS:
# pip install -U langchain-core langchain-groq pydantic python-dotenv

from pydantic import BaseModel, Field
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import PydanticOutputParser
from langchain_groq import ChatGroq
import os

os.environ["GROQ_API_KEY"] = "groq_api"

# 1. STRUCTURED MODEL
class SummaryResponse(BaseModel):
    summary: str
    word_count: int
    highlights: list[str]

parser = PydanticOutputParser(pydantic_object=SummaryResponse)

# ESCAPE THE FORMAT INSTRUCTIONS
format_rules = parser.get_format_instructions().replace("{", "{{").replace("}", "}}")

# 2. PROMPT
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a concise factual summarizer."),
        ("human",
         "Write a {length}-word {tone} summary about: {topic}.\n"
         "Provide exactly 3 short highlights.\n\n"
         "Output MUST be valid JSON using this schema:\n"
         f"{format_rules}\n\n"
         "Audience: {audience}"
        ),
    ]
)


# 3. MODEL (GROQ)
llm = ChatGroq(
    model="openai/gpt-oss-20b",
    temperature=0
)

# 4. LCEL CHAIN
chain = prompt | llm | parser

# 5. TEST EXAMPLES
examples = [
    {"topic": "Climate change causes", "length": "50", "tone": "neutral", "audience": "general readers"},
    {"topic": "Transformer neural networks", "length": "40", "tone": "technical", "audience": "ML engineers"},
]

for ex in examples:
    print("\n=== INPUT ===")
    print(ex)

    result = chain.invoke(ex)
    print("\n=== PARSED JSON OUTPUT ===")
    print(result.model_dump())

    real_wc = len(result.summary.split())
    print("Reported:", result.word_count)
    print("Actual:", real_wc)
```
### OUTPUT:

![alt Image](https://github.com/user-attachments/assets/7811609e-857a-4330-9628-1fde46d6fe7d)

### RESULT:

Thus, the LangChain Expression Language (LCEL) expression that utilizes two prompt parameters and three key components (prompt, model, and output parser) was designed and implemented successfully. And also evaluated its functionality by analyzing relevant examples of its application in real-world scenarios.
