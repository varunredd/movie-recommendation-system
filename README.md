# Movie Recommendation System (MovieLens 100K)

This repository contains my implementation of a **movie recommendation system** for the ITCS 6162 – Data Mining assignment.  
The goal is to explore and compare different recommendation techniques on the classic **MovieLens 100K** dataset.

---

## 📌 Project Overview

The project implements three core recommenders:

1. **User-Based Collaborative Filtering**
   - Finds users with similar rating patterns.
   - Recommends movies that similar users liked but the target user has not rated.

2. **Item-Based Collaborative Filtering**
   - Finds movies with similar rating patterns across users.
   - Supports “_Because you watched X_” style recommendations.

3. **Pixie-Inspired Graph-Based Random Walk**
   - Builds a bipartite **user–movie graph**.
   - Runs a random walk starting from a user node.
   - Ranks movies by visit frequency during the walk to generate recommendations.

Each method uses the same MovieLens 100K data but models it from a different perspective: user-centric, item-centric, and graph-centric.

---

## 📂 Repository Structure

```text
.
├── Movie_Recommendation.ipynb        # Main Jupyter notebook (all code + results)
├── ratings.csv                       # Cleaned ratings data
├── movies.csv                        # Cleaned movies data
├── users.csv                         # Cleaned users data
├── Pixie_Algorithm_Explanation.pdf   # Detailed explanation of Pixie-style random walks
├── Recommendation_Report_detailed.pdf# Full project report (methods + results)
└── README.md                         # This file
```

## 🧾 Dataset: MovieLens 100K

- **100,000 ratings**
- **943 users**
- **1,682 movies**
- **Ratings** are integers from **1 to 5**

### Main Files & Fields

#### `ratings.csv`
- `user_id`
- `movie_id`
- `rating`
- `timestamp`

#### `movies.csv`
- `movie_id`
- `title`
- `release_date`

#### `users.csv`
- `user_id`
- `age`
- `gender`
- `occupation`
- `zip_code` (used mainly for descriptive analysis)

### Data Preparation

In the notebook, the raw MovieLens files are:

- Loaded with appropriate **delimiters**
- Inspected for **missing values** and **consistency**
- Cleaned and **exported as CSVs** (the cleaned versions used in this repo)

---

## 🧠 Methods Implemented

### 1️⃣ User-Based Collaborative Filtering

**Goal:** Recommend movies to a user based on ratings from **similar users**.

**Steps:**

1. **Build the user–movie rating matrix**
   - Rows: `user_id`
   - Columns: `movie_id`
   - Values: `rating`
   - Use `pivot` to create this matrix.

2. **Handle missing ratings**
   - Fill missing values with `0` **for similarity computation**.

3. **Compute user–user similarity**
   - Use **cosine similarity** on the user–movie matrix.
   - Each row is treated as a vector representing that user’s preferences.

4. **For a target user:**
   - Select the **top-k most similar users**.
   - For each **unseen movie**, compute a **weighted average rating**:
     - Weights are based on similarity scores.
   - Recommend the **top-N movies** with the highest **predicted scores**.

---

### 2️⃣ Item-Based Collaborative Filtering

**Goal:** Recommend movies similar to a given movie, based on **co-ratings** by users.

**Steps:**

1. **Transpose the rating matrix**
   - Now:
     - Rows: `movie_id`
     - Columns: `user_id`

2. **Compute item–item similarity**
   - Use **cosine similarity** between movie vectors.

3. **For a given movie title:**
   - Look up its corresponding `movie_id`.
   - Retrieve similarity scores between that movie and all other movies.
   - Sort by similarity and select the **top-N most similar movies**, excluding the movie itself.

---

### 3️⃣ Pixie-Inspired Graph-Based Random Walk

This is a **simplified, educational version** inspired by Pinterest’s **Pixie algorithm** for recommendations.

**Graph Construction:**

1. Build a **bipartite graph**:
   - One set of nodes: **Users**
   - Other set of nodes: **Movies**
   - An **edge** exists between a user and a movie **if the user rated that movie**.

2. **Normalize ratings per user**
   - For each user, subtract their **mean rating** from each of their ratings.
   - This centers ratings so the graph emphasizes **preferences above/below personal average**.

3. **Adjacency representation**
   - Use a Python dictionary:
     - `node -> list_of_neighbor_nodes`
   - Both user and movie nodes are keys in this adjacency list.

**Random Walk Algorithm:**

1. **Start from a given `user_id`.**
2. At each step, **randomly move to a neighbor**:
   - From a user → to one of their rated movies.
   - From a movie → to one of the users who rated it.
3. **Count visits to movie nodes**:
   - Maintain a counter of how many times each movie node is visited during the walk.
4. **Filter out movies already rated by the user.**
5. Recommend movies with the **highest visit counts**:
   - These movies are considered **most relevant** to the starting user according to the random walk.

---

## ▶️ How to Run the Notebook

Follow these steps to set up and run the notebook locally:

### 1️⃣ Clone the Repository

Clone the repository (or download as ZIP and extract):

```bash
git clone https://github.com/<your-username>/movie-recommendation-system.git
cd movie-recommendation-system
```

### 2️⃣ Create a Python Environment 

Create a virtual environment:

```bash
python -m venv venv
```

Activate the environment:

On macOS / Linux:

source venv/bin/activate


On Windows:
```bash
venv\Scripts\activate
```

3️⃣ Install Dependencies

Install the required Python packages:

```bash
pip install pandas numpy scikit-learn jupyter
```

4️⃣ Launch Jupyter Notebook

Start Jupyter Notebook:

jupyter notebook

5️⃣ Run the Notebook

In the Jupyter interface, open:

```bash
Movie_Recommendation.ipynb
```

Run all cells in order, for example:

Use Kernel → Restart & Run All, or

Run each cell with Shift + Enter.
