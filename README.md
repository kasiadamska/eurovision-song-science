# eurovision-song-science

Eurovision x Hit Song Science

## 🎤 Eurovision Song Contest Dataset & Analysis

This repository contains a comprehensive dataset and analysis of the Eurovision Song Contest from 2008 to 2024, covering 646 songs in total. It includes detailed metadata for each entry, such as audio features, lyrics attributes, voting history, and YouTube performance metrics, for all participating songs/countries. Whether you're exploring patterns in music, sentiment, or voting behavior, this dataset offers valuable insights for data analysis and machine learning applications. Song information for the upcoming 2025 contest is also included (37 songs).

---

## 🗓️ About the Dataset

- **Timeframe**: 2008–2024 (contest songs) + 2025 (upcoming entries).
- **Note**: Songs from 2020 are **excluded** due to the contest's cancellation (COVID-19).
- **Structure**: Post-2008 structure includes two semi-finals and a grand final. Top 10 from each semi-final qualify to the final.

This setup allows for isolated analysis of semi-final vs. grand final performance and predictive modeling.

---

## 📚 Sources

- **Base Dataset**: [MIRoVision dataset](https://github.com/Spijkervet/eurovision-dataset)  
  *Citation*: Burgoyne, J. A., Spijkervet, J., & Baker, D. J. (2023). *Measuring the Eurovision Song Contest: A living dataset for real-world MIR*. ISMIR.
- **Supplementary Data**: [EurovisionWorld](https://eurovisionworld.com/)
  - Results & voting data (2023–2024)
  - Song metadata & lyrics (2024–2025)
  - English lyrics translations (all years)
- **Audio**: Studio recordings only via YouTube links (for consistency in analysis).

---

## 📁 Repository Structure

- `paper-data/`: Datasets used in the associated paper:
  - `full_data`: All songs
  - `semi_final_data`: Only semi-final entries
  - `grand_final_data`: Finalists only
- `data/`: Updated datasets with extended features (audio, lyrics, YouTube metadata) including **2025** entries.
- Feature extraction notebooks:
  - `audio-features/`
  - `lyrics-features/`
  - `vote-features/`
  - `youtube-features/`
- `paper-analysis/`: Analysis scripts (preprocessing, feature engineering, modeling using XGBoost AutoML).

---

## 📌 Feature Descriptions

### 🏷 Metadata

| Column              | Description                                                   |
|---------------------|---------------------------------------------------------------|
| `year`              | Contest year                                                  |
| `to_country`        | Country represented                                           |
| `performer`         | Artist name                                                   |
| `song`              | Song title                                                    |
| `sf_num`            | Semi-final number (1, 2 or none)                              |
| `qualification`     | 0 = Did not qualify, 1 = Qualified, 2 = Pre-qualified         |
| `place_sf`          | Semi-final ranking                                            |
| `place_final`       | Final ranking                                                 |
| `place_contest`     | Overall ranking                                               |
| `points_sf`         | Semi-final points (televote only from 2023)                   |
| `points_final`      | Total final points (jury + televote)                          |
| `points_tele_final` | Final televote points (from 2016)                             |
| `points_jury_final` | Final jury points (from 2016)                                 |

---

### 📺 YouTube Links

| Column           | Description                                                     |
|------------------|-----------------------------------------------------------------|
| `performance_url`| Live contest performance video                                  |
| `audio_url`      | Studio recording (for audio feature extraction)                 |
| `pre_contest_url`| Official pre-contest music video (for YouTube feature extraction|

---

### 🗳️ Contest-Related & Voting Features

| Column               | Description                                                  |
|----------------------|--------------------------------------------------------------|
| `running_final`      | Performance order in the final                               |
| `running_sf`         | Performance order in semi-finals                             |
| `LY_SF_reciprocation`| Previous year's vote reciprocation ratio (semi-finals)       |
| `LY_SF_vote`         | Previous year's vote ratio (semi-finals)                     |
| `LY_final_reciprocation`| Previous year's vote reciprocation ratio (final)       |
| `LY_final_vote`      | Previous year's vote ratio (final)                           |

---

### ✍️ Lyrics Features

| Column                  | Description                                                |
|-------------------------|------------------------------------------------------------|
| `lyrics`                | Song lyrics                                                 |
| `lyrics_eng_translation`| English translation (if needed)                            |
| `lyrics_all_english`    | Unified English lyrics (original or translated)            |
| `lyrics_english`        | Binary flag (1 = English lyrics)                           |
| `lyrics_english_mix`    | Binary flag (1 = English + another language)               |
| `total_words`           | Word count                                                 |
| `unique_words`          | Unique word count                                          |
| `type_token_ratio`      | Vocabulary richness                                        |
| `compression_size_reduction` | LZW compression ratio (lyrical repetitiveness)       |
| `n_gram_repetitiveness` | N-gram repetition measure (2–10 word sequences)           |
| **Sentiment scores:** `sadness`, `joy`, `love`, `anger`, `fear`, `surprise` | values (0–1)|
| **`topic`**                | Main topic label (`love` or `reflection`)                 |

> **Bolded features** in the dataset are *updates not included in the paper version*.

---

### 🎥 YouTube Features

| Column               | Description                                                |
|----------------------|------------------------------------------------------------|
| `yt_channel`         | YouTube channel name                                      |
| `upload_date`        | Upload date                                               |
| `total_views`        | Total views                                               |
| `likes`              | Total likes                                               |
| `comments`           | Total comments                                            |
| `days_since_upload`  | Time difference since upload                              |
| `yt_views_per_day`   | Views per day                                             |
| **`average_replay_intensity`**| Average intensity of most replayed segments         |
| **`skewness_value`**     | Skewness of replay distribution                           |
| **`mean_abs_difference`**| Smoothness measure (abs first differences)                |
| **`variance_difference`**| Smoothness measure (variance of differences)              |
| **`primary_peak_time`**  | % position of most replayed moment                        |
| **`number_of_maxima`**   | Number of local peaks                                     |
| **`additional_peak_times`**| Time positions of other peaks                          |

> **Bolded features** in the dataset are *updates not included in the paper version*.
---

### 🎧 Audio Features

Audio features are extracted using Essentia’s Music Extractor toolkit: https://essentia.upf.edu/streaming_extractor_music.html

**Category | Features**

**Low-level audio**  
average loudness, loudness ebu128 int, loudness ebu128 range, dissonance, dynamic complexity,  
hfc, pitch salience, zero-crossing rate,  
Bark bands (crest, flatness_db, kurtosis, skewness, spread),  
ERB bands (crest, flatness_db, kurtosis, skewness, spread),  
Mel bands (crest, flatness_db, kurtosis, skewness, spread),  
spectral features* (spectral centroid, spectral complexity, spectral decrease, spectral energy,  
spectral energy band high, spectral energy band low,  
spectral energy band middle high, spectral energy band middle low,  
spectral entropy, spectral flux, spectral kurtosis, spectral rms, spectral rolloff,  
spectral skewness, spectral spread, spectral strong peak)  
**mfcc, gfcc, spectral contrast coeffs, spectral contrast valleys, silence rate**

**Rhythm**  
bpm, beats count, beats loudness, **beats loudness band ratio**, onset rate, danceability

**Tonal**  
chords changes rate, chords number rate, chords strength, **chords histogram**,  
hpcp crest, hpcp entropy, tuning diatonic strength, tuning equal tempered deviation, tuning frequency,  
tuning nontempered energy ratio, chords key, chords scale, key edma (strength, key, scale),  
key krumhansl (strength, key, scale), key temperley (strength, key, scale)

> *Features in **bold** are updated and not present in the paper version of the dataset.*

---

## 📊 Analysis Pipeline

See `paper-analysis/` for scripts covering:

- Data processing & feature engineering
- Dimensionality reduction
- Correlation analysis
- XGBoost model training with AutoML
- Evaluation metrics and outputs

---

## 📝 Citation

> This dataset and analysis are part of the following publication.
> 
> Adamska, K., & Reiss, J. (2025). Predicting Eurovision Song Contest Results: A Hit Song Science Approach.Transactions of the International Society for Music Information Retrieval, 8(1), 93–107. DOI: https://doi.org/10.5334/tismir.214

