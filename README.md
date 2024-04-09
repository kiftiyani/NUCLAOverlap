# Joint-wise Action Decomposition by Multi-action Recognition
## Dataset Description
This work utilize N-UCLA[[1]](#1) dataset that provides 3D human skeleton data captured by Kinect camera. The N-UCLA dataset comprises of 1494 samples of 10 action classes: pick up with one hand, pick up with two hand, drop trash, walk around, sit down, stand up, donning, doffing, throw, and carry.
From the existing class of N-UCLA[[1]](#1), we pair combinations of two actions and select the most realistic combination based on our judgment and the appearance of the data. 
For instance, since the "walk" class is predominantly performed by the lower part of the body, it would be realistic to merge it with an action primarily performed by the upper body. 
Due to the limitation of the class label and the importance of realistic combinations, this study only merges pairs of two classes.
List of merged classes:
|Class composition| Action Name|
|-----------------|------------|
|4, 3 | Walk around and drop trash|
|4, 9 | Walk around and throw|
|4, 10 | Walk around and carry|
|5, 7 | Sit down and donning|
|5, 8 | Sit down and doffing|
|5, 10 | Sit down and carry|
|6, 7 | Stand up and donning|
|6, 8 | Stand up and doffing|
|6, 9 | Stand up adn throw|
|6, 10 | Stand up and carry|


<figure>
  <img src="Figure/sample.jpg" alt="">
  <figcaption>Multi-action label example from merged action sit down and doffing.</figcaption>
</figure>

## Merging Process
Initially, we partition the body into six parts that are most likely to exhibit distinct movements: head, middle body, left arm, right arm, left leg, and right leg. We calculate the average motion of each body part to define the class label. 
For example, in the figure above, for the combination of action sit down and doffing, the $\color{red}red \space X$ in $8^{th}$ and $33^{rd}$ frame indicate joint with label sit down and $\color{blue}blue \space circle$ indicate joint with label doffing. 
We assign this clear label ($\color{yellow}most \space yellow$ regions or $\color{purple}most \space dark \space purple$ regions in the left part of the figure above) if the average motion of the corresponding body part in one sample label is greater than the other sample label if the difference between these two and the average motions exceeds a threshold $\tau$=0.01. 
If the difference between the average motion of two corresponding body parts is less than the threshold, we assign each joint label of this particular body part as weighted label based on the motion value for each joint, demonstrate with $\color{green}green \space triangle$ in the figure. 
We sample a maximum of 200 samples for each combination, resulting in a total of 1983 samples.

## Resources
The data presented in JSON files contains an array of skeletons and labels. Each JSON file organized as follows:
```
{
  "file_name": "a003_a002_i13_i38",
  "skeletons": [[
                  [-0.7204612493515015, -0.2564444839954376, 1.1690807342529297],
                  ...
                ]],
  "label": [[
             [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]],
             ...
            ]]
}
```
the number after `a` in the file names identify the action label involve from the original N-UCLA dataset (start from 0). and the number after `i` indentify the data index for the merging.

Download the dataset [here](https://drive.google.com/file/d/1LiZotTZS3L7FugxDdtOIsrvduVvXR76o/view?usp=sharing)

## References
<a id="1">[1]</a>
Wang, J., Nie, X., Xia, Y., Wu, Y., Zhu, S.: Cross-view action modeling, learning, and recognition. In: 2014 IEEE Conference on Computer Vision and Pattern Recognition (CVPR). pp. 2649–2656. IEEE Computer Society, Los Alamitos, CA, USA ([jun 2014](https://doi.org/10.1109/CVPR.2014.339)). 
