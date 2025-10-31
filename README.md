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

### Conclusion

This experiment successfully demonstrated the development of Python code capable of integrating and comparing outputs from multiple AI capabilities (persona-based vs. grounded search). The use of structured prompting and persona patterns proved effective in generating relevant technical responses, with grounding significantly enhancing the practical and timely nature of the advice.

Result

The corresponding Prompt is executed successfully, and the comparison highlights the necessity of using grounding tools when generating technical advice that requires up-to-date best practices.
