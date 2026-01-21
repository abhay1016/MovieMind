# Movie Recommender System

A full-stack movie recommendation application that provides personalized movie suggestions using TF-IDF content-based filtering and genre-based recommendations. The system combines a FastAPI backend API with a Streamlit frontend interface.

## Features

- **Content-Based Recommendations**: Uses TF-IDF (Term Frequency-Inverse Document Frequency) to find movies similar to your selection based on textual content
- **Genre-Based Recommendations**: Suggests movies from similar genres
- **Movie Search**: Search for movies with autocomplete functionality
- **Movie Details**: View comprehensive information about movies including ratings, release dates, and descriptions
- **TMDB Integration**: Fetches real movie data, posters, and metadata from The Movie Database (TMDB) API
- **Responsive UI**: Modern web interface built with Streamlit

## Tech Stack

### Backend
- **FastAPI**: High-performance Python web framework for building APIs
- **Uvicorn**: ASGI server for FastAPI
- **Scikit-learn**: Machine learning library for TF-IDF vectorization
- **Pandas & NumPy**: Data manipulation and numerical computing
- **HTTPX**: Asynchronous HTTP client for TMDB API calls
- **Python-dotenv**: Environment variable management

### Frontend
- **Streamlit**: Modern data app framework for building interactive UIs
- **Requests**: HTTP library for API communication

### Data
- **df.pkl**: Pre-processed movie dataset
- **tfidf_matrix.pkl**: Pre-computed TF-IDF matrix for fast similarity calculations
- **tfidf.pkl**: Fitted TF-IDF vectorizer object
- **indices.pkl**: Movie title to index mapping for quick lookups

## Project Structure

```
movie_recommend/
├── main.py              # FastAPI backend application
├── app.py               # Streamlit frontend application
├── requirements.txt     # Python dependencies
├── .env                 # Environment variables (TMDB_API_KEY)
├── df.pkl               # Movie dataset
├── tfidf_matrix.pkl     # Pre-computed TF-IDF matrix
├── tfidf.pkl            # TF-IDF vectorizer
├── indices.pkl          # Movie indices mapping
└── README.md            # This file
```

## Setup & Installation

### Prerequisites
- Python 3.8 or higher
- A TMDB API key (get it free at [themoviedb.org](https://www.themoviedb.org/settings/api))

### Installation Steps

1. **Clone or download the project**
   ```bash
   cd movie_recommend
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**
   
   Create or update the `.env` file in the project root:
   ```
   TMDB_API_KEY=your_tmdb_api_key_here
   ```

## Running the Application

### Option 1: Start Backend & Frontend Together

```bash
# Terminal 1: Start the FastAPI backend
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000

# Terminal 2: Start the Streamlit frontend
streamlit run app.py
```

### Option 2: Deploy Backend to Cloud

Update the `API_BASE` in `app.py` to point to your deployed backend:
```python
API_BASE = "https://your-deployed-api.com"
```

## API Endpoints

### `/recommend`
Get movie recommendations based on a selected movie

**Parameters:**
- `tmdb_id` (int): The TMDB ID of the movie
- `limit` (int, optional): Number of recommendations to return (default: 10)

**Response:**
Returns a `SearchBundleResponse` with:
- Movie details from TMDB
- TF-IDF based recommendations
- Genre-based recommendations

### `/search`
Search for movies by title

**Parameters:**
- `query` (str): Movie title to search for
- `limit` (int, optional): Number of results to return

**Response:**
Returns a list of `TMDBMovieCard` objects with basic movie information

## How It Works

1. **TF-IDF Vectorization**: Movie overviews and descriptions are converted to TF-IDF vectors
2. **Similarity Calculation**: Cosine similarity is computed between the selected movie and all others
3. **Ranking**: Movies are ranked by similarity score
4. **TMDB Enrichment**: The top recommendations are enhanced with TMDB data (posters, ratings, etc.)
5. **Genre Filtering**: Additional recommendations based on matching genres are provided

## Environment Variables

Create a `.env` file in the project root:

```env
TMDB_API_KEY=your_api_key_here
```

**TMDB_API_KEY**: Required for fetching movie metadata and posters from The Movie Database

## Data Files

The application relies on pre-computed ML artifacts:

- **df.pkl**: DataFrame containing movie information (title, genres, overview, etc.)
- **tfidf_matrix.pkl**: Pre-computed sparse matrix of TF-IDF vectors
- **tfidf.pkl**: Fitted TfidfVectorizer object for transforming new text
- **indices.pkl**: Dictionary mapping movie titles to their DataFrame indices

These files are generated during data preprocessing and should not be modified manually.

## Usage Example

1. Open the Streamlit app in your browser
2. Browse the home page to see popular or recommended movies
3. Click on a movie poster to view details
4. Get recommendations based on your selection
5. Explore similar movies by genre or content similarity

## Performance Notes

- TF-IDF calculations are pre-computed for fast recommendations
- Short cache TTL (30 seconds) on autocomplete API calls
- Recommendations are generated in real-time with minimal latency
- CORS is enabled for cross-origin requests

## Future Enhancements

- Collaborative filtering based on user ratings
- User accounts and personalized recommendation history
- Real-time search suggestions with caching
- Advanced filtering by year, rating, runtime, etc.
- Watch history tracking and user preferences

## Troubleshooting

### "TMDB_API_KEY missing" error
- Ensure you have a valid TMDB API key
- Add it to your `.env` file in the project root
- Restart the application after adding the key

### "Request failed" errors
- Check your internet connection
- Verify the TMDB API key is valid
- Ensure the backend API is running and accessible

### Slow recommendations
- Check if the pickle files are loaded correctly
- Verify sufficient disk space for the pre-computed matrices
- Consider using a server with better CPU for faster similarity calculations

## License

This project is open source and available for personal and educational use.

## Support

For issues, questions, or suggestions, please check the project's issue tracker or documentation.
