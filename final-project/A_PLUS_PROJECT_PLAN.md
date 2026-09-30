# A+ Project Plan: UAV Crop-Weed Segmentation

## Project Aim

Build a complete, reproducible TensorFlow/Keras semantic segmentation project for UAV crop-weed imagery. The model should segment each RGB image into three pixel-level classes:

- background/soil
- crop
- weed

The final project should show a full deep learning workflow: problem framing, data preparation, baseline, model comparison, evaluation, error analysis, limitations, reproducibility, and AI tool disclosure.

## A+ Target

The A+ version is not about adding many features. It is about making the core RGB segmentation pipeline rigorous, clear, and reproducible.

Target final claim:

> I built and evaluated a TensorFlow/Keras RGB semantic segmentation pipeline for UAV crop-weed images, compared a naive baseline, a simple encoder-decoder CNN, and U-Net, tested one focused ablation, evaluated using Mean IoU, Dice, and weed-class IoU, and analysed both quantitative results and visual failure cases.

## Must-Have Experiments

### 1. Naive Baseline

Use a very simple baseline so the trained models have something meaningful to beat.

Recommended naive baseline:

- predict the majority pixel class for every pixel, likely background/soil

Report:

- Mean IoU
- per-class IoU
- weed-class IoU

Purpose:

- proves the model is learning more than the class distribution
- helps avoid relying on misleading pixel accuracy

### 2. Learning Baseline

Train a simple encoder-decoder CNN in TensorFlow/Keras.

Purpose:

- provides a fair simple neural baseline
- shows whether U-Net adds value beyond a basic segmentation model

Recommended characteristics:

- small number of convolution blocks
- downsampling encoder
- upsampling decoder
- softmax output with three classes

### 3. Main Model: U-Net

Train a U-Net model in TensorFlow/Keras.

Why U-Net fits:

- semantic segmentation model
- preserves spatial detail with skip connections
- suitable for small or medium-sized image datasets
- easy to explain in the report and Q&A

### 4. One Focused Ablation

Do one small experiment that tests a specific design choice.

Recommended ablation:

- U-Net without augmentation vs U-Net with augmentation

Why this is a good choice:

- directly connected to UAV imagery conditions
- easy to explain
- helps with generalisation
- lower risk than adding vegetation indices

Alternative ablation if augmentation is difficult:

- cross-entropy loss vs Dice-based loss
- no class weighting vs class weighting
- 128 x 128 input size vs 256 x 256 input size

Do not add vegetation indices unless the RGB pipeline, report figures, and README are already strong.

## Metrics

Primary metric:

- Mean IoU on the test set

Supporting metrics:

- Dice score
- per-class IoU
- weed-class IoU
- weed precision and recall if time allows

Avoid relying only on pixel accuracy, because background/soil pixels may dominate the masks.

## Results Table Template

Use a table like this in the final report:

| Model | Setup | Mean IoU | Background IoU | Crop IoU | Weed IoU | Dice |
|---|---|---:|---:|---:|---:|---:|
| Majority baseline | all pixels predicted as majority class | TBD | TBD | TBD | TBD | TBD |
| Encoder-decoder CNN | RGB only | TBD | TBD | TBD | TBD | TBD |
| U-Net | RGB only, no augmentation | TBD | TBD | TBD | TBD | TBD |
| U-Net | RGB only, with augmentation | TBD | TBD | TBD | TBD | TBD |

The strongest claim should focus on whether U-Net improves weed-class IoU and Mean IoU compared with the baselines.

## Required Figures

The final report should include:

1. Example input image and ground-truth mask
2. Pixel class distribution chart
3. Training and validation loss curve
4. Training and validation Mean IoU curve if available
5. Per-class IoU bar chart
6. Qualitative prediction comparison:
   - input image
   - ground truth mask
   - baseline prediction
   - U-Net prediction
7. Failure case examples

## Error Analysis Checklist

The report should discuss specific failure modes, not only say that the model makes mistakes.

Useful failure categories:

- small weed regions are missed
- crop and weed are confused when colour and texture are similar
- shadows cause false positives
- soil residues or green background noise cause false positives
- object boundaries are coarse after resizing
- class imbalance biases predictions toward background/soil

For A+ quality, include visual examples for the most important failure modes.

## Report Structure

Follow the course brief structure:

1. Introduction and related work
2. Data and preprocessing
3. Model architectures and training procedure
4. Results
5. Error analysis
6. AI tool disclosure
7. Conclusion and future work
8. References

## What To Cover In Each Report Section

### Introduction and Related Work

Include:

- precision agriculture motivation
- why weed segmentation matters
- why UAV imagery is useful
- brief explanation of semantic segmentation
- why U-Net is appropriate

### Data and Preprocessing

Include:

- dataset name and source URL
- image and mask structure
- three classes: background/soil, crop, weed
- train/validation/test split
- resizing strategy
- RGB normalisation
- mask label conversion
- leakage checks
- licence and ethics notes

Important leakage note:

- if patches come from larger UAV images, avoid mixing highly similar patches from the same source image across train/test if the dataset structure allows this check

### Model Architectures and Training Procedure

Include:

- naive baseline
- simple encoder-decoder CNN
- U-Net
- loss function
- optimiser
- batch size
- number of epochs
- early stopping or model checkpointing
- augmentation settings if used
- hardware note, including Apple M4 Pro if useful

### Results

Include:

- main results table
- training curves
- per-class IoU
- visual prediction examples
- comparison between baseline and U-Net
- ablation result

### Error Analysis

Include:

- at least 3 specific failure modes
- visual examples
- explanation of why each failure may happen
- connection to real UAV/agricultural deployment limitations

### AI Tool Disclosure

Minimum fields required by the course:

- tool used
- what it was used for
- what was verified
- what was written independently

Suggested content:

- Tool used: ChatGPT/Codex
- Used for: planning, code debugging, explanation of TensorFlow/Keras concepts, report editing
- Verified by: running notebooks, inspecting outputs, reading generated code, checking metrics and plots
- Written independently: final project decisions, interpretation of results, report conclusions, presentation explanation

### Conclusion and Future Work

Include:

- whether the U-Net beat the baselines
- whether weed segmentation was successful
- key limitations
- future work ideas:
  - vegetation indices
  - larger input resolution
  - stronger architectures
  - class weighting or Dice loss
  - testing on New Zealand agricultural imagery
  - edge deployment on drone or farm hardware

## README Checklist

The README should include:

- project title and goal
- dataset link and citation
- folder structure
- environment setup
- how to download/place the data
- how to run training
- how to run evaluation
- how to reproduce figures/results
- where trained models and outputs are saved
- expected runtime/hardware notes
- AI disclosure summary or link to report section

## Notebook Checklist

The notebook should be readable by a marker.

Suggested sections:

1. Problem framing
2. Imports and configuration
3. Load data
4. Visualise images and masks
5. Preprocess images and masks
6. Check class distribution
7. Naive baseline
8. Simple encoder-decoder CNN
9. U-Net
10. Ablation experiment
11. Evaluation
12. Error analysis
13. Save outputs

Every important step should have a short markdown explanation.

## Presentation Plan

Eight-minute structure:

| Time | Content |
|---:|---|
| 1 min | Problem and motivation |
| 1 min | Dataset and segmentation masks |
| 1.5 min | Baseline and U-Net architecture |
| 2 min | Key results |
| 1.5 min | Visual predictions and error analysis |
| 1 min | Limitations, AI disclosure, future work |

Presentation must include:

- problem
- data
- model
- key results
- limitations
- AI disclosure

Keep a short demo or screenshot walkthrough ready, but do not let it take over the timing.

## Scope Control

Core scope:

- RGB images only
- naive baseline
- simple encoder-decoder CNN
- U-Net
- one ablation
- quantitative metrics
- qualitative error analysis
- reproducible README

Out of scope unless everything else is complete:

- vegetation indices
- object detection
- edge deployment
- advanced segmentation architectures
- large hyperparameter search
- full production pipeline

## Final A+ Checklist

Before submission, confirm:

- [ ] project runs in TensorFlow/Keras
- [ ] dataset source and licence are cited
- [ ] train/validation/test split is clear
- [ ] naive baseline is reported
- [ ] simple encoder-decoder baseline is reported
- [ ] U-Net is reported
- [ ] one ablation is reported
- [ ] Mean IoU is reported
- [ ] weed-class IoU is reported
- [ ] Dice score is reported
- [ ] results table is included
- [ ] training curves are included
- [ ] visual predictions are included
- [ ] failure cases are analysed
- [ ] README explains reproduction steps
- [ ] AI tool disclosure is included
- [ ] references include dataset, libraries, and Chollet course text
- [ ] presentation fits 8 minutes
- [ ] every submitted code cell can be explained in Q&A
