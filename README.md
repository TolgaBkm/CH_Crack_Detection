# CH_Crack_Detection

> YOLOv8x instance segmentation model for crack detection, trained and validated on the YTU-CrackIS dataset.

---

## Overview

This repository provides the code and resources to reproduce the validation results for a **YOLOv8x-seg** model trained for crack instance segmentation. The model was trained on the **YTU-CrackIS** dataset and outputs both bounding box and segmentation mask predictions.

- **Task:** Instance Segmentation
- **Model:** YOLOv8x-seg
- **Class:** `crack` (single class)
- **Input size:** 608 × 608
- **Framework:** [Ultralytics](https://github.com/ultralytics/ultralytics)

---

## Dataset

The YTU-CrackIS dataset is publicly available on Zenodo:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19051849.svg)](https://doi.org/10.5281/zenodo.19051849)

The dataset is in YOLO polygon (instance segmentation) format:
```
YTU-CrackIS/
├── images/
│   └── val2017/
└── labels/
    └── val2017/
```

---

## Model Weights

Pre-trained model weights are available on Zenodo:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19051849.svg)](https://doi.org/10.5281/zenodo.19051849)

Download and place the weights at:
```
8x-608-l4/weights/best.pt
```

---

## Requirements

```bash
pip install ultralytics pyyaml opencv-python matplotlib
```

Tested with:
- Python 3.10+
- Ultralytics 8.4.23
- PyTorch 2.10.0

---

## Validation

Run the validation script to reproduce box and mask mAP results:

```bash
python validate.py
```

This will:
1. Auto-generate `crack_dataset.yaml`
2. Load the model weights
3. Run validation on the val2017 split
4. Print box and mask mAP metrics
5. Save plots (confusion matrix, PR curve) to a results folder

**Expected output:**
```
Box  mAP@0.5      : X.XXXX
Box  mAP@0.5:0.95 : X.XXXX
Mask mAP@0.5      : X.XXXX
Mask mAP@0.5:0.95 : X.XXXX
```

> **Note:** Validation was run on CPU (Apple M4). If you have a GPU, change `device='cpu'` to `device=0` in `validate.py` for faster inference.

---

## Sample Inference

Run inference on a single image:

```python
from ultralytics import YOLO

model = YOLO("8x-608-l4/weights/best.pt", task="segment")

results = model.predict(
    source="path/to/your/image.jpg",
    imgsz=608,
    conf=0.25,
    device='cpu'   # change to 0 for GPU
)

# Show results
results[0].show()

# Save results
results[0].save(filename="result.jpg")

# Access masks and boxes
masks = results[0].masks    # segmentation masks
boxes = results[0].boxes    # bounding boxes
```

---

## Repository Structure

```
CH_Crack_Detection/
├── 8x-608-l4/
│   └── weights/
│       └── best.pt          # model weights (download from Zenodo)
├── YTU-CrackIS/             # dataset (download from Zenodo)
│   ├── images/val2017/
│   └── labels/val2017/
├── validate.py              # validation script
├── sanity_check.py          # dataset sanity check script
├── requirements.txt         # Python dependencies
├── LICENSE
└── README.md
```

---

## Acknowledgements

This work was supported by [**PROJECT NAME**], funded by [**FUNDING BODY**], under grant number [**GRANT NUMBER**].

---

## Citation

This work is currently under review. Citation information will be added upon publication.

---

## License

This project is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License**.  
See the [LICENSE](LICENSE) file for details, or visit [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc/4.0/).
