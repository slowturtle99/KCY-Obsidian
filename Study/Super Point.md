up::
tags:: #AI #AI/ComputerVision 
# SuperPoint: Self-Supervised Interest Point Detection and Description

[[Super Point Paper]]

![[Pasted image 20240117161703 1.png]]

## Overview
- CNN network for Interest point detection and description
- Self supervised training framework

## Super Point Architecture
![[Pasted image 20240117141452 1.png]]
### Shared Encoder
- VGG-style encoder
- $\mathbb{R}^{H \times W} \rightarrow \mathbb{R}^{H_c \times W_c \times F}$ where $H_c = H/8, W_c = W/8$

### Interest Point Detector Decoder
![[Pasted image 20240117142028 1.png]]
- Decoder head : $\mathbb{R}^{H_c \times W_c \times F} \rightarrow \mathbb{R}^{H_c \times W_c \times 65}$
	- Each output cell responsible for local 8x8 region
	- 65 channel = 64 locations + 1 dustbin for "no interest point"
- Make dense heatmap for interest points
	- Channel wise [[Softmax]]
	- Remove dustbin : $\mathbb{R}^{H_c \times W_c \times 65} \rightarrow \mathbb{R}^{H_c \times W_c \times 64}$
	- Reshape: $\mathbb{R}^{H_c \times W_c \times 64} \rightarrow \mathbb{R}^{H \times W}$

### Descriptor Decoder
![[Pasted image 20240117142522 1.png]]
- Desciptor decoder head: $\mathbb{R}^{H_c \times W_c \times F} \rightarrow \mathbb{R}^{H_c \times W_c \times D}$
	- Model similar to Universal Correspondence Network
- Output a dense map of descriptors: $\mathbb{R}^{H_c \times W_c \times D} \rightarrow \mathbb{R}^{H \times W \times D}$
	- Bi-cubic interpolation
	- L2 Normalize

## How to Train Super Point

### Training Data
Image $I \in \mathbb{R}^{H \times W}$ , Key point label $Y \in \mathbb{R}^{H_c \times W_c \times 65}$
Warped image $I' \in \mathbb{R}^{H \times W}$, Warped key point label $Y' \in \mathbb{R}^{H_c \times W_c \times 65}$
Keypoint matching $S$

### Loss function
$\mathcal{L}\left(\mathcal{X}, \mathcal{X}^{\prime}, \mathcal{D}, \mathcal{D}^{\prime} Y, Y^{\prime}, S\right) = \quad \mathcal{L}_p(\mathcal{X}, Y)+\mathcal{L}_p\left(\mathcal{X}^{\prime}, Y^{\prime}\right)+\lambda \mathcal{L}_d\left(\mathcal{D}, \mathcal{D}^{\prime}, S\right)$

$\mathcal{X}, \mathcal{X}' \in \mathbb{R}^{H_c \times W_c \times 65}$ : Detector output
$\mathcal{D}, \mathcal{D}'\in \mathbb{R}^{H_c \times W_c \times D}$ : Descriptor output

- Detector Loss for image
- Detector Loss for warped image
- Descriptor Loss for image pair
- Weight
#### Loss Function for Interest Point Detector
$\mathcal{L}_p(\mathcal{X}, Y)=\frac{1}{H_c W_c} \sum_{\substack{h=1 \\ w=1}}^{H_c, W_c} l_p\left(\mathbf{x}_{h w} ; y_{h w}\right)$
$l_p$: cross entropy function
- Target: $\mathcal{X} \rightarrow Y$
#### Loss Function for Descriptor
$\begin{aligned} \mathcal{L}_d\left(\mathcal{D}, \mathcal{D}^{\prime}, S\right) & = \\ & \frac{1}{\left(H_c W_c\right)^2} \sum_{\substack{h=1 \\ w=1}}^{H_c, W_c} \sum_{\substack{h^{\prime}=1 \\ w^{\prime}=1}}^{H_c, W_c} l_d\left(\mathbf{d}_{h w}, \mathbf{d}_{h^{\prime} w^{\prime}}^{\prime} ; s_{h w h^{\prime} w^{\prime}}\right),\end{aligned}$

where, 
$\begin{array}{r}l_d\left(\mathbf{d}, \mathbf{d}^{\prime} ; s\right)=\lambda_d * s * \max \left(0, m_p-\mathbf{d}^T \mathbf{d}^{\prime}\right) +(1-s) * \max \left(0, \mathbf{d}^T \mathbf{d}^{\prime}-m_n\right) \end{array}$
and
$s_{h w h^{\prime} w^{\prime}}= \begin{cases}1, & \text { if }\left\|\widehat{\mathcal{H} \mathbf{p}_{h w}}-\mathbf{p}_{h^{\prime} w^{\prime}}\right\| \leq 8 \\ 0, & \text { otherwise }\end{cases}.$

- matching pair -> same description
- non matching pair -> different description
### Synthetic Pre-Training
![[Pasted image 20240117144648 1.png]]
- Synthetic Shapes dataset consisting of rendered triangles, quadrilaterals, lines, cubes, checkerboards
- Ground truth corner locations
- Supervised learning
- Not real world data

### Self-Supervised Training
### Why we need Self-Supervised Training Method
- To use unlabeled real-world dataset
- Using SIFT, ORB, SURF for labeling? -> Performance is limited by original algorithm

#### Homographic Adaptation
![[Pasted image 20240117144614 1.png]]
- Generate pseudo ground truth interest point locations
	  $\hat{F}\left(I ; f_\theta\right)=\frac{1}{N_h} \sum_{i=1}^{N_h} \mathcal{H}_i^{-1} f_\theta\left(\mathcal{H}_i(I)\right)$

- Random homography generation![[Pasted image 20240117151620 1.png]]

- Iterative Homographic Adaptation
	-> Pseudo ground truth data generation with base detector
	-> Train detector with generated dataset
	-> Update base detector


## Result
![[Pasted image 20240117151933 1.png]]
