# Bingchen-Tutu-NFL-Prediction

### Helmet Mapping + Deepsort

In this notebook:
1. I introduce a package `helmet_assignment` which includes some helper code.
2. I show how `deepsort` can be applied to post process exsiting predictions.

Postprocessing is applied to the baseline predictions from [this amazing starter notebook](https://www.kaggle.com/its7171/nfl-baseline-simple-helmet-mapping).


### "helmet_assignment" package provides helper code.

This package includes helpful functions like`NFLAssignmentScorer`, `check_submission` and `add_track_features`.

- dataset: https://www.kaggle.com/robikscube/helmet-assignment-helpers
- github: https://github.com/RobMulla/helmet-assignment


### Baseline helmet mapping
This section uses the simple helmet mapping approach from the awesome notebook:

https://www.kaggle.com/its7171/nfl-baseline-simple-helmet-mapping


#### Settings and loading data

Note I've extracted `max_iter`, `DIG_STEP` and `DIG_MAX` to the top for easy experimentation. I've also modified the code to run in debug mode if running on the public test set.


#### Score the predictions before applying deepsort postprocessing

The scores are roughly ~0.3, which is similar to the public leaderboard.


#### Deepsort Postprocessing

Deepsort is a popular framework for object tracking within video. 
- [This blog post](https://nanonets.com/blog/object-tracking-deepsort/
) shows some examples of it being put to use.
- This notebook shows how to apply deepsort to this helmet dataset: https://www.kaggle.com/s903124/nfl-helmet-with-yolov5-deepsort-starter
- You can also read the paper for deepsort here: https://arxiv.org/pdf/1703.07402.pdf

The approach is fairly simple:
1. Step through each frame in a video and apply the deepsort algorithm. This clusters helmets across frames when it is the same player/helmet.
2. Group by each of these deepsort clusters - and pick the most common label for that cluster. Then override all of the predictions for that helmet to the same player.


#### Importing Deepsort from dataset
Because your submission is not allowed to use internet access, you can reference the deepsort codebase from the attached dataset. Deepsort also has a dependency of `easydict` which I've also added as a dataset.


#### Deepsort config

Deepsort uses a config yaml file for some settings. These are just the default configs and could be improved.


#### Functions to apply deepsort to helmet boxes.

Below are two functions `deepsort_helmets` which runs deepsort across a video. There is a lot of room for improving this function. The merging of deepsort labels onto the original helmet boxes is currently done in a very crude manner.

`add_deepsort_label_col` mapps the most common label to each deepsort cluster.


#### Apply Deepsort to Baseline Predictions


### Check Submission & Save
Finally we will create a submission file and check that it passes the submission requirements.
The steps are:
1. Drop the `label` and replace with `label_deepsort` predictions.
2. Remove any duplicate labels within a single video/frame. This is required to meet the submission requirements.
3. Save the results.


### Display video showing predictions

Lastly, if we want to review our predictions we can create a video to review the predictions using the `video_with_predictions` function from the `helmet_assignment` helper package.
