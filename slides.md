<!-- .slide: class="cover" data-background="images/image3.png" data-background-position="right -10% top -80%" data-background-size="auto 70%"> -->

![University of Twente](images/image1.png) <!-- .element: class="plain" -->

## Optimizing wireless video streams for computer vision

### F.J. (Frank) van der Hoek, s1016539

#### Final presentation MSc Assignment

**Committee:**

- dr.ir. J.F. Broenink
- K.H. Russcher, MSc
- dr. M. Poel

---h---

# Contents

1. Introduction
2. Theoretical background
3. Qualitative analysis
4. Test design
5. Results
6. Discussion
7. Conclusions & recommendations

---h---

<!-- .slide: class="chapter" -->

# Introduction

---v---

<!-- .slide: data-background-image="images/politiedrone1.jpg" data-background-color="#000" -->

# Context

![politie drone](images/politiedrone2.jpeg)

---v---

# Problem

<!-- .slide: data-background-image="images/no-artefacts.png" data-background-color="#000" data-transition="slide-in fade-out" -->

---v---

# Problem

<!-- .slide: data-background-image="images/artefacts.png" data-background-color="#000" data-transition="fade-in slide-out" -->

---v---

# Focus

**Multiple solutions:**

1. Replace the wireless connection with a wired connection.
2. Better antenna or other wireless communication hardware.
3. Computer vision on the robot itself.
4. Reduce video data by strategically discarding parts of the data. <!-- .element: class="fragment highlight-current-blue" -->

---v---

# Related work

- H.264 encoder compresses videos for _perceived_ quality by the human visual system, for example, by taking motion into account (Robitza, 2017a)
- Extension to H.264: _SVC improves quality_ (Schierl et al., 2007; Van der Auwera et al., 2008) and _bandwidth utilization_ (Chiang et al., 2008), using _spatial_, _temporal_ and _quality_ scaling
- Humans perceive quality _subjectively_ (e.g., Mannos et al., 1974; Wang et al., 2002)
- Missing: perceived quality by _computer vision_

 <!-- => spatial, temporal, quality scaling => effect on computer vision -->

---v---

# Research questions

**Main research question:**

_How can wireless video streams be optimized for computer vision, when the throughput is limited?_

**Sub questions:**

1. _Can the required throughput of video streams be reduced using spatial, temporal, and quality scaling, such that videos can be streamed reliably over a wireless connection?_

   - Minimum data rate Wi-Fi: 6.5Mbps

2. _How do spatial, temporal, and quality scaling affect computer vision algorithms?_

   - Computer vision using visual tracking

---h---

<!-- .slide: class="chapter" -->

# Theoretical background

---v---

# Pinhole camera model

<center>

![The pinhole camera model](images/pinhole.png)<!-- .element class="plain" style="max-width:70%;" -->

`$$\lambda \mathbf{u} = \mathbf{KRp} + \mathbf{Kt}$$`

</center>

---v---

# Epipolar geometry

<center>

![The pinhole camera model](images/epipolar.png)<!-- .element class="plain" style="max-width:70%;" -->

`$$\lambda_j \mathbf{u}_j = \lambda_i \mathbf{K}\mathbf{R}_j\mathbf{K}^{-1} \mathbf{u}_i + \mathbf{Kt}_j$$`

</center>

---v---

# H.264 encoding

- Block-based
- Inter frame encoding
- Intra frame encoding
- Transform encoding ~ DCT

![Inter frame encoding](images/interframe.png)<!-- .element class="plain" -->

---h---

<!-- .slide: class="chapter" -->

# Qualitative analysis

---v---

# Spatial scaling

![Spatial scaling](images/spatial.png)<!-- .element class="plain" -->

- Data reduction less than linear wrt number of discarded pixels
- Fewer trackable points per frame
- Increases uncertainty in depth estimate

---v---

# Temporal scaling

![Spatial scaling](images/temporal.png)<!-- .element class="plain" -->

- Data reduction less than linear wrt number of discarded frames
- Fewer trackable points between frames
- Increases uncertainty in depth estimate

---v---

# Quality scaling

![Spatial scaling](images/quality.png)<!-- .element class="plain" -->

- Constant rate factor (CRF)
- Data halves for each increment of 6
- Increases uncertainty in depth estimate

---v---

# Combined scaling

- Spatial scaling has the largest impact on throughput, followed by quality scaling and then temporal scaling
- Trade-off between spatial scaling and temporal scaling, dependent on velocity
- Minimum required quality

---h---

<!-- .slide: class="chapter" -->

# Test design

---v---

# Setup

![Experimental setup](images/setup.png)<!-- .element class="plain" -->
`$$\lambda_j \mathbf{u}_j = \lambda_i \mathbf{K}\mathbf{R}_j\mathbf{K}^{-1} \mathbf{u}_i + \mathbf{Kt}_j$$`<!-- .element: class="fragment fade" -->

---v---

<!-- .slide: data-transition="slide-in fade-out" -->

# Visual tracking

## Gradient-based point selection

<center>

![Point selection source](images/highres.png)

</center>

---v---

<!-- .slide: data-transition="fade-in slide-out" -->

# Visual tracking

## Gradient-based point selection

<center>

![Point selection result](images/selectedpoints.png)

</center>

---v---

# Visual tracking - depth estimation

Target number of points to track: 200

---v---

# Datasets

<center>

![RGB-D dataset](images/rgbd.png)<!-- .element class="plain" -->
![CoRBS dataset](images/corbs.png)<!-- .element class="plain" -->

</center>

<small>

| Dataset                 |         Resolution | Frame rate (FPS) | Trajectory rate (Hz) |  Realistic?  |
| ----------------------- | -----------------: | ---------------: | -------------------: | :----------: |
| TUM RGB-D Pioneer robot |   `$640\times480$` |               30 |                  100 |     yes      |
| CoRBS Desk              | `$1920\times1080$` |               30 |                  100 | less, but HD |

</small>

---h---

<!-- .slide: class="chapter" -->

# Results

---v---

# Bit rate

![Spatial scaling bit rate](images/bitrate_spatial.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Temporal scaling bit rate](images/bitrate_temporal.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Quality scaling bit rate](images/bitrate_quality.png)<!-- .element class="plain" style="max-width: 30%;" -->

---v---

# Compression ratio

![Spatial scaling compression](images/compression_spatial.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Temporal scaling compression](images/compression_temporal.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Quality scaling compression](images/compression_quality.png)<!-- .element class="plain" style="max-width: 30%;" -->

---v---

# #Trackable points

![Spatial scaling trackable points](images/trackablepoints_spatial.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Temporal scaling trackable points](images/trackablepoints_temporal.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Quality scaling trackable points](images/trackablepoints_quality.png)<!-- .element class="plain" style="max-width: 30%;" -->
---v---

# #Points in map

![Spatial scaling map points](images/mappoints_spatial.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Temporal scaling map points](images/mappoints_temporal.png)<!-- .element class="plain" style="max-width: 30%;" -->
![Quality scaling map points](images/mappoints_quality.png)<!-- .element class="plain" style="max-width: 30%;" -->

---h---

<!-- .slide: class="chapter" -->

# Discussion

---v---

# Key findings

#### Throughput

- As expected, all 3 types of scaling reduce throughput
- Due to inter frame encoding, increasing frame rate takes little data
- Intra frame encoding is not as effective as inter frame encoding

&nbsp;

#### #Trackable points (potential map density)

- As expected, most impact by spatial scaling
- Limited impact by others

&nbsp;

#### #Map points (uncertainty of depth estimates)

- No visible impact by quality scaling
- Temporal scaling has more impact than spatial scaling

---v---

<!-- .slide: data-transition="slide-in fade-out" -->

# Optimal scaling

![Optimal scaling table](images/optimal.png)<!-- .element class="plain" -->
---v---

<!-- .slide: data-transition="fade-in fade-out" -->

# Optimal scaling

![Optimal scaling table](images/optimal2.png)<!-- .element class="plain" -->
---v---

<!-- .slide: data-transition="fade-in fade-out" -->

# Optimal scaling

![Optimal scaling table](images/optimal3.png)<!-- .element class="plain" -->
---v---

<!-- .slide: data-transition="fade-in fade-out" -->

# Optimal scaling

![Optimal scaling table](images/optimal4.png)<!-- .element class="plain" -->
---v---

<!-- .slide: data-transition="fade-in slide-out" -->

# Optimal scaling

![Optimal scaling table](images/optimal5.png)<!-- .element class="plain" -->

---v---

# Implications & limitations

- Blueprint for manufacturers
- Generalizable to computer vision algorithms based on similar techniques (pixel intensity)
- Further research for exact bit rates for the strategy
- Limited number of tracked points
- Considered uncertainty, not accuracy

---h---

<!-- .slide: class="chapter" -->

# Conclusions & recommendations

---v---

# Conclusions

**Throughput reduction by scaling**

- Enough for at least the lowest Wi-Fi coding and modulation scheme

**Impact on computer vision**

- Spatial scaling: Fewer trackable points, increased uncertainty
- Temporal scaling: Increased uncertainty
- Quality scaling: Almost no impact

**Optimization of wireless video streams**

- Optimal strategy: 1. Quality scaling, 2. Spatial scaling, 3. Temporal scaling
- Can be used as blueprint for manufacturers
- Generalizable to other computer vision algorithms

---v---

# Recommendations

- Analyse bit rates for the strategy
- Implement on actual robot
- Accuracy of generated map / reconstruction
