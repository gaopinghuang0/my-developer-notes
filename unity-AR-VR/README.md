## Unity

### Basic Tutorials
* [FPS Microgame](https://learn.unity.com/project/fps-template)

### Best Practices
* [Unity3D Best Practices (2013)](http://www.glenstevens.ca/unity3d-best-practices/). Some of them are outdated, but things related to prefabs are inspiring:
  * Use prefabs for everything, even unique game objects should be prefabs.
  * Use separate prefabs for specialization; do not specialize instances. 
    * Watch this video: [Improved Prefab Workflows: Nested Prefabs, Prefab Mode and Prefab Variants (2018)](https://www.youtube.com/watch?time_continue=19&v=ibmdm_PoyMA&feature=emb_logo).
  * Link prefabs to prefabs; do not link instances to instances.

### VR Tutorials
* VRTK. [Advanced VR Interactions in Unity Tutorial (Mar 2019) by Eric Van de Kerckhove](https://www.raywenderlich.com/2163461-advanced-vr-interactions-in-unity-tutorial). Bug (6/27/2020): When entering Unity play mode, the Unity game view is working properly, but Oculus Rift S is stuck in Unity Loading screen (similar to this [issue](https://www.reddit.com/r/oculus/comments/a2jg2f/cannot_load_into_unity_play_mode/)). 
* [Design, Develop, and Deploy for VR - Unity Learn](https://learn.unity.com/course/oculus-vr?uv=2018.4)
  * Identify target audience. Build out an ideal user profile.

### Scriptable Object
* Video [Unite 2016 - Overthrowing the MonoBehaviour Tyranny in a Glorious Scriptable Object Revolution](https://www.youtube.com/watch?v=6vmRwLYWNRo)
* Video [Introduction to Scriptable Objects - February 2016 Live Training](https://learn.unity.com/tutorial/introduction-to-scriptable-objects#5cf187b7edbc2a31a3b9b123)
* Excellent video [Unite Austin 2017 - Game Architecture with Scriptable Objects - YouTube](https://www.youtube.com/watch?v=raQ3iHhE_Kk&feature=youtu.be)

### Coroutines
* [Unity Coroutines (May 2018)](http://www.theappguruz.com/blog/how-to-use-coroutines-in-unity)


## AR algorithms

### Feature points and plane detection
* [ ] [SIFT (Scale Invariant Feature Transform)](https://link.springer.com/article/10.1023/B:VISI.0000029664.99615.94) 2004 by David G. Lowe.
* [ ] [SURF (Speeded Up Robust Features)](https://link.springer.com/chapter/10.1007/11744023_32) 2006 by H. Bay et al.
* [ ] [BRISK (Binary Robust Invariant Scalable Keypoints)](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/43288/eth-7684-01.pdf?sequence=1) 2011. It can provide better overall performance than SIFT and SURF. It is often based on [FAST algorithm (2006)](https://link.springer.com/chapter/10.1007/11744023_34) by Rosten and Drummond.
  * FAST uses the circular surroundings of each pixel `p` (e.g., 16 pixels in a circle around `p`) to detect a corner. If a certain number of connected pixels are brigher or darker than the central pixel `p`, then the algorithm found a corner.
  * In BRISK, at least 9 consecutive pixels in the 16-pixel circle must be sufficiently brighter or darker than the central pixel `p`. In addition, BRISK also uses down-sized images (scale-space pyramid) to achieve better invariance to scale.
  * *Keypoints*: the algorithm must find the feature again in a different image (with different perspective, lightning, etc). BRISK descriptor is a binary string with 512 bits, which is a concatenation of brightness comparison results between different samples surrouding the center keypoint.
* A blog post: [Basics of AR: Anchors, Keypoints & Feature Detection (Aug 2018)](https://www.andreasjakl.com/basics-of-ar-anchors-keypoints-feature-detection/) describes a little more about BRISK.
  * It also includes code of Python + OpenCV to visualize the feature points in an image.

### Light estimation

### SLAM
* [SLAM (Simultaneous Localization and Mapping)](https://en.wikipedia.org/wiki/Simultaneous_localization_and_mapping)
* [ ] EM algorithm
* [ ] Kalman filters
* [ ] Particle filters (aka. Monte Carlo methods).