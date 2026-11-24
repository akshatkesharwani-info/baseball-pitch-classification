# Baseball Pitch Type Classification

Classify the type of pitch (fastball, slider, curveball, changeup and more) from speed, spin and movement.

## STAR
- **Situation:** Pitch type is normally tagged by Statcast, but it is a good test of whether physical measurements alone can separate pitch types.
- **Task:** Build a classifier that works on pitchers it has never seen, not just on pitchers it trained on.
- **Action:** Pulled live MLB Statcast data for 1 to 5 June 2023 with pybaseball (17,528 pitches). Removed missing values and pitch types with fewer than 300 pitches, leaving 17,300 pitches from 357 pitchers and 9 pitch types. Used speed, spin rate, spin axis, horizontal and vertical movement, release position, extension and throwing hand. Split by pitcher (no pitcher in both train and test, 12,936 train and 4,364 test pitches). Compared Logistic Regression, Random Forest and Gradient Boosting.
- **Result:** Random Forest reached **79.1% accuracy** and **0.644 macro F1** on unseen pitchers against 29.8% for always guessing four-seam fastball.

## Results (test set, 4,364 pitches from unseen pitchers)
| Model | Accuracy | Macro F1 |
|---|---|---|
| Always four-seam fastball (baseline) | 29.79% | - |
| Logistic Regression | 75.11% | 0.604 |
| Random Forest | **79.08%** | **0.644** |
| Gradient Boosting | 77.13% | 0.606 |

Per-class F1 (Gradient Boosting report): FF 0.92, SI 0.85, CU 0.79, CH 0.72, SL 0.72, ST 0.64, FC 0.42, FS 0.33, KC 0.07.

## Key insights
- Random Forest importance: vertical movement 0.249, release speed 0.202, horizontal movement 0.160, spin rate 0.107, spin axis 0.099. Throwing hand barely matters (0.018).
- Four-seam fastballs, sinkers and curveballs are easy. Cutters (FC) are the main problem: 180 were predicted as sliders and 87 as four-seam fastballs, since a cutter sits between the two.
- Knuckle curves (KC) are almost never found (recall 3.8%) because they look like curveballs and only 331 were in the data.
- Other common mistakes: changeup as sinker (76), slider as sweeper (75), splitter as changeup (69).

## Limitations
- Only 5 days of one season, so some pitch types have few examples.
- Macro F1 (0.64) is much lower than accuracy because rare pitch types are weak.
- The per-class table above comes from the Gradient Boosting model in the notebook, not the best model (Random Forest).

## How to run
1. Open `baseball_pitch_classification.ipynb` in Google Colab.
2. Runtime > Run all (installs `pybaseball`, the Statcast download takes around 15 to 60 seconds).
3. The first data cell should print "real data loaded". Warnings from pybaseball are harmless.

## Files
- `baseball_pitch_classification.ipynb` - executed notebook with outputs
- `requirements.txt`

## Author
Akshat Kesharwani
- Portfolio: https://akshatkesharwani-info.github.io/
- GitHub: https://github.com/akshatkesharwani-info
