# Data

The signals come from the course data pack for the project (Problem H, `DSMP_Problem_H_Data`). Only the smaller normalised test files are included here:

| File | Contents |
| --- | --- |
| `test_normalised.mat` | 220 normalised test signals |
| `test_noisy_normalised.mat` | the same signals with added noise (generated in notebook 03) |
| `test_max.mat` | 220 per-signal scale values |
| `test_signals.mat` | 4 raw sample signals |

The notebooks also expect the following files, which are not redistributed. Place them in this folder to run everything end to end:

- `train_normalised.mat`
- `test_defect_-5db.mat`, `test_defect_-10db.mat`, `test_defect_-15db.mat`, `test_defect_-20db.mat`, `test_defect_-25db.mat`, `test_defect_-30db.mat`
- `test_defect1.mat` to `test_defect6.mat`
- `scale.mat`
