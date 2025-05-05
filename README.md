# Real‑Time Pedestrian Tracking  
*A YOLOv5 + DeepSORT implementation for COMP3065 coursework*

---

## 1. Overview
This repository contains a compact Python pipeline that detects and tracks
pedestrians in raw video.  
Key features :

* **YOLOv5‑s** detector – fine‑tuned for 150 epochs on a merged
  pedestrian dataset (CityPersons v7 + self‑recorded frames).
* **DeepSORT** tracker – Kalman filter + appearance embedding for stable IDs.
* **Mobile‑video support** – automatic orientation correction
  (`--auto-orient`) fixes sideways clips from smartphones.
* **Interactive GUI** – OpenCV sliders to adjust *confidence* and *IoU*
  thresholds; hot‑keys: `space` pause, `n` step, `q` quit.
* **Headless fallback** – if `$DISPLAY` is missing, GUI is disabled but video
  and TXT logs are still produced.

---

## 2. Quick Start

### 2.1 Clone & install
```bash
git clone https://github.com/<your‑handle>/pedestrian-tracker.git
cd pedestrian-tracker
conda env create -f environment.yml   # or: pip install -r requirements.txt
conda activate pedtrack
````

### 2.2 Download weights

| File                 | Size  | Link                       |
| -------------------- | ----- | -------------------------- |
| `yolov5s_ped.pt`     | 14 MB | ⬇ see `weights/README.txt` |
| `ckpt.t7` (DeepSORT) | 5 MB  | ⬇ official release         |

Place both files in `weights/`.

### 2.3 Inference demo

```bash
python main.py \
  --input_path test_video/clip_A.mp4 \
  --auto-orient \
  --display                # omit when running on a headless server
```

Outputs:

* `output/clip_A_<timestamp>.mp4` – annotated video
* `output/predict/*.txt`   – per‑frame boxes & IDs
* `results_summary.txt`    – average FPS & ID‑switch count

---

## 3. Training (optional)

```bash
python yolov5/train.py \
  --img 640 --batch 16 --epochs 150 \
  --data data/pedestrian.yaml \
  --weights yolov5s.pt \
  --name ped_custom
```

The training curves are saved to `runs/train/ped_custom/results.png`
(see `fig/yolo_training.png` for the final run).

---

## 4. Project Structure

```
.
├─ main.py                # entry‑point: detection + tracking
├─ utils_ds/              # helper modules (draw, parser, etc.)
├─ yolov5/                # Ultralytics detector (submodule)
├─ deep_sort/             # tracker code
├─ weights/               # YOLO + DeepSORT checkpoints
├─ test_video/            # three sample clips
├─ fig/                   # figures used in the report
└─ extract_frames.py      # utility to create nine‑frame contact sheets
```

---

## 5. Results (A5000, fp16, 640×640)

| Clip           | Precision | Recall | mAP\@0.5 | FPS |
| -------------- | --------- | ------ | -------- | --- |
| High‑density   | 0.62      | 0.58   | 0.62     | 49  |
| Medium‑density | 0.63      | 0.59   | 0.62     | 49  |
| Hand‑held      | 0.61      | 0.56   | 0.61     | 47  |

*Numbers match the final validation epoch.  See the PDF report for full details.*

---

## 6. Citation

If you use this code, please cite the original works:

```bibtex
@article{yolov5,
  title  = {{YOLOv5}}, author = {Glenn Jocher et al.},
  year   = 2020, url = {https://github.com/ultralytics/yolov5}}
@inproceedings{wojke2017simple,
  title = {Simple Online and Realtime Tracking with a Deep Association Metric},
  author = {Nicolai Wojke and Alex Bewley and Dietrich Paulus},
  booktitle = {ICIP}, year = {2017}}
@misc{citypersons-woqjq_dataset,
  title={Citypersons Dataset}, author={Citypersons conversion},
  howpublished = {\url{https://universe.roboflow.com/citypersons-conversion/citypersons-woqjq}},
  year={2022}}
```

---

## 7. Contact

**Yancheng Chen**
20411992 ｜ [scyyc19@nottingham.edu.cn](mailto:scyyc19@nottingham.edu.cn)
