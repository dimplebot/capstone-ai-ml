**Part 4 - LLM-Powered Feature: Model Prediction Explanation Pipeline**

**Chosen Track - Track C** - Model prediction explanation pipeline

In this part, the best performing machine learning model developed in part 3 is combined with large language model LLM to generate explanations for model predictions. The Random forest pipeline (best_model.pkl) is loaded, predictions are generated for three hand-crafted feature vectors and the LLM produced JSON-based explanations for each prediction. The responses are validated against a predefined JSON schema before being accepted.

**LLM API** - The OpenRouter API was used to communicate with the LLM through Python's requests library.

The API key is stored securely in an environment variable (LLM_API_KEY) using a .env file and loaded with python-dotenv. The API key was never hardcoded in the notebook. The reusable function named call_llm() was implemented to construct the JSON request payload, send an HTTP POST request, check the response status, return the model response. This function has also been tested using simple prompt.

The API key loaded is intentionally not included in this repository for security reasons.

System Prompt: You are a helpful assistant
User Prompt: Reply with only the work: hello
hello


**SYSTEM PROMPT**:
The following system prompt is used for the prediction explanation task.

system_prompt = """
You are an AI assistant that explains machine learning predictions.

Return ONLY valid JSON.

Do not include markdown.
Do not include code blocks.
Do not include explanations.
Do not include any text before or after the JSON.

The JSON must contain exactly these fields:

{
  "prediction_label": "Low Risk or High Risk",
  "confidence_level": "Low, Medium, or High",
  "top_reason": "Main reason for the prediction",
  "second_reason": "Second most important reason",
  "next_step": "Recommended action"
}
"""

**USER PROMPT**:
Feature Values:
{feature_values}

Predicted Class:
{predicted_class}

Prediction Probability:
{prediction_probability}

Generate a JSON explanation according to the required schema.

The LLM is called using **temperature=0** because this task requires structured and reproducible JSON output. A temperature of 0 always selects the highest-probability next token, producing consistent and deterministic responses. This makes the output suitable for JSON parsing, schema validation, and automated processing.

```text
| Input | Output (Temperature = 0) | Output (Temperature = 0.7) | Key Difference |
|-------|---------------------------|----------------------------|----------------|
| Sample 1 | Low Risk explanation focused on healthy BMI and non-smoking status. | Low Risk explanation included more details about exercise and healthy lifestyle. | Temperature 0.7 produced a more descriptive explanation while keeping the same prediction. |
| Sample 2 | High Risk explanation focused on smoking, high BMI, and blood pressure. | High Risk explanation also highlighted the pre-existing condition and used slightly different wording. | Temperature 0.7 generated a more varied explanation while maintaining the same prediction. |
| Sample 3 | Low Risk explanation focused on non-smoking and manageable health indicators. | Low Risk explanation included additional health recommendations and more detailed wording. | Temperature 0.7 produced a longer explanation while keeping the same prediction. |
```

The model was tested using both temperature = 0 and temperature = 0.7. With temperature = 0 the responses were consistent and deterministic, making them suitable for structured JSON generation. With temperature = 0.7 the prediction labels remained the same, but the wording, supporting reasons, and recommendations varied slightly. This happens because temperature = 0 always selects the highest-probability next token, while temperature = 0.7 samples from a wider range of possible tokens, resulting in more varied responses. Therefore, **temperature = 0** is selected for the final implementation.

A JSON schema was created to validate every LLM response. The schema required the following five fields: prediction_label, confidence_level, top_reason, second_reason, next_step.

Each LLM response was:

1. Parsed using `json.loads()`.
2. Validated using `jsonschema.validate()`.
3. Checked for schema compliance.

If validation failed, a fallback response would be returned. During testing all three LLM responses passed schema validation successfully.

The saved Random Forest model (best_model.pkl) is loaded successfully using joblib.load(). Three handcrafted feature vectors are passed to the model to obtain predicted classes and prediction probabilities. These predictions are then provided to the LLM, which generated structured JSON explanations. Every response was successfully validated against the predefined JSON schema.

```text
| Feature Input | Predicted Class | Probability | Explanation JSON | Validation Status |
|---------------|----------------:|------------:|------------------|-------------------|
| Sample 1 (Age = 25, BMI = 22.5, Non-smoker) | Low Risk | 0.880 | Low Risk explanation highlighting healthy BMI, non-smoking status, and healthy lifestyle. | PASS |
| Sample 2 (Age = 55, BMI = 34.2, Smoker) | High Risk | 0.955 | High Risk explanation highlighting smoking, high BMI, and elevated blood pressure. | PASS |
| Sample 3 (Age = 40, BMI = 29.4, Non-smoker) | Low Risk | 0.905 | Low Risk explanation highlighting non-smoking status and manageable health indicators. | PASS |
```

**PII Gaurdrail**:

Before every LLM call, the input was checked using regular expressions to detect personally identifiable information (PII), including email addresses and phone numbers. If PII was detected, the LLM call was blocked to protect user privacy.

```text
| Test Input | PII Detected | Guardrail Result |
|------------|--------------|------------------|
| Input containing an email address (meena@gmail.com) | Yes | Blocked |
| Input without email or phone number | No | Allowed |
```

A PII guardrail is implemented before every LLM request to protect sensitive information. During testing an input containing an email address is correctly blocked, while a normal input without personal information is successfully processed by the LLM. This helps prevent accidental sharing of sensitive user data.


The best-performing Random forest model from Part 3 is successfully integrated with an LLM to generate human-readable prediction explanations. The complete pipeline included model prediction, structured prompting, JSON schema validation, temperature comparison, and PII protection. All three prediction explanations were returned as valid JSON and successfully passed schema validation, demonstrating a reliable and reproducible LLM-powered explanation system.