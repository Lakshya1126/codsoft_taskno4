# Movie Recommendation System

A small Python demo of two basic recommendation approaches using **NumPy** and **pandas**:

1. **User-based recommendations**: suggests movies that other users rated, which the target user hasn't seen yet.
2. **Content-based recommendations**: suggests movies that match a chosen genre.

## Requirements

- Python 3.8+
- numpy
- pandas

```bash
pip install numpy pandas
```

## Data

The script uses two in-memory DataFrames, so no external files are needed.

**`ratings`**: user ratings for movies

| Column     | Description              |
|------------|--------------------------|
| `user_id`  | ID of the user           |
| `movie_id` | ID of the movie          |
| `rating`   | Rating given (1-5)       |

**`movies`**: movie metadata

| Column     | Description              |
|------------|--------------------------|
| `movie_id` | ID of the movie          |
| `title`    | Movie title              |
| `genre`    | Genre (Action, Comedy, Thriller) |

## Usage

```bash
python recommender.py
```

(Replace `recommender.py` with your script's filename.)

## How It Works

### `recommend_movies_user_based(user_id)`

1. Separates the target user's ratings from all other users' ratings.
2. Keeps only movies the target user has **not** rated yet.
3. Averages the other users' ratings for each of those movies.
4. Returns the movies sorted by average rating, highest first.

### `recommend_movies_content_based(user_genre)`

Returns all rows from `movies` whose `genre` matches the given genre.

## Example Output

```
User-Based Recommendations:
movie_id
4    4.0
3    2.0
Name: rating, dtype: float64

Content-Based Recommendations for Action genre:
   movie_id    title   genre
0         1  Movie A  Action
2         3  Movie C  Action
```

For user 1, Movie D (id 4) ranks first with an average rating of 4.0, followed by Movie C (id 3) at 2.0.

## Limitations

- The user-based approach does not compute similarity between users. It simply averages ratings from all other users.
- The content-based approach only matches on an exact genre and does not use the user's rating history.
- The dataset is tiny and hardcoded, so it is for demonstration only.

## Possible Improvements

- Add user similarity (cosine similarity or Pearson correlation) and weight ratings by it.
- Build a user-item matrix for collaborative filtering.
- Combine both methods into a hybrid recommender.
- Show movie titles in the user-based results by merging with `movies`.
- Load data from CSV files instead of hardcoding it.
