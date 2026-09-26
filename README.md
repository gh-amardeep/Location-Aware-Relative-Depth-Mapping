# 📍 Location-Aware Relative Depth Mapping

<p align="center">
  <b>Classical Computer Vision • Stereo Vision • Relative Depth • Multi-View Image Processing</b>
</p>

A classical computer-vision pipeline for estimating and visualizing **relative depth** from a sequence of horizontally captured images. The project combines **ORB feature matching, homography-based image alignment, StereoSGBM disparity estimation, relative-depth normalization, and multi-view depth fusion** using Python, OpenCV, and NumPy.

---

## 🚀 Project Overview

This project explores how multiple overlapping 2D images can be processed to obtain useful **relative depth information** without relying on a deep-learning depth-estimation model.

The pipeline uses **9 horizontally captured images**, with **I5 selected as the reference image**.

The overall workflow is:

```text
9 Horizontally Captured Images
              │
              ▼
        Image Ordering
              │
              ▼
     ORB Feature Detection
              │
              ▼
       Feature Matching
              │
              ▼
      Homography Estimation
              │
              ▼
       Image Alignment
              │
              ▼
        StereoSGBM
              │
              ▼
        Disparity Maps
              │
              ▼
       Relative Depth
              │
              ▼
    Transform Depth → I5
              │
              ▼
       Multi-View Fusion
              │
              ▼
 Location-Aware Relative Depth
              │
              ├───────────────┐
              ▼               ▼
       Pixel Location      3D Visualization
           Query
```

---

## ✨ Key Features

- 📷 Multi-view image processing using 9 horizontally captured images
- 🔍 ORB-based feature detection and matching
- 🧭 Homography estimation for geometric alignment
- 🪞 Stereo correspondence using OpenCV StereoSGBM
- 📊 Disparity-map generation
- 📐 Relative-depth estimation
- 🔄 Multi-view depth transformation into the I5 reference frame
- 🧩 Fusion of depth information from multiple views
- 📍 Location-aware pixel depth querying
- 🖥️ 3D visualization of relative depth
- 🐍 Python + OpenCV + NumPy implementation
- 🚫 No deep-learning model required

---

## 🧠 Core Idea

Stereo vision provides an important relationship between **disparity** and **depth**:

```text
Higher Disparity
      ↓
Closer Object

Lower Disparity
      ↓
Farther Object
```

Instead of converting disparity into absolute physical distance, this project normalizes the estimated depth information into a **relative depth value**.

The normalized relative-depth representation is:

```text
Zr = (d - dmin) / (dmax - dmin)
```

where:

- `d` = estimated disparity/depth-related value
- `dmin` = minimum value in the valid range
- `dmax` = maximum value in the valid range
- `Zr` = normalized relative-depth value

The resulting values are clipped to:

```text
0 ≤ Zr ≤ 1
```

This project therefore provides **relative depth**, rather than calibrated metric distance.

---

# 🖼️ Input Data

The project works with a sequence of 9 horizontally captured images:

```text
I1  I2  I3  I4  I5  I6  I7  I8  I9
                 ↑
            Reference
```

The central image **I5** is used as the reference coordinate system.

The neighboring views provide overlapping information that can be used for stereo matching and depth estimation.

---

# 🔍 1. ORB Feature Detection

The first stage extracts distinctive visual features from the images using **ORB (Oriented FAST and Rotated BRIEF)**.

Conceptually:

```text
Input Image
     ↓
ORB Keypoint Detection
     ↓
ORB Descriptors
     ↓
Feature Matching
```

These features allow corresponding points to be identified between overlapping images.

---

# 🔗 2. Feature Matching

Features between neighboring images are matched to identify corresponding visual structures.

The matched feature points are then used to estimate the geometric relationship between the views.

This provides the correspondence information required for subsequent image alignment.

---

# 🧭 3. Homography Estimation

A homography is estimated from matched feature points to align one image with another.

Conceptually:

```text
Image A
   │
   │ Feature Matches
   ▼
Matched Points
   │
   ▼
Homography H
   │
   ▼
Aligned Image
```

The transformation is used to bring information from different views into a common coordinate frame.

---

# 🪞 4. StereoSGBM Disparity Estimation

After the appropriate image alignment, the project uses OpenCV's:

```python
cv2.StereoSGBM_create()
```

to estimate disparity.

The output is a disparity map representing the pixel-wise correspondence between stereo views.

```text
Left / Reference View
          +
     Right View
          ↓
      StereoSGBM
          ↓
    Disparity Map
```

---

# 📊 5. Relative Depth Mapping

The disparity/depth information is normalized to create a relative-depth representation.

The project does not claim to recover physical distance in meters.

Instead, the output answers the more general question:

```text
Which regions are relatively closer?
Which regions are relatively farther?
```

This makes the approach useful for visualization and spatial interpretation even without a calibrated camera baseline or absolute depth scale.

---

# 🔄 6. Multi-View Depth Fusion

Depth information obtained from different image pairs is transformed into the coordinate system of the reference image **I5**.

The overall concept is:

```text
I1 ──┐
I2 ──┤
I3 ──┤
I4 ──┤
     ├──► Reference Frame I5
I6 ──┤
I7 ──┤
I8 ──┤
I9 ──┘
             │
             ▼
      Multi-View Fusion
             │
             ▼
   Final Relative Depth Map
```

This allows depth information from multiple viewpoints to contribute to a common reference representation.

---

# 📍 7. Location-Aware Depth Query

One of the key interactive features of the project is the ability to query a specific pixel location in the reference image.

A location can be represented as:

```text
(u, v)
```

where:

- `u` = horizontal pixel coordinate
- `v` = vertical pixel coordinate

The query returns information such as:

```text
Reference Image : I5
Pixel Location  : (u, v)
Relative Depth  : Zr
Interpretation  : NEAR / MIDDLE / FAR
```

This transforms the depth map from a visualization into a simple **location-aware analysis tool**.

---

# 🧊 8. 3D Relative-Depth Visualization

The final result can also be visualized in 3D.

The visualization uses:

```text
X → horizontal pixel coordinate
Y → vertical pixel coordinate
Z → normalized relative depth
```

The resulting plot provides an intuitive view of how relative depth varies across the image.

> **Important:** This visualization represents pixel coordinates combined with normalized relative depth. It is not a calibrated physical 3D reconstruction.

---

# 📊 Results

The repository can include selected outputs from the notebook.

Recommended result gallery:

```text
output-images/
├── input-images-overview.png
├── feature-matching.png
├── disparity-maps.png
├── relative-depth-maps.png
├── final-relative-depth.png
├── queried-location.png
└── relative-depth-3d.png
```

### Example Results Section

```markdown
## 📊 Results

### Multi-View Input

![Input Images](output-images/input-images-overview.png)

### Disparity Estimation

![Disparity Maps](output-images/disparity-maps.png)

### Relative Depth

![Relative Depth](output-images/final-relative-depth.png)

### 3D Relative-Depth Visualization

![3D Visualization](output-images/relative-depth-3d.png)
```

---

# 🧩 Complete Processing Pipeline

```text
                MULTI-VIEW IMAGES
                       │
                       ▼
              Image Preprocessing
                       │
                       ▼
                ORB Features
                       │
                       ▼
               Feature Matching
                       │
                       ▼
              Homography Estimation
                       │
                       ▼
                Image Alignment
                       │
                       ▼
                 StereoSGBM
                       │
                       ▼
                 Disparity
                       │
                       ▼
              Relative Depth
                       │
                       ▼
          Transform to Reference I5
                       │
                       ▼
               Depth Fusion
                       │
                       ▼
          ┌────────────┴────────────┐
          ▼                         ▼
   Pixel Depth Query         3D Visualization
          │
          ▼
   Near / Middle / Far
```

---

# 🛠️ Technologies Used

### Programming Language

- Python

### Libraries

- OpenCV
- NumPy
- Matplotlib
- Jupyter Notebook

### Computer Vision Techniques

- ORB feature detection
- Feature descriptor matching
- Homography estimation
- Stereo correspondence
- StereoSGBM
- Disparity estimation
- Relative-depth normalization
- Multi-view depth fusion
- 3D visualization

---

# 📁 Project Structure

```text
Location-Aware-Relative-Depth-Mapping/
│
├── README.md
│
├── Location_Aware_Relative_Depth_Mapping.ipynb
│
├── images/
│   ├── I1.jpg
│   ├── I2.jpg
│   ├── I3.jpg
│   ├── I4.jpg
│   ├── I5.jpg
│   ├── I6.jpg
│   ├── I7.jpg
│   ├── I8.jpg
│   └── I9.jpg
│
└── output-images/
    ├── input-images-overview.png
    ├── disparity-maps.png
    ├── relative-depth-maps.png
    ├── final-relative-depth.png
    ├── queried-location.png
    └── relative-depth-3d.png
```

---

# ▶️ How to Run

### 1. Clone the repository

```bash
git clone <YOUR-REPOSITORY-URL>
cd Location-Aware-Relative-Depth-Mapping
```

### 2. Install dependencies

```bash
pip install numpy opencv-python matplotlib jupyter
```

### 3. Verify the input images

Place the 9 input images in:

```text
images/
```

### 4. Open the notebook

```bash
jupyter notebook
```

Then open:

```text
Location_Aware_Relative_Depth_Mapping.ipynb
```

### 5. Run the notebook sequentially

Execute the cells in order to:

1. Load the multi-view images
2. Detect ORB features
3. Match features
4. Estimate homographies
5. Align views
6. Estimate disparity
7. Generate relative depth
8. Transform depth information to I5
9. Fuse multi-view information
10. Query pixel locations
11. Visualize the relative-depth result

---

# 🎯 Learning Outcomes

This project provides practical experience with:

- Classical computer vision
- Feature-based image matching
- ORB descriptors
- Homography and projective geometry
- Stereo vision
- Disparity estimation
- Relative depth estimation
- Multi-view geometry
- Image coordinate transformations
- Depth visualization
- Interactive pixel-level analysis

---

# ⚠️ Limitations

This project estimates **relative depth**, not absolute physical distance.

The final depth values depend on:

- Image overlap
- Feature quality
- Stereo correspondence
- Scene texture
- Homography accuracy
- StereoSGBM parameters
- Camera geometry

Without camera calibration, known baseline, and appropriate intrinsic parameters, the output should not be interpreted as metric depth in meters.

The 3D visualization is therefore intended primarily for **relative spatial interpretation and visualization**.

---

# 🔬 Possible Future Improvements

Potential extensions include:

- 📷 Camera calibration using a checkerboard
- 📏 Metric depth estimation using a known stereo baseline
- 🧠 Comparison with deep-learning depth models
- 🎯 Improved feature matching with SIFT or modern feature methods
- 🧩 More robust multi-view depth fusion
- 🎥 Extension from static images to video
- ⚡ Real-time depth estimation
- 🗺️ Dense 3D reconstruction with calibrated camera geometry

---

# 💡 Why This Project?

The project demonstrates that useful spatial information can be extracted from ordinary images using a **classical computer-vision pipeline**, without depending on a trained neural network.

It combines several important concepts:

```text
Feature Matching
       +
Projective Geometry
       +
Stereo Vision
       +
Disparity Estimation
       +
Relative Depth
       +
Multi-View Fusion
       ↓
Location-Aware Depth Mapping
```

---

## ⭐ Project Highlights

```text
📷 9 Multi-View Images
🔍 ORB Feature Matching
🧭 Homography Alignment
🪞 StereoSGBM
📊 Disparity Maps
📐 Relative Depth
🔄 Multi-View Fusion
📍 Pixel Location Query
🧊 3D Visualization
🐍 Python
👁️ OpenCV
```

---

## 👨‍💻 Author

**AmarDeep Dwivedi**

M.Tech  
Electrical Engineering — CSPML  
IIT Dharwad

---

## 📜 License

This project is intended for educational and research purposes.

---

<p align="center">
  <b>📍 From Pixels → Disparity → Relative Depth → Spatial Understanding</b>
</p>
