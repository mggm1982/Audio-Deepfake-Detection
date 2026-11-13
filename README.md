# Real AI Detector — CWS Project Repository

This is an organized copy of the project archive supplied for the CWS submission. The original upload is preserved as `Original_Upload.zip`. The source scripts are grouped by responsibility under `Code/src/`; no PPT, video presentation, or preprint has been created here.

## Folder map

- `Code/` — Python scripts grouped by data preparation, training, evaluation, inference, and diagnostics, plus dependency list and script catalog.
- `Training Data/` — ASVspoof2019 LA training partition and In-the-Wild training recordings.
- `Validation Data/` — In-the-Wild validation partition.
- `Testing Data/` — held-out In-the-Wild test partition, ASVspoof2019 LA dev partition, and optional user audio.
- `Additional Evaluation Data/` — optional WaveFake external benchmark subset and its LJSpeech reference archive.
- `Manifests/` — CSV indexes used by scripts.
- `Model Weights/` — three preserved PyTorch checkpoints.
- `Results/` — original logs, metrics, and predictions.

## Environment setup

Use Python 3.10 or 3.11 as a practical starting point. Install PyTorch separately using the official selector for the machine's operating system and CPU/CUDA setup: https://pytorch.org/get-started/locally/ . Then, from the project root, install the remaining requirements:

```bash
pip install -r Code/requirements.txt
```

## Important: data and manifest status

The uploaded ZIP did not contain raw audio, manifest CSVs, or the original speaker-disjoint manifest-generation script. Therefore, this organized repository includes the expected directory layout and precise data placement notes, but it is not yet a self-contained reproducible run.

The ordinary manifest builder can be run from the project root after placing the matching raw audio and protocol files:

```bash
python Code/src/data_preparation/create_manifests.py
python Code/src/data_preparation/create_asvspoof_dev_manifest.py
```

**Do not run the speaker-disjoint training script until the three `itw_speaker_*.csv` files have been recovered or recreated from the exact documented speaker split.** The included ordinary manifest builder does not create them. Randomly recreating those splits would produce a different experiment and would not validate the saved checkpoint's original result.

## Main final-pipeline commands (after restoring the missing speaker manifests)

```bash
python Code/src/training/train_cnn_speaker_disjoint.py
python Code/src/evaluation/evaluate_speaker_disjoint.py
python Code/src/evaluation/evaluate_speaker_model_asvspoof_dev.py
python Code/src/evaluation/analyze_speaker_performance.py
```

Optional WaveFake subset preparation/evaluation:

```bash
python Code/src/data_preparation/prepare_wavefake_benchmark.py
python Code/src/evaluation/evaluate_wavefake_external.py
```

## Split counts recorded in the supplied final training log

The saved speaker-disjoint experiment log records:

| Split / source | Real | Fake / spoof | Total |
|---|---:|---:|---:|
| ASVspoof2019 LA training | 2,580 | 22,800 | 25,380 |
| In-the-Wild training | 12,401 | 8,258 | 20,659 |
| **Combined training** | **14,981** | **31,058** | **46,039** |
| In-the-Wild validation | — | — | 2,154 |
| In-the-Wild held-out test | 6,184 | 2,782 | 8,966 |

The log also records 38 training speakers, 8 validation speakers, and 8 held-out test speakers. The validation/test labels are not included in the combined training count. These figures are reported from the supplied log, not recalculated from raw audio (which was absent from the ZIP).

## Audio preprocessing and spectrograms

The training dataset classes perform audio reading, mono conversion, resampling to 16 kHz, 4-second crop/padding, STFT, Mel filter-bank projection, log scaling, clipping, and normalization on the fly. The repository does not contain a standalone spectrogram-export script or a saved spectrogram dataset. Do not treat spectrograms as missing source data.

## Code organization note

The Python files have been moved into functional folders and their root paths/imports updated for this repository layout. Model logic and the training/evaluation calculations have not intentionally been redesigned. The original archive remains available for comparison.
