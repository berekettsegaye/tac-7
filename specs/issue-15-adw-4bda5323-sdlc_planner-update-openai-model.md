# Chore: Update OpenAI model to o4-mini

## Metadata
issue_number: `15`
adw_id: `4bda5323`
issue_json: `{"number":15,"title":"Support better models for query generation.","body":"chore - adw_sdlc_iso\nupdate openai model to use o4-mini (o4-mini-2025-04-16)"}`

## Chore Description
Update the OpenAI model configuration from `gpt-4.1-2025-04-14` to `o4-mini-2025-04-16` for improved query generation capabilities. This chore involves updating the model identifier in the LLM processor module and corresponding test assertions to use the new o4-mini model.

## Relevant Files
Use these files to resolve the chore:

- `app/server/core/llm_processor.py` - Contains the OpenAI API integration functions that specify the model to use for SQL query generation and random query generation. Lines 44 and 184 contain the current model configuration (`gpt-4.1-2025-04-14`) that needs to be updated to `o4-mini-2025-04-16`.

- `app/server/tests/core/test_llm_processor.py` - Contains test cases that verify the correct model is being used in API calls. Line 44 contains a test assertion that checks the model name matches `gpt-4.1-2025-04-14` and needs to be updated to verify `o4-mini-2025-04-16` instead.

- `app_docs/feature-4c768184-model-upgrades.md` - Documentation file that describes previous model upgrades. Should be referenced to understand the pattern of model updates, but does not need to be modified as this is a separate chore with its own documentation trail.

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### Step 1: Update OpenAI model in SQL generation function
- Open `app/server/core/llm_processor.py`
- Locate the `generate_sql_with_openai` function (around line 7-66)
- Update line 44 from `model="gpt-4.1-2025-04-14"` to `model="o4-mini-2025-04-16"`
- Verify the change is correct and the function signature remains unchanged

### Step 2: Update OpenAI model in random query generation function
- In the same file `app/server/core/llm_processor.py`
- Locate the `generate_random_query_with_openai` function (around line 146-197)
- Update line 184 from `model="gpt-4.1-2025-04-14"` to `model="o4-mini-2025-04-16"`
- Verify the change is correct and the function signature remains unchanged

### Step 3: Update test assertions for model verification
- Open `app/server/tests/core/test_llm_processor.py`
- Locate the `test_generate_sql_with_openai_success` test method (around line 16-46)
- Update line 44 from `assert call_args[1]['model'] == 'gpt-4.1-2025-04-14'` to `assert call_args[1]['model'] == 'o4-mini-2025-04-16'`
- Verify the assertion correctly validates the new model name

### Step 4: Run validation commands
- Execute the validation commands listed below to ensure the chore is complete with zero regressions
- All tests must pass without errors
- Verify the application still functions correctly with the new model

## Validation Commands
Execute every command to validate the chore is complete with zero regressions.

- `cd app/server && uv run pytest` - Run server tests to validate the chore is complete with zero regressions
- `cd app/server && uv run pytest tests/core/test_llm_processor.py -v` - Run specific LLM processor tests to verify model updates
- `cd app/server && python -c "from core.llm_processor import generate_sql_with_openai, generate_random_query_with_openai; print('Model configuration updated successfully')"` - Verify the module imports correctly

## Notes
- This chore updates the OpenAI model from GPT-4.1 to o4-mini, which represents a shift to a more efficient reasoning model
- The o4-mini model (released April 2025) provides improved query generation capabilities while being more cost-effective than GPT-4.1
- The Anthropic model configuration (claude-sonnet-4-0) remains unchanged in this chore
- All function signatures and API interfaces remain unchanged, ensuring backward compatibility
- The model string follows OpenAI's versioning convention: `{model-name}-{release-date}`
- Only three specific locations need updates: two in the llm_processor.py module and one test assertion
