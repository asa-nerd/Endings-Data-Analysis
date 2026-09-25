# Improethics – Data Analysis

Jupyter notebooks for analysing the study **Endings**.

Participants listened to a freely improvised concert performance (≈ 11 minutes) and marked, in real time, the moments at which they expected the performance to end. They also provided information about their personal background (age, gender, musical training, experience with improvisation).

The notebooks look at:

- the personal background of the participants,
- the distribution of the end marks over the time of the performance (filtering double marks, first marks only, kernel density estimation),
- properties of the sound recording (sonogram, perceived loudness, textural event density, tonal vs. noise content),
- relations between personal background and end marks.

## Project structure

```
.
├── notebooks/
│   ├── 01-Participants-Background.ipynb     # Load JSON data, build allDataDF, personal background
│   ├── 02-End-Marks.ipynb                   # Filter double marks, timelines, first marks, KDE
│   ├── 03-Sound.ipynb                       # Analysis of the audio recording
│   └── 04-Relations-Background-Marks.ipynb  # Background vs. end marks
├── data/
│   ├── rawData/                # Original JSON exports, one file per client (not in repository)
│   ├── processedData/          # (not in repository)
│   │   ├── jsonData/           # Datasets used for the analysis
│   │   ├── excludedJSON/       # Excluded datasets (no valid marks / incomplete)
│   │   └── filteredJSON/       # Marks only, one file per participant
│   ├── notebookData/           # Dataframes (parquet) passed between notebooks (not in repository)
│   └── sound/                  # Recording of the performance (WAV, not in repository)
├── image-output/               # Exported figures (SVG)
├── CITATION.cff
└── requirements.txt
```

## Data

The participant data (`data/rawData/`, `data/processedData/`, `data/notebookData/`) and the sound recording are not included in the repository. The notebooks are included with their outputs, so the results can be viewed without the data. To run the notebooks, the data has to be placed in the folders shown above.

Each JSON file in `data/rawData/` contains three records for one client:

| `dataset`      | Content                                                                                               |
|----------------|-------------------------------------------------------------------------------------------------------|
| `config`       | Recording, length of the timeline (`tcLength`, ms), client `id` and `personalBackground`              |
| `timesequence` | Time sequence of the session                                                                          |
| `marks`        | The end marks set by the participant (`timeStamp` in ms, `deviceType`)                                |

`personalBackground` contains `age`, `gender`, `musicaltraining` and `experienceimprovising`.

Datasets without valid marks or with incomplete data were excluded (see `data/processedData/excludedJSON/` and the notes in notebook 01).

## Setup

The project was developed with **Python 3.9** in PyCharm.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install jupyter   # if you do not run the notebooks from PyCharm
```


## Running the notebooks

Run the notebooks from inside `notebooks/`, since all paths are relative to it (e.g. `../data/...`).

1. **01-Participants-Background** reads `data/processedData/jsonData/` (not in repository) and saves `data/notebookData/allDataDF.parquet`.
2. **02-End-Marks** loads `allDataDF.parquet`.
   - **4-1:** set the time window for filtering double marks with the slider. **Save Data** writes the result to `data/notebookData/filteredDataDF.parquet`.
   - **4.4:** choose the dataset for the KDE (`firstMarksDF`, `allDataDF`, `filteredDataDF`) and set the bandwidth.
3. **03-Sound** analyses `data/sound/performanceRoughMix.wav`. The recording is not included in the repository because it is too large (199 MB). Place it in `data/sound/` before running the notebook.
4. **04-Relations-Background-Marks** loads `allDataDF.parquet` and `filteredDataDF.parquet`. Save `filteredDataDF` in notebook 02 first; notebook 04 uses whichever time window was set when you saved it.

**Save SVG** buttons write figures to `image-output/`.
