# Dataset download sources and placement

This file records upstream sources. Download datasets from their official/source pages, read the current terms, and then place the extracted files into the paths listed below. Do not redistribute restricted datasets publicly without checking the terms that apply to your downloaded copy.

## 1. ASVspoof2019 Logical Access (LA)

Source: https://www.asvspoof.org/index2019.html

The ASVspoof 2019 page describes the registration/download process for training and development data and links the database release. Edinburgh DataShare hosts the LA archive: https://datashare.ed.ac.uk/items/31074a11-b6f6-4e92-a4ad-07093f8c0c45

Place the training partition and protocol here:

- `Training Data/ASVspoof2019_LA/ASVspoof2019_LA_train/flac/`
- `Training Data/ASVspoof2019_LA/ASVspoof2019_LA_cm_protocols/ASVspoof2019.LA.cm.train.trn.txt`

Place the development partition and its protocol here:

- `Testing Data/ASVspoof2019_LA/ASVspoof2019_LA_dev/flac/`
- `Testing Data/ASVspoof2019_LA/ASVspoof2019_LA_cm_protocols/ASVspoof2019.LA.cm.dev.trl.txt`

The ASVspoof dev partition is used by the supplied code for same-corpus auxiliary evaluation. It should not be described as an external dataset.

## 2. In-the-Wild audio deepfake dataset

Source / dataset card: https://huggingface.co/datasets/mueller91/In-The-Wild

The dataset card describes the release ZIP and its download command. Arrange the files into the project-specific train/validation/test folders as follows:

- `Training Data/InTheWild/train/real/`
- `Training Data/InTheWild/train/fake/`
- `Validation Data/InTheWild/val/real/`
- `Validation Data/InTheWild/val/fake/`
- `Testing Data/InTheWild/test/real/`
- `Testing Data/InTheWild/test/fake/`

The train/validation/test split must match the original experimental speaker split. The uploaded ZIP did not include that assignment file or the generator script. The speaker-disjoint pipeline needs `itw_speaker_train.csv`, `itw_speaker_val.csv`, and `itw_speaker_test.csv` with a `speaker` column, and all three sets of speaker IDs must be disjoint. Do not randomly split the data and then attribute the old model results to that new split.

## 3. WaveFake external benchmark (optional)

Source: https://zenodo.org/records/5642694

The supplied preparation script queries this Zenodo record for the generated WAV archive and uses `remotezip` to retrieve selected audio files. It also requires the original LJSpeech archive at:

`Additional Evaluation Data/WaveFake/LJSpeech-1.1.tar.bz2`

The preparation script creates a local matched subset and `pairs.csv`; the full WaveFake generated archive does not need to be manually extracted into the project. This benchmark is external evaluation only, not training data.
