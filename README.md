# <p align="center">Evaluation of Range Sensing-based Place Recognition for Long-term Urban Localization</p>

<p align="center">Paper : https://ieeexplore.ieee.org/document/10477884</p>

## Obejctive
<p align="center"><img src="PR.png" width=400></p>
<p align="center">Problem formulation of place recognition</p>

<p align="center"><img src="Borease Dataset.png" width=820></p>
<p align="center">Seasonal and weather changes in long-term scenarios in Borease Dataset</p>

The effectiveness of long-term place recognition may be degraded by environment changes, such as seasonal changes and weather changes. To have a deep understanding of this issue, we conduct a comprehensive evaluation study on several state-of-the-art range sensing-based (i.e., LiDAR and radar) place recognition methods on the Borease dataset that encapsulates long-term localization scenarios with stark seasonal variations and adverse weather conditions. In addition, We design a new metric to evaluate the influence of matching thresholds on the performance of place recognition in long-term localization.

If you find this work helpful, please consider citing:
```bibtex
@ARTICLE{ma2024evaluation,
  author={Ma, Weixin and Yin, Huan and Yao, Lei and Sun, Yuxiang and Su, Zhongqing},
  journal={IEEE Transactions on Intelligent Vehicles}, 
  title={Evaluation of Range Sensing-based Place Recognition for Long-term Urban Localization}, 
  year={2024},
  volume={},
  number={},
  pages={1-12},
}
```

## AWC-FT
A novel metric to evaluate the influence of matching thresholds on place recognition performance for long-term localization.
<p align="center"><img src="AwC_FT.png" width=600></p>
<p align="center"> An example for how to compute the proposed $AwC\mbox{-}FT$ (i.e., the area of gray area). The normalized $AwC\mbox{-}FT$, $\overline{AwC\mbox{-}FT}$ is the ratio of the original $AwC\mbox{-}FT$ to the area of the purple dashed box.</p>

## Raw Data
All the results in the paper can be found in this repository.

### Details about the Downsampled Sequences
All the detailed files for the down-sampled sequences used for evaluation can be found in floder `frame_info`. In each child folder, there are `lidar_frames.txt` and `radar_frames.txt`as shown as following:

```text
boreas-YYYY-MM-DD-HH-MM
|--- lidar_frames.txt
|     |--- 1606417097502930 0.814389404 -0.580309922 -0.003208223 -0.001453277 0.580209436 0.814116180 0.023913492 -0.002786294 -0.011265371 -0.021336336 0.999708884 0.129992412 0.000000000 0.000000000 0.000000000 1.000000000 
|          ...
|--- radar_frames.txt
|     |--- 1606417097528152 0.979843754 0.199175060 0.015346467 -0.005591902 0.199444473 -0.979731467 -0.018658891 -0.010576910 0.011319031 0.021343566 -0.999708123 0.494885921 0.000000000 0.000000000 0.000000000 1.000000000 
           ...
```

`lidar_frames.txt` records details about the LiDAR frames of the sequence `boreas-YYYY-MM-DD-HH-MM`. The $1^{th}$ column is the timestamp of the LiDAR frame. Column 2-17 is the $4 \times 4$ pose matrix of the LiDAR frame.

`radar_frames.txt` records details about the Radar frames of the sequence `boreas-YYYY-MM-DD-HH-MM`. The $1^{th}$ column is the timestamp of the Radar frame. Column 2-17 is the $4 \times 4$ pose matrix of the Radar frame.

### Data Organization for Matching Results

All the matching results are stored in folder `results`. Matching results for each { $\{< k, j >}$ } $_j$ is compressed as `.rar` file. The data organization of each `.rar` file is as shown as following: 

```text
boreas-YYYY-MM-DD-HH-MM
|---LiDAR-Iris
|   |--1
|      -loop_result.txt
|      -que_frame_pose.txt
|      -query_ref_id.txt
|      -ref_frame_pose.txt
|   |--2
|      - ...
|   |--3
|      - ...
|   |--4
|      - ...
|   |--5
|      - ...
|---LiDAR-Iris-radar
|   |-- ...
|---MinkLoc3Dv2
|   |-- ...
|---OverlapTransformer
|   |-- ...
|---Scan Context
|   |-- ...
|---Scan Context-radar
|   |-- ...
```

`loop_result.txt` is the top-1 matching results for each query frame of the query sequence. The $1^{th}$ column is the ID of frame in query sequence. The $2^{nd}$ column is the ID of the matching frame in reference sequence. The $3^{th}$ column is the similarity between the query frame and the matching frame.
```text
loop_result.txt
|-- 0 3304 0.181415737
    ...
```

`que_frame_pose.txt` is the ground-truth pose of the query frame. The $1^{th}$ column refers to `x` coordinate. The $2^{nd}$ column refers to `y` coordinate. The $3^{th}$ column refers to `z` coordinate. 
```text
que_frame_pose.txt
|-- -4.318759337 0.052690267 0.025194207
    ...
```

`ref_frame_pose.txt` is the ground-truth pose of the reference frame. The $1^{th}$ column refers to `x` coordinate. The $2^{nd}$ column refers to `y` coordinate. The $3^{th}$ column refers to `z` coordinate. 
```text
ref_frame_pose.txt
|-- -0.001453277 -0.002786294 0.129992412
    ...
```

`query_ref_id.txt` records the sequence IDs for query sequence and reference sequence used for the current folder. 
```text
query_ref_id.txt
|-- que: boreas-2020-12-01-13-26
    ref: boreas-2020-11-26-13-58
```

## Devkit 
We also provide tools for calculating $\overline{AwC\mbox{-}RT}$, $\overline{AwC\mbox{-}PT}$, and $\overline{AwC\mbox{-}FT}$.

### Dependencies
1) Numpy
2) Matplotlib

### Example Usage
1) Download any `.rar` file or the whole `results` folder, and Unzip `.rar` files.
2) Download the python evaluation script, `eval_by_AwC_PT_RT_FT.py`.
3) Place the script in any folder for { $\{< k, j >}$ } $_j$ using a specific method, e.g. ``/your path/boreas-2020-11-26-13-58/LiDAR-Iris".
4) Run the python script, then you can get results and related AwC figures.

