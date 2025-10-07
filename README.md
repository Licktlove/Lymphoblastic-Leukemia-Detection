# Lymphoblastic-Leukemia-Detection
# High-efficiency spatially guided learning network for lymphoblastic leukemia detection in bone marrow microscopy images
⭐ This code has been completely released ⭐ 

⭐ our [article](https://www.sciencedirect.com/science/article/abs/pii/S0010482525012119) ⭐ 

If our code is helpful to you, please cite:

```
@article{
  title={High-efficiency spatially guided learning network for lymphoblastic leukemia detection in bone marrow microscopy images},
  author={Liye Mei, Chentao Lian, Suyang Han, Zhaoyi Ye, Yuyang Hua, Meixing Sun, Jing He, Zhiwei Ye, Mengqing Mei , Yaxiaer Yalikun, Hui Shen, Cheng Lei, Bei Xiong},
  journal={Computers in Biology and Medicine},
  volume={196},
  number={110860},
  year={2025},
  publisher={Elsevier}
}

## Requirements

```python
pip install -r requirements.txt
```


<p align="center"> <img src="Fig/Model.png" width="80%"> </p>


## Train

### 1. Prepare training data 

- The download link for the LLD-2024 data set is [here](########).
```python
SGLNet
├── LLD-2024
│   ├── images
│   │   ├── train
│   │   │   ├── 1.jpg
│   │   │   ├── 2.jpg
│   │   │   ├── .....
│   │   ├── val
│   │   ├── test
│   ├── labels
│   │   ├── train
│   │   │   ├── 1.txt
│   │   │   ├── 2.txt
│   │   │   ├── .....
│   │   ├── val
│   │   ├── test
```
- After downloading the data set, modify the paths in path, train, val and test in the [data.yaml](data.yaml) file.


### 2. Begin to train
```python
python train.py
```


## Test

### 1. Begin to test
```python
python val.py
```

## Results

| **Methods** | **mAP** | **Precision** | **Recall** | **GFLOPs** ↓ | **Params (M)** ↓ | **FPS** |
|:-----------:|:-------:|:-------------:|:----------:|:-------------:|:----------------:|:-------:|
| **RTDETR**  | 0.871   | **0.879**     | 0.838      | 108.3         | 32.8             | 23.3    |
| **YOLOv5**  | 0.923   | 0.871         | 0.866      | 64.0          | 25.0             | 35.7    |
| **YOLOv6**  | 0.915   | 0.870         | 0.850      | 44.0          | 16.3             | **62.5** |
| **YOLOv8**  | 0.920   | 0.867         | **0.876**  | 80.8          | 27.2             | 34.8    |
| **YOLOv9**  | 0.921   | 0.862         | 0.872      | 102.5         | 25.4             | 36.6    |
| **SGLNet**  | **0.925** | **0.886**   | 0.863      | **40.8**      | **12.98**        | 56.1    |
- Bold indicates first or second best performance.

## Visualization of results


<p align="center"> <img src="Fig/results.png" width="80%"> </p>


## Acknowledgements
This code is built on [ultralytics (PyTorch)](https://github.com/ultralytics/ultralytics). We thank the authors for sharing the codes.



## Contact
If you have any questions, please contact me by email (102301204@hbut.edu.cn).



