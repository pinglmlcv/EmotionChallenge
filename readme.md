### Citation
If you use these models or code in your research, please cite:

```
@inproceedings{guo2017multi,
  title={Multi-modality Network with Visual and Geometrical Information for Micro Emotion Recognition},
  author={Guo, Jianzhu and Zhou, Shuai and Wu, Jinlin and Wan, Jun and Zhu, Xiangyu and Lei, Zhen and Li, Stan Z},
  booktitle={Automatic Face \& Gesture Recognition (FG 2017), 2017 12th IEEE International Conference on},
  pages={814--819},
  year={2017},
  organization={IEEE}
}

@article{guo2018dominant,
  title={Dominant and Complementary Emotion Recognition from Still Images of Faces},
  author={Guo, Jianzhu and Lei, Zhen and Wan, Jun and Avots, Egils and Hajarolasvadi, Noushin and Knyazev, Boris and Kuharenko, Artem and Jacques, Julio CS and Bar{\'o}, Xavier and Demirel, Hasan and others},
  journal={IEEE Access},
  year={2018},
  publisher={IEEE}
}
```

### For final evaluation

#### Submission
First, you should generate the crop and aligned data on test Chanllenge dataset. Just change to `crop_align` dir
```
python landmark.py
```
The crop and aligned of test data(final evaluation phase data) of 224x224 will place in `$ROOT/data/face_224`

Then change to `cnn` dir, just type
```
python extract.py
```
It will load data preprocessed and caffe model to generate labels named `predictions.txt` and `predictions.zip` for test data. All details were considered.

In this repo, some directory path may be confused, just be careful, contact me if any questions occured.

**The trained caffe model is just a experiement model, it may not has the best perfomance in this challenge.**
 
Just upload `predictions.zip` to submit window then.

### Introduction
We use [Dlib](https://github.com/davisking/dlib) to do face and landmark detection, and use landmark to do face cropping and alignment, then we use [Caffe](https://github.com/BVLC/caffe) to with landmark and cropping image to train a cnn model to do the face expression recognition task.

### Pipline
#### Preproces
First, run `landmark.py` to get all the origin image landmark, then build the `crop_align` binary, and run `crop_align.py` to get all the 224x224 size image.

**Build crop_align**
 ```
 cd crop_align
 mkdir build
 cd build
 cmake ..
 make
 ```

All the preprocessed data except the images are in data dir.

#### Training
Change to cnn dir, run `prepare_data.py` to prepare training, validation and test data. Then run `train_val.sh` to start training.

#### Extract(Test)
Just run `extract.py` to generate the result, the input is the test image and its landmark offset info.

### Method
We use the landmark offset and image info to do this task. In detail, the landmark offset is calculated by substraction of 224x224 image landmark and each id's mean landmark, and we concact this feature to modified alexnet's last output feature. We change softmax loss to hinge loss to get a little better result.
  
  More detail is in the `fact_sheet.tex`.
