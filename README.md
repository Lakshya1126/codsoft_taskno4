<div align="center">

# Movie Recommendation System

**A lightweight Python implementation of user-based and content-based movie recommendations.**


</div>

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Data Model](#data-model)
- [API Reference](#api-reference)
- [Example Output](#example-output)
- [Design Notes and Limitations](#design-notes-and-limitations)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This project demonstrates the core ideas behind recommender systems using a small, self-contained dataset. It implements two foundational approaches:

| Approach | Idea | Signal used |
|----------|------|-------------|
| **User-based** | Recommend unseen movies that other users rated highly | Ratings from other users |
| **Content-based** | Recommend movies that match a chosen genre | Movie metadata |

It is designed as an educational reference and as a starting point for more advanced implementations. No external data files or services are required.

## Key Features

- **Unseen-movie filtering:** only movies the target user has not yet rated are recommended.
- **Ranked output:** user-based results are sorted by average rating, highest first.
- **Genre filtering:** content-based results return every movie in the requested genre.
- **Self-contained:** the sample dataset is defined in memory, so the demo runs out of the box.
- **Minimal dependencies:** only NumPy and pandas.

## Quick Start

### Prerequisites

- Python 3.8 or later
- `pip`

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/movie-recommendation-system.git
cd movie-recommendation-system

# 2. (Recommended) Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install numpy pandas
```

### Run the Demo

```bash
python recommender.py
```

> Replace `recommender.py` with the filename of your script.

## Usage

The recommendation functions can also be called directly:

```python
# Recommend unseen movies for user 1, ranked by average rating
user_recs = recommend_movies_user_based(user_id=1)
print(user_recs)

# Recommend all movies in the Action genre
genre_recs = recommend_movies_content_based(user_genre="Action")
print(genre_recs)
```

## Data Model

The demo uses two in-memory pandas DataFrames.

### `ratings`

| Column | Type | Description |
|--------|------|-------------|
| `user_id` | `int` | Unique user identifier |
| `movie_id` | `int` | Unique movie identifier |
| `rating` | `int` | User rating on a 1 to 5 scale |

### `movies`

| Column | Type | Description |
|--------|------|-------------|
| `movie_id` | `int` | Unique movie identifier |
| `title` | `str` | Movie title |
| `genre` | `str` | Genre: `Action`, `Comedy`, or `Thriller` |

## API Reference

### `recommend_movies_user_based(user_id)`

Recommends movies the target user has not yet rated, based on how other users rated them.

| Parameter | Type | Description |
|-----------|------|-------------|
| `user_id` | `int` | ID of the user to generate recommendations for |

**Returns:** `pandas.Series` indexed by `movie_id`, containing the average rating of each unseen movie, sorted in descending order.

**Algorithm**

1. Split ratings into those from the target user and those from all other users.
2. Exclude movies the target user has already rated.
3. Compute the mean rating of each remaining movie across the other users.
4. Sort by mean rating, highest first.

---

### `recommend_movies_content_based(user_genre)`

Recommends movies that belong to a given genre.

| Parameter | Type | Description |
|-----------|------|-------------|
| `user_genre` | `str` | Genre to filter by (exact match) |

**Returns:** `pandas.DataFrame` containing every row from `movies` whose `genre` equals `user_genre`.

## Example Output

```text
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

For user 1, **Movie D** (`movie_id` 4) is the top recommendation with an average rating of 4.0, followed by **Movie C** (`movie_id` 3) at 2.0.

## Design Notes and Limitations

| Area | Current behavior | Impact |
|------|------------------|--------|
| User similarity | Ratings are averaged across all other users without weighting | Recommendations are not personalized to similar tastes |
| Genre matching | Exact string match only; rating history is ignored | No partial or multi-genre matching |
| Dataset | Small and hardcoded | Results are illustrative, not statistically meaningful |
| Output | User-based results show `movie_id` only | Titles require a manual lookup in `movies` |

## Roadmap

- [ ] Weight ratings by user similarity (cosine similarity or Pearson correlation)
- [ ] Build a user-item matrix for collaborative filtering
- [ ] Add a hybrid recommender that combines both approaches
- [ ] Join user-based results with `movies` to display titles
- [ ] Load data from CSV files
- [ ] Add unit tests and continuous integration

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Commit your changes: `git commit -m "Add your feature"`.
4. Push to the branch: `git push origin feature/your-feature`.
5. Open a pull request describing your changes.

For major changes, please open an issue first to discuss what you would like to change.

## License

Distributed under the MIT License. See [`LICENSE`](LICENSE) for details.
