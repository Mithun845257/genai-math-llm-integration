## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the speed of moving object, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Develop a Python-based application that calculates the speed of a moving object using the distance traveled and time taken. Integrate this calculation with a Chat Completion System using the Function Calling feature of a Large Language Model (LLM), enabling the model to automatically invoke the speed calculation function and return accurate results based on user input.


### DESIGN STEPS:

#### STEP 1:
Import the required Python libraries and configure the OpenAI API.


#### STEP 2:
Create a Python function to calculate the speed using distance and time.


#### STEP 3:
Define the function schema for LLM Function Calling using JSON Schema.

#### STEP 4:
Send the user's query to the Chat Completion API and allow the LLM to invoke the function.

####STEP 5:
Execute the function and display the calculated speed as the final response.



### PROGRAM:
Developed By : Mithun G
Reg no : 212225040235
```
import os
import json
import openai
from dotenv import load_dotenv

# Load API key
load_dotenv()
openai.api_key = os.getenv("OPENAI_API_KEY")

# Function to calculate speed
def calculate_speed(distance, time):
    speed = float(distance) / float(time)
    return json.dumps({"speed": round(speed, 2)})

# Function schema
functions = [
    {
        "name": "calculate_speed",
        "description": "Calculate speed using distance and time.",
        "parameters": {
            "type": "object",
            "properties": {
                "distance": {
                    "type": "string",
                    "description": "Distance travelled in kilometers"
                },
                "time": {
                    "type": "string",
                    "description": "Time taken in hours"
                }
            },
            "required": ["distance", "time"]
        }
    }
]

# User prompt
messages = [
    {
        "role": "user",
        "content": "A car travels 180 km in 3 hours. What is its speed?"
    }
]

# Step 1: Ask the LLM
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="auto"
)

# Step 2: Extract function arguments
args = json.loads(
    response["choices"][0]["message"]["function_call"]["arguments"]
)

# Step 3: Execute the function
result = calculate_speed(args["distance"], args["time"])

# Step 4: Add function call and result to the conversation
messages.append(response["choices"][0]["message"])
messages.append({
    "role": "function",
    "name": "calculate_speed",
    "content": result
})

# Step 5: Get the final response
final_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages
)

print(final_response["choices"][0]["message"]["content"])
```

### OUTPUT:
<img width="314" height="42" alt="image" src="https://github.com/user-attachments/assets/2739cc5f-cbc5-4f6c-b0a0-865837669d11" />

### RESULT:
The speed of the moving object was calculated successfully using LLM Function Calling.

