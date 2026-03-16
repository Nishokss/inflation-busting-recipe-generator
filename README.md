# Inflation-Busting Recipe Generator

AI-powered Streamlit app that helps UK users find budget-friendly meal ideas using discounted supermarket products. The app pulls product data from Open Food Facts, stores offers in Supabase, and generates tailored recipes with Groq LLM.

## Features

- Budget-aware recipe recommendations from discounted ingredients
- Diet filtering (Vegetarian / Non-Vegetarian)
- Multi-step guided chat flow (budget, meal type, servings, time, restrictions)
- Price comparison from available discounted products
- User authentication with Supabase Auth
- Save and reload recipe/chat sessions per user
- PDF export for generated recipes
- Auto-seeding of Supabase with UK product offers

## Tech Stack

- Python 3.10+
- Streamlit
- Supabase (database + auth)
- LangChain + Groq (`llama-3.1-8b-instant`)
- Open Food Facts API
- fpdf2

## Project Structure

```text
.
├── app.py                          # Main Streamlit UI + session flow
├── run.py                          # Startup helper: env check + seed + launch
├── chatbot.py                      # LLM orchestration + recommendation logic
├── auth.py                         # Supabase auth + onboarding/session cookie logic
├── config.py                       # Env loading, Supabase clients, LLM factory
├── data_ingestion/
│   ├── fetch_open_food_facts.py    # Fetch UK products + generate discount rows
│   └── load_to_supabase.py         # Normalize and upsert stores/products/offers
├── db/schema.sql                   # Supabase tables, policies, and view
├── prompts/recipe_prompt.md        # Core recipe prompt template
└── utils/pdf_utils.py              # Recipe markdown → PDF bytes
```

## Prerequisites

- Python 3.10 or newer
- A Supabase project
- A Groq API key

## Environment Variables

Create a `.env` file in the project root:

```env
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_key
GROQ_API_KEY=your_groq_api_key

# Optional: enables admin-style DB operations where needed
SUPABASE_SERVICE_KEY=your_supabase_service_role_key
```

## Setup

1. Clone the repository and enter the project directory.
2. Create a virtual environment.
3. Install dependencies.
4. Add your `.env` file.
5. Apply database schema in Supabase SQL editor.

### Windows (PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Database Setup (Supabase)

Run the SQL from `db/schema.sql` in your Supabase SQL Editor.

This creates:
- `stores`, `products`, `offers`
- `chat_sessions` (with RLS policies)
- `user_profiles` (with RLS policies)
- `user_login_details` view

## Run the App

### Recommended

```bash
python run.py
```

`run.py` will:
1. Validate required `.env` keys
2. Seed offers from Open Food Facts if `offers` is empty
3. Launch Streamlit on `http://localhost:8501`

### Alternative

```bash
streamlit run app.py
```



## Notes

- Keep `.env` out of version control.
- If push protection blocks commits, rotate exposed keys immediately.
- App has fallback behavior when optional components are unavailable (e.g., PDF utility import fallback).

## Troubleshooting

- **Missing env keys**: ensure `SUPABASE_URL`, `SUPABASE_ANON_KEY`, and `GROQ_API_KEY` are set.
- **No offers found**: verify internet access and Open Food Facts availability; app can use cached Supabase data if present.
- **Auth/session issues**: confirm Supabase Auth is enabled and schema policies were applied.
- **Dependency errors**: reinstall with `pip install -r requirements.txt`.

## License

No license file is currently included in this repository.
