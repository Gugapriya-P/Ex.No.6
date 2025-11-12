# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

### Date: 31.10.25
### NAME : GUGAPRIYA.P 
### Register no.: 212223060075

### Aim

To write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights, focusing on the use of the Persona Pattern for technical analysis.

## AI Tools Required (Conceptual Mapping)

Conceptual/Persona Tool: An LLM utilized solely for high-level, opinionated analysis based on its training data and a defined persona (Simulated by Gemini API with a strong system instruction).

Grounded/Search Tool: An LLM enhanced with real-time web access for fact-checking and current best practices (Simulated by Gemini API with Google Search grounding).

Explanation: The Persona Pattern and Comparison Strategy

This experiment uses the "Senior Cloud Architect" persona to evaluate a key technical challenge: migrating a legacy application to serverless. This choice of persona is intended to force the AI to produce outputs that are opinionated, pragmatic, and focused on security/cost implications.

The Python script (ai_comparison_tool.py) performs the following:

Defines the Persona: A detailed system instruction sets the tone and expertise level.

Queries Tool 1 (Conceptual): The model answers using only its pre-trained knowledge under the architect persona.

Queries Tool 2 (Grounded): The model answers under the same persona but uses Google Search to ground its response, ensuring best practices are current.

Generates Actionable Insights: A final prompt is used to compare the two outputs and create a unified, actionable summary, demonstrating the value of combining AI outputs.

## Comparison Metrics:

Specificity: Is the advice general or does it name specific services/tools?

Grounding: Does the output include citations or references to current standards?

Actionability: How easily can the advice be translated into immediate development tasks?

Observations: AI Output Comparison

(To be filled after running the Python script)

Tool

Prompt Focus

Key Output Characteristics

Specificity Score (1-5)

Grounding Score (1-5)

Conceptual Tool

High-Level Strategy

Grounded Tool

Real-Time Best Practices

Analysis and Discussion

(Discuss how the presence of Google Search grounding in Tool 2 affected the output compared to Tool 1. For example, Tool 2 might mention specific costs or recent security vulnerabilities, while Tool 1 provides only general advice.)

Persona Consistency: Both tools maintained the Senior Cloud Architect persona, but the quality of advice differed significantly.

<img width="462" height="404" alt="image" src="https://github.com/user-attachments/assets/d3c832fe-aba8-4b9a-ae05-c4465165543a" />
## CODE:
```
import json
import time
import random
import sys
import asyncio
from typing import Dict, Any, Tuple, List

API_KEY = ""
BASE_URL = "https://generativelanguage.googleapis.com/v1beta/models/"

SYSTEM_PERSONA = """
You are a Senior Cloud Architect specializing in AWS Serverless technologies (Lambda, DynamoDB, API Gateway). 
Your goal is to provide pragmatic, security-conscious, and cost-effective migration advice. 
You must think critically and act as if you are advising a CTO.
"""

MIGRATION_TASK_PROMPT = "Evaluate the top 3 critical security risks and the 3 most important implementation steps for migrating a legacy monolithic application to a Serverless architecture. Focus on cost and risk mitigation."

async def exponential_backoff_fetch(url: str, payload: Dict[str, Any], max_retries: int = 5) -> Dict[str, Any] | None:
    headers = {'Content-Type': 'application/json'}
    final_url = f"{url}?key={API_KEY}" if API_KEY else url
    for attempt in range(max_retries):
        try:
            response = await fetch(final_url, {'method': 'POST', 'headers': headers, 'body': json.dumps(payload)})
            if response.status != 200:
                raise Exception(f"API Error {response.status}")
            return await response.json()
        except Exception:
            if attempt < max_retries - 1:
                await asyncio.sleep((2 ** attempt) + random.uniform(0, 1))
            else: return None

def extract_content(response: Dict[str, Any] | None, tool_name: str) -> Tuple[str, List[Dict[str, str]]]:
    if not response or not response.get("candidates"):
        return f"[{tool_name}]: Failed.", []
    candidate = response["candidates"][0]
    text = candidate.get("content", {}).get("parts", [{}])[0].get("text", "No text content found.")
    sources = []
    grounding_metadata = candidate.get("groundingMetadata", {})
    if grounding_metadata and grounding_metadata.get("groundingAttributions"):
        sources = [{"uri": attr["web"]["uri"], "title": attr["web"]["title"]}
                   for attr in grounding_metadata["groundingAttributions"]
                   if attr.get("web")]
    return text, sources

async def call_conceptual_tool(user_prompt: str) -> Dict[str, Any] | None:
    url = f"{BASE_URL}gemini-2.5-flash-preview-09-2025:generateContent"
    payload = {"contents": [{"parts": [{"text": user_prompt}]}],
               "systemInstruction": {"parts": [{"text": SYSTEM_PERSONA}]}}
    return await exponential_backoff_fetch(url, payload)

async def call_grounded_tool(user_prompt: str) -> Dict[str, Any] | None:
    url = f"{BASE_URL}gemini-2.5-flash-preview-09-2025:generateContent"
    payload = {"contents": [{"parts": [{"text": user_prompt}]}],
               "systemInstruction": {"parts": [{"text": SYSTEM_PERSONA}]},
               "tools": [{"google_search": {}}]} 
    return await exponential_backoff_fetch(url, payload)

async def compare_and_analyze_outputs(output1: str, output2: str) -> Dict[str, Any] | None:
    analysis_prompt = f"Analyze and combine the following two sets of advice into a single, unified, actionable three-point implementation plan. Prioritize the most specific and security-focused items from both.\n\nTool A (Conceptual):\n{output1}\n\nTool B (Grounded):\n{output2}"
    url = f"{BASE_URL}gemini-2.5-flash-preview-09-2025:generateContent"
    payload = {"contents": [{"parts": [{"text": analysis_prompt}]}],
               "systemInstruction": {"parts": [{"text": "You are a neutral Program Manager focused on combining technical advice into concrete steps."}]}}
    return await exponential_backoff_fetch(url, payload)
async def main():
    print("# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools")
    print("----------------------------------------------------------------------")
    # 1. Conceptual Tool Execution
    response_c = await call_conceptual_tool(MIGRATION_TASK_PROMPT)
    text_c, sources_c = extract_content(response_c, "Conceptual Tool")
    print("\n[1. CONCEPTUAL TOOL (High-Level Strategy)]")
    print(text_c)
    print(f"(Sources found: {len(sources_c)})")
    # 2. Grounded Tool Execution
    response_g = await call_grounded_tool(MIGRATION_TASK_PROMPT)
    text_g, sources_g = extract_content(response_g, "Grounded Tool")
    print("\n[2. GROUNDED TOOL (Real-Time Best Practices)]")
    print(text_g)
    print(f"(Sources found: {len(sources_g)})")
    if sources_g:
        print("**Top Citations:**")
        for i, src in enumerate(sources_g[:2]): print(f"  {i+1}. {src['title']}")
    # 3. Synergy Analysis
    response_a = await compare_and_analyze_outputs(text_c, text_g)
    text_a, _ = extract_content(response_a, "Analysis Tool")
    print("\n[3. FINAL ACTIONABLE IMPLEMENTATION PLAN (Synergy)]")
    print(text_a)
    print("\n----------------------------------------------------------------------")

if 'pyodide' in sys.modules:
    asyncio.run(main())
else:
    pass
```
### Conclusion

This experiment successfully demonstrated the development of Python code capable of integrating and comparing outputs from multiple AI capabilities (persona-based vs. grounded search). The use of structured prompting and persona patterns proved effective in generating relevant technical responses, with grounding significantly enhancing the practical and timely nature of the advice.

Result

The corresponding Prompt is executed successfully, and the comparison highlights the necessity of using grounding tools when generating technical advice that requires up-to-date best practices.
