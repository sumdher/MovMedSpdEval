# MovMedSpeed Trajectory Segmenter
- Detects low speed segments within a spatiotemporal trajectory (indoor - UWB localization system) and determines the closest Point Of Interest (POI) from the segments' centroids.
- A novel method -- a data processing pipeline that automatically determines unique parameters for each trajectory, within a given set of trajectories. Various visualizations to choose optimal parameters, validated and tested with ground truth.
- Extracts, processes and works on the main feature: _speed_. Applies a filter to mitigate noise: Moving median (on speed), (or, moving average and EMA/EWM).
- Problem: Discrete segmentation of a time-series graph with irregular sampling rate, close to signal processing.

Key libraries (besides the usual):
_[find_peaks](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.find_peaks.html)_ to find the peaks and valleys.
_[KneeLocator](https://pypi.org/project/kneed/)_ to find the elbow points.
_[ipywidgets](https://ipywidgets.readthedocs.io/en/stable/)_ for a Google Colab/Jupyter interface for dynamic visualization.
For more dependencies, please refer to `requirements.txt`

## Strengths:
- Data-driven, adaptive determination of parameters from the data, instead of arbitrarily setting them. (Entropy difference and Jensen-Shannon Divergence)
- Interface for dynamic visualiations (of graphs) to manually determine and define the parameters.
- Comparable results with existing algorithms; sometimes better in some aspects. (See section: "**Results**")
- Fast and robust.

## Weaknesses:
- Unusual behaviour if the data is too noisy.
- Needs POIs' locations to be known in advance (Future work).
- The method works for indoor trajectories with mainly low-speed data, the adaptation to other kinds of data needs to be studied. (Future work)

## IMPLEMENTATION

- An abstract high-level implementation that will do the job and **find the segments** is in the file `find_segments_adaptive_parameters.py`.
- To **visualize the speed graph**, an interactive widget is made for Python noteooks like Jupyter and Colab.
  This is implemented in the file `visualize_segments.py`.
- In the presence of the ground truth, one can also **optimise the parameters** to find the best combination that yields
  the best results to further study the data. This is implemented in `optimize_parameters.py`. 

## Requires:
**Spatiotemporal trajactory data**

Shape: (x, y) coordinates and timestamps, unique trajectory ID for multiple trajectories.

**POI data**

Shape: (x, y) coordinates, unique ID/name.

**[OPTIONAL] Ground truth stops/segments** (Only for evaluation and testing)

Shape: start_time, end_time, (x, y) centroid, closest POI.

## Output:
**Segments**

Shape: start_time, end_time, (x, y) centroid, closest POI (and other things)

# Results

- A novel evaluation criteria were developed by my professors in Univeristy of Milan. Link _[here](https://doi.org/10.1109/PerCom53586.2022.9762404)_.
- It evaluates the stops both quantitatively and qualitatively. **F-score** is the quantitative evaluation and **S-score** is the qualitative aspect - the amount of spatial and temporal overlap with the real stop. **TP** is the number of true positives.
- The table shows the result of the evaluation of the segments extracted from 20 trajectories. Rows highlighted in yellow are the results of this algorithm.
- Row with window = auto is where it adaptively determines the best window size for all trajectories.
- Otherwise, a constant window of 15 for all trajectories was also found to be good.
<img width="989" alt="Group 2" src="https://github.com/sumdher/MovMedSpdEval/assets/26754139/53a9ad55-65b3-4512-bdfa-17e8b43c7338">


# Screengrabs of the Interface

![image](https://github.com/sumdher/MovMedSpdEval/assets/26754139/7d983d04-8dbb-4e41-beef-9d9109d19f02)
![image](https://github.com/sumdher/MovMedSpdEval/assets/26754139/802dacfd-72ad-43a9-a9b3-fea0da9b9105)



