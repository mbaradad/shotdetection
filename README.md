# Shot detection
This repo provides code to perform video shot detection and extract middle, beginning and end frames for each shot.

## Dependencies
The most robust way to run the code (with reproducible results) is getting certain specific versions of the required libraries. For example, different ffmpeg versions may drop different frames at the start of the video (producing different results if different versions are use). Specifically, to install the exact versions of the dependencies you can create a conda environment as:  
``` 
conda create --name shot_detect python=3.6
conda activate shot_detect
conda install -c conda-forge opencv=3.1.0
# cuda is required for compiling with opencv
conda install cudatoolkit=9.0 -c pytorch
conda install av==0.4.0 -c conda-forge
conda install ffmpeg==2.8.6 -c conda-forge
```

The code has been tested om python 3.6

## How to run
Compile using
```
cmake .
make
```

Extract the movie shots running
```
sh run_shot_detection.sh {input_movie_file} {output_folder}
```

The code will create a folder `{output_folder}/{input_movie_file}` containing the start, middle and last frame for every shot and a file `{output_folder}/{input_movie_file}/{input_movie_file}.videvents` with the frame and timestamp of every start of shot.

You can test the code using a test video as
```
wget http://wednesday.csail.mit.edu/frames/tools/video_tools/test_vid/short.mp4
mkdir output
sh run_shot_detection.sh short.mp4 output/
```

## How it works
Extracts the movie .dfd file, capturing the difference between motion compensated frames.
```
./ShotDetection input_file.mp4 output_file.dfd
```
Creates mat_index containing frame-number and timestamp mappings and saves into some base dir
```
python mat_index.py --video_fname input_file.mp4 --base_dir out_dir
```
Create file with shot changes from .dfd and save into a base dir
```
python dfd_to_videoevents.py --base_dir out_dir --dfd_path output_file.dfd
```
Given the shot changes, dump the first middle and last frame for every shot.
```
python dump_frames_of_shot.py --video_fname input_file.mp4 --dfd_path output_file.dfd --base_dir "$out_dir"
```

## Tips and tricks
If you want to enable parallel processing (of block matching) with OPENMP, please add `-fopenmp` to the `CMAKE_CXX_FLAGS`.

## Licence and citations
The code is based on `CVHCI/Okapi` library for processing of images.
