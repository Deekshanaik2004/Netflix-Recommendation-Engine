# TMDBMovie — Movie Recommender

A local movie recommender / search app using TMDB API + local TF-IDF similarity data.

- `app.py`: Streamlit frontend (UI + calls backend API)
- `main.py`: FastAPI backend (TMDB API passthrough + TF-IDF recommendations)
- `df.pkl`, `indices.pkl`, `tfidf_matrix.pkl`, `tfidf.pkl`: local recommendation dataset (TF-IDF model)

## Features

- Home feed (TMDB categories: trending, popular, top_rated, now_playing, upcoming)
- Keyword search (`/tmdb/search` for suggestions, plus fallback logic)
- Movie details (`/movie/id/{tmdb_id}`)
- Genre-based recommendations (`/recommend/genre`)
- TF-IDF local recommendations (`/recommend/tfidf` and `/movie/search` bundle)

## Prerequisites

- Python 3.11+ (recommended)
- Virtual env
- TMDB API key

## Setup

1. Create venv:
   ```powershell
   python -m venv venv
   .\venv\Scripts\Activate.ps1
   ```

2. Install dependencies:
   ```powershell
   pip install -r requirements.txt
   ```

3. Create `.env` in project root:
   ```text
   TMDB_API_KEY=your_tmdb_api_key_here
   ```

4. Verify pickle files exist:
   - `df.pkl`
   - `indices.pkl`
   - `tfidf_matrix.pkl`
   - `tfidf.pkl`

## Run the backend API

```powershell
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Optional: use `ass.py` instead of `main.py` if you prefer `httpx` implementation:

```powershell
uvicorn ass:app --reload --host 127.0.0.1 --port 8000
```

## Run frontend

```powershell
streamlit run app.py
```

Then open URL shown by Streamlit (`http://localhost:8501` by default).

## API Endpoints

- `GET /health` (if API is live)
- `GET /home?category=<popular|trending|top_rated|upcoming|now_playing>&limit=<n>`
- `GET /tmdb/search?query=<text>&page=<1-10>`
- `GET /movie/id/{tmdb_id}`
- `GET /recommend/genre?tmdb_id=<id>&limit=<n>`
- `GET /recommend/tfidf?title=<movie>&top_n=<n>`
- `GET /movie/search?query=<movie>&tfidf_top_n=<n>&genre_limit=<n>`

## Notes

- `app.py` caches search results for 30s (`@st.cache_data(ttl=30)`).
- `main.py` is built to use TMDB plus local TF-IDF cached similarity on startup.
- If you have `c:\	emp` path issues or SSL errors, check `certifi` setup.

## Troubleshooting

- `RuntimeError: TMDB_API_KEY missing`: ensure `.env` contains `TMDB_API_KEY`.
- `FileNotFoundError` for `.pkl`: verify the dataset files are present in root.

## Optional improvement ideas

- Add Dockerfile for containerized deployment.
- Add tests for API routes and TF-IDF logic.
- Enable user-selectable local dataset source.
