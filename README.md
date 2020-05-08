# Road Fault Identification (DeepSegmentor)
A Pytorch implementation of DeepCrack and RoadNet projects for Thesis, 2020. 

### 1.Datasets

 - [Crack Detection Dataset](https://www.sciencedirect.com/science/article/pii/S0925231219300566)
 - [Multi-task Road Detection Dataset](https://ieeexplore.ieee.org/document/8506600)

### 2.Installation

We provide a user-friendly configuring method via [Conda](https://docs.conda.io/en/latest/) system, and you can create a new Conda environment using the command:

```
conda env create -f environment.yml
```

### 3.Balancing Weights

We follow the [Median Frequency Balancing](https://arxiv.org/pdf/1411.4734.pdf) method, using the command:
```
python3 ./tools/calculate_weights.py --data_path <path_to_segmentation>
```

### 4.Training

Before the training, please download the dataset and copy it into the folder `datasets`. 

 - Crack Detection

```
sh ./scripts/train_deepcrack.sh <gpu_id>
```
 - Road Detection

 ```
sh ./scripts/train_roadnet.sh <gpu_id>
```

We provide our pretrained models here:

|Model|Google Drive|
|:----|:----:|
|DeepCrack|[[link]](https://drive.google.com/file/d/1URcYt9MwdpYxvb5HeN-_1vLJLOnAPNh7/view?usp=sharing)|
|RoadNet|[[link]](https://drive.google.com/file/d/1_hWjVGXfaJosfZHUvtJ8E6Dua2zO8iMB/view?usp=sharing)|

### 5.Testing

 - Crack Detection

```
sh ./scripts/test_deepcrack.sh <gpu_id>
```
|Image|Ground Truth|GF|fused|side1|side2|side3|side4|side5|
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----|
|![](./figures/deepcrack/11194_image.png)|![](./figures/deepcrack/11194_label_viz.png)|![](./figures/deepcrack/11194_gf.png)|![](./figures/deepcrack/11194_fused.png)|![](./figures/deepcrack/11194_side1.png)|![](./figures/deepcrack/11194_side2.png)|![](./figures/deepcrack/11194_side3.png)|![](./figures/deepcrack/11194_side4.png)|![](./figures/deepcrack/11194_side5.png)|

[[See more examples >>>]](./figures/deepcrack.md)

 - Road Detection

```
sh ./scripts/test_roadnet.sh <gpu_id>
```
|Image|Ground Truth|Prediction|
|:----:|:----:|:----:|
|![](./figures/roadnet/1-14-5_image.png)|![](./figures/roadnet/1-14-5_label_gt.png)|![](./figures/roadnet/1-14-5_label_pred.png)|

[[See more examples >>>]](./figures/roadnet.md)

### 6.Evaluation

 - Metrics (appeared in our papers):

 |Metric|Description|Usage|
 |:----:|:-----|:----:|
 |P|Precision, `TP/(TP+FP)`|segmentation|
 |R|Recall, `TP/(TP+FN)`|segmentation|
 |F|F-score, `2PR/(P+R)`|segmentation|
 |TPR|True Positive Rate, `TP/(TP+FN)`|segmentation|
 |FPR|False Positive Rate, `FP/(FP+TN)`|segmentation|
 |AUC|The Area Under the ROC Curve|segmentation|
 |G|Global accuracy, measures the percentage of the pixels correctly predicted|segmentation|
 |C|Class average accuracy, means the predictive accuracy over all classes|segmentation|
 |I/U|Mean intersection over union|segmentation|
 |ODS|the best F-measure on the dataset for a fixed scale|edge,centerline|
 |OIS|the aggregate F-measure on the dataset for the best scale in each image|edge,centerline|
 |AP|the average precision on the full recall range|edge,centerline|

 **Note**: If you want to apply the standard non-maximum suppression (NMS) for edge/centerline thinning. Please see more details in [Piotr's Structured Forest matlab toolbox](https://github.com/pdollar/edges) or some helper functions provided in the [hed/eval](https://github.com/s9xie/hed_release-deprecated/tree/master/examples/eval).

[[See more details (*Evaluation* + *Guided Filter* + *CRF*) >>>]](./eval/README.md)

Usage:

```
cd eval
python eval.py --metric_mode prf --model_name deepcrack --output deepcrack.prf
```

[[Display the accuracy curves >>>]](./plots)

### Acknowledgment

This code is based on the [pytorch-CycleGAN-and-pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix). 
Thanks to the contributors of this project! Saved a lot of time in implementation!!


