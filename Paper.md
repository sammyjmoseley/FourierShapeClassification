--- 
title: Fourier Analysis Algorithm for Identifying Shapes
author: Sammy Moseley
date: 12/12/2016
header-includes: 
    - \usepackage{natbib}
    - \usepackage{graphicx}
    - \usepackage{subfig}
    - \usepackage{amsmath}
    - \usepackage{float}
    - \usepackage{mathrsfs}

output:
  pdf_document:
    md_extensions: +simple_tables

md_extensions: +simple_tables+raw_tex

bibliography: Bibliography.bib
--- 

# 1. Introduction

Computers have been becoming increasingly integral to everyday life over the past half century. The advent of smart-phones has made computer technology more accessible and mobile. They have transformed the way we interact with society: making it easier to communicate with individuals, and making it easier to share and view content on social networks. Over the next couple decades, we can expect new technological developments, such as driver-less cars, augmented/virtual reality, and unmanned aerial vehicles (UAVs). These technologies will cause a significant fundamental change to the way we go about our daily lives. 

The three aforementioned technologies hinge on more "intelligent" software. For example, for a diver-less car to be able to function it needs to recognize objects and well as make decision based on the data it produces. To process the imagery, a combination of computer vision and machine learning algorithms can be used. 

These two fields were both born in the 1960s with major foundational developments taking place through the 1970s [@wiki_cv; @forbes_ml]. MORE ABOUT CV AND ML. Conclude with something about how it is important to chose appropriate algorithms for the context of the problem.Finding a suitable object detection algorithms for UAVs can be challenging since the images can be of poor quality as they are taken from a flying craft. 

CUAir, an interdisciplinary team at Cornell University, is trying to solve these computer visions difficulties for UAVs. The team designs, builds, and engineers an UAV, and competes annually at AUVSI SUAS, an international competition in Maryland. The team has a number of objectives to meet, including automatic takeoff, landing, egg drop, manual target recognition, and automatic target recognition (ATR). The ATR objective specifically relates to the computer vision problem for UAVs.

The ATR objective includes identifying 6 characteristics: color of shape and alphanumeric, shape name, alphanumeric character, GPS location, and orientation. Each ATR task presents specific challenges. To solve the shape classification problem, traditionally one would use a Hu moments algorithm. Hu moment feature extractors are implemented in OpenCV, a standard and widely used open-source computer vision library. Alternatively, a Fourier analysis algorithm that extracts Fourier characteristics from the contour of a shape can be used to identify shapes.

The objective of this paper is to investigate whether a Fourier analysis algorithm is feasible and accurate method to identify shapes. To demonstrate these properties it is necessary to:
1. Prove with rigorous mathematics the Fourier analysis produces unique vectors for different shapes, and identical ones for equivalent shapes
2. Illustrate with computational mathematics that under simulated real world conditions the Fourier analysis algorithm outperforms the benchmark Hu moment algorithm


other sentences, may want to include:
A Fourier analysis algorithm involves taking the unidentified data, transforming it into a waveform, and using the Fourier analysis vector to classify the shapes with a machine learning algorithm.

Our primary application for our system is search and rescue. Other applications include military, agriculture, and industry.

# 2. Benchmark Hu Moment Algorithm

##2.1 Algorithm Overview
An overview of a HuMoment shape classification algorithm:
1. Shape Thresholding : Binary thresholding
2. Hu Moment Featuring : Create a feature vector from the 7 Hu moment invariants
3. Machine Learning Classification : Train a k-nearest neighbor model

### 2.1.1 Shape Thresholding
Depending on the image characteristics, a simple binary thresholding algorithm could suffice to distinguish the image from the background. More complex thresholding ideas are elucidated in Appendix A.

### 2.1.2 Hu Moment Featuring
Hu moments were chosen because they are well established method form of object recognition. Hu moments (1962) are seven geometric moments that are rotation agnostic. The from the moments, one can also derive size agnostic invariants. Other moment invariants exist, however, one such alternative, Zernike moment invariants, shows not a significant improvement of accuracy, while being significantly more complex [@sabhara]. Therefore, Hu moments were chosen for control testing.

### 2.1.3 Machine Learning Classification
K-Nearest Neighbor Algorithm was selected because it classifies feature vectors easily and well. It was chosen over a Support Vector Machine (SVM) algorithm because it is usually performs better with small amounts of data than SVM [@raikwal].

# 3. Algorithm

##3.1 Algorithm Overview

An overview of the Fourier transform contour shape classification algorithm:
1. Shape Contouring : Given and region of interest, contour the shape
2. Polar Conversion : The shape is mapped from Cartesian space to the polar coordinate system. It is important to note that the shapes must be expressible as a polar function - for a given $\theta$ there must be only one point in the contour at radius $r$.
3. Resolution conversion: Re-sample the contour to get a constant sampling frequency
4. Fourier Analysis : Using a DFT library, get the discrete wave forms for the shape.
5. Machine Learning Classification : Train a k-nearest neighbor model

### 3.1.1 Shape Contouring
Depending on the image characteristics, a simple binary thresholding algorithm could suffice to distinguish the image from the background. More complex thresholding ideas are elucidated in Appendix A.



### 3.1.2 Polar Conversion
Converting the shape can be done using simple conversion function from Cartesian to polar space:

$f:\mathbb{R}^2\rightarrow\mathbb{R}^2$, where $f[(x,y)]\mapsto(\arctan(\frac{y}{x}), \sqrt{x^2+y^2})$

When coding this on the computer, it is recommended to use arctan2 to get the correct angle for all quadrants and avoid division by 0 errors.

![Pentagon Contour](PentagonContour.jpg)

An example of contoured and polarized pentagon can be seen in Figure 1.

### 3.1.3 Resolution Conversion
For most Discrete Fourier Transform (DFT) algorithms, it is necessary to have a constant frequency sampling rate, therefore, one should decimate and interpolate as necessary to have a constant sampling frequency for each contour and across contours. The target frequency should be optimized for the shape classes.

### 3.1.4 Fourier Analysis
A DFT is ideal for shape classification because it is size and orientation agnostic. Take the polar coordinates from the pentagon from Figure 1 and graph their radius as a function to get Figure 2.

![Pentagon Function](PentagonFunction.jpg)

To show that the Fourier analysis frequency domain for one n-gon is always different from another k-gon for any n and k, we can first express the contour for the n-gon as the following equation: 
\begin{equation} \label{eq:gencontour}
r(\theta, n) = \sec(\theta-\lfloor \frac{\theta n}{2\pi}\rfloor \frac{2*\pi}{n} - \frac{\pi}{n})
\end{equation}
where n is the number of sides. Fourier analysis can then be applied to the contours' equations to get the discrete frequency domains for shapes.

\begin{figure}
\centering
\begin{tabular}{cc}
  \includegraphics[width=65mm]{DiscretePlots/3.png} &   \includegraphics[width=65mm]{DiscretePlots/4.png} \\
(a) triangle & (b) square \\[6pt]
 \includegraphics[width=65mm]{DiscretePlots/5.png} &   \includegraphics[width=65mm]{DiscretePlots/6.png} \\
(c) pentagon & (d) hexagon \\[6pt]
\end{tabular}
\caption{Fourier Transform Frequency Domain}
\label{fig:shapefts}
\end{figure}

Notice how the the frequency domain for an n-gon includes only frequencies that are multiples of n (fig. \ref{fig:shapefts}). To show this formally we can express the general contour equation (eq. \ref{eq:gencontour}) as a sum of continuous functions so we can calculate the Fourier series.

\begin{equation} \label{eq:ftcof}
\begin{aligned}
a_{k,n} &= \frac{1}{\pi}\sum_{i=0}^{n-1}\int_{\frac{2\pi i}{n}}^{\frac{2\pi(i+1)}{n}} \sec(\theta - \frac{2\pi i}{n} - \pi n ) \cos(k\theta)d\theta \\
b_{k,n} &= \frac{1}{\pi}\sum_{i=0}^{n-1}\int_{\frac{2\pi i}{n}}^{\frac{2\pi(i+1)}{n}} \sec(\theta - \frac{2\pi i}{n} - \pi n ) \sin(k\theta)d\theta \\
m_{k,n} &= \sqrt{a_{k,n}^2 + b_{k,n}^2} \\
\end{aligned}
\end{equation}

where $a_{k,n}, b_{k,n}$ is the $k$ th $\cos, \sin$ term for the Fourier series describing the contour function for a $n$-gon. To prove that the Fourier frequency domain does not include frequencies which are not divisible by the number of sides $n$, let us define rotational symmetry,
\begin{equation} \label{eq:rotsym}
\centering
r\left(\theta, n\right) = r\left(\theta + \frac{2\pi}{n}, n \right)
\end{equation}

and reflectional symmetry,
\begin{equation} \label{eq:refsym}
\centering
r\left(\theta, n\right) = r\left(-\theta, n \right)
\end{equation}

All terms of the the Fourier series that have frequencies divisible by $n$ satisfy the properties defined by the above equations. Individual terms of the Fourier series that would have frequencies not divisible by $n$ would not satisfy the above properties, however, it is not clear that a linear combination of them would not satisfy the above equations. Define the following notation $\hat{a}_{k,n}$ is the variable that has value $a_{k,n}$. Each variable is only equal to itself, ore more formally: $\hat{a}_{p,q}=\hat{a}_{x,y} \iff p=q \wedge q=y$. Define the evaluation function $\mathscr{E}\left(\hat{v}_{a,b}\right) = v_{a,b}$, takes a variable and returns it's value.

Also, let's define the two following sets:
\begin{equation} \label{eq:setdef}
\begin{aligned}
\centering
C_n &= \{\hat{a}_{k,n} : \mathscr{E}\left(\hat{a}_{k,n}\right) \ne 0 \wedge n\nmid k \}\\
D_n &= \{\hat{b}_{k,n} : \mathscr{E}\left(\hat{b}_{k,n}\right) \ne 0 \wedge n\nmid k \}
\end{aligned}
\end{equation}


$C_n$ and $D_n$ contain the variables for the frequencies in the frequency domain that are not divisible by $n$. To show that $C_n,D_n=\emptyset$ we will use proof by induction.

**Base Case**: $\left\vert{C_n \cup D_n}\right\vert \ne 1$
Proof by contradiction: Assume $\left\vert{C_n \cup D_n}\right\vert = 1$
Let's call the one element in one of the sets $\hat{p}_{k,n}$. To maintain the properties described by equation \ref{eq:refsym}, $p_{k,n}sin\left(k\theta\right)=p_{k,n}sin\left(-k\theta\right)$, $sin$ is odd, so this is not true for all $\theta$, therefore $\hat{p}_{k,n}\notin D_{n}$.
If $\hat{p}_{k,n}\in C_{n}$ then $\hat{p}_{k,n}$ would satisfy equation \ref{eq:refsym}, however, it would not satisfy equation \ref{eq:rotsym}: $p_{k,n}\cos\left(k\theta\right) = p_{k,n}\cos\left(\frac{2\pi}{n}+k\theta\right)$. This is also not the case since $\frac{2\pi}{n}$ is not is a multiple of the terms period $\frac{2\pi}{k}$ as $n\mid k$ (eq \ref{eq:setref}).

**Inductive Step**: Assume $\left\vert{C_n \cup D_n}\right\vert$ cannot equal $p$, show that $\left\vert{C_n \cup D_n}\right\vert$ cannot equal $p+1$
Proof by contradiction: Assume that there exists a $\hat{p}_{k,n}$ not in $C_n\cup D_n$ that can be added to $D_n$ of the sets so that the Fourier series now satisfies the rotational symmetry outlined by outlined by equation \ref{eq:rotsym},
\begin{equation} \label{eq:hihi1}
\begin{aligned}
\sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(0\right) + \sum_{\hat{b}_{k,n}\in D_n} \mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(0\right) + p_{k,n}\sin\left(0\right) &=\\ & \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(\frac{2\pi k}{n}\right) + \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(\frac{2\pi k}{n}\right) + p_{k,n}\sin\left(\frac{2\pi k}{n}\right)\\
\sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right) &= \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(\frac{2\pi k}{n}\right) + \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(\frac{2\pi k}{n}\right) + p_{k,n}\sin\left(\frac{2\pi k}{n}\right)\\
\end{aligned}
\end{equation}

,and symmetrical symmetry outlined by equation and \ref{eq:refsysm},
\begin{equation} \label{eq:hihi2}
\begin{aligned}
\sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right) &= \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(-\frac{2\pi k}{n}\right) + \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(-\frac{2\pi k}{n}\right) + p_{k,n}\sin\left(-\frac{2\pi k}{n}\right)\\
\end{aligned}
\end{equation}

Subtracting equation \ref{eq:hihi2} from equation \ref{eq:hihi1} you:
\begin{equation*}
\begin{aligned}
0 &= 2\sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(-\frac{2\pi k}{n}\right) + 2p_{k,n}\sin\left(-\frac{2\pi k}{n}\right)\\
\end{aligned}
\end{equation*}
\begin{equation}
\begin{aligned}
p_{k,n}\sin\left(-\frac{2\pi k}{n}\right) &= \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(-\frac{2\pi k}{n}\right) \\
\end{aligned}
\end{equation}

This is a contradiction, $sin$ waves of different frequency are orthogonally perpendicular, so it is not possible to have $\hat{p}_{k,n}$ in $D_n$. Under the assumption that $\hat{p}_{k,n}$ could be in $C_n$, we follow similar steps. The Fourier series must satisfy rotational symmetry outlined by outlined by equation \ref{eq:rotsym},
\begin{equation} \label{eq:hihi1}
\begin{aligned}
\sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(0\right) + \sum_{\hat{b}_{k,n}\in D_n} \mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(0\right) + p_{k,n}\cos\left(0\right) &=\\ & \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(\frac{\theta + 2\pi k}{n}\right) + \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(\theta + \frac{2\pi k}{n}\right) + p_{k,n}\cos\left(\theta + \frac{2\pi k}{n}\right)\\
\sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right) + p_{k,n} &= \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(\theta + \frac{2\pi k}{n}\right) + \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(\theta +  \frac{2\pi k}{n}\right) + p_{k,n}\cos\left(\theta + \frac{2\pi k}{n}\right)\\
\end{aligned}
\end{equation}

,and symmetrical symmetry outlined by equation and \ref{eq:refsysm},
\begin{equation} \label{eq:hihi2}
\begin{aligned}
\sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right) + p_{k,n} &= \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(-\theta - \frac{2\pi k}{n}\right) + \sum_{\hat{b}_{k,n}\in D_n}\mathscr{E}\left(\hat{b}_{k,n}\right)\sin\left(-\theta  - \frac{2\pi k}{n}\right) + p_{k,n}\cos\left(-\theta - \frac{2\pi k}{n}\right)\\
\end{aligned}
\end{equation}

Add equation \ref{eq:hihi2} from equation \ref{eq:hihi1},
\begin{equation*}
\begin{aligned}
2 \sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right) + 2p_{k,n} &= 2 \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(\theta + \frac{2\pi k}{n}\right) + 2p_{k,n}\cos\left(\theta + \frac{2\pi k}{n}\right)\\
\frac{d}{d\theta}\Bigg(2 \sum_{\hat{a}_{k,n}\in C_n} \mathscr{E}\left(\hat{a}_{k,n}\right) + 2p_{k,n} &= 2 \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\cos\left(\theta + \frac{2\pi k}{n}\right) + 2p_{k,n}\cos\left(\theta + \frac{2\pi k}{n}\right)\Bigg)\\
0 &= -2 \sum_{\hat{a}_{k,n}\in C_n}\mathscr{E}\left(\hat{a}_{k,n}\right)\sin\left(\theta + \frac{2\pi k}{n}\right) -2p_{k,n}\sin\left(k\theta + \frac{2\pi k}{n}\right)
\end{aligned}
\end{equation*}
\begin{equation}
\begin{aligned}
p_{k,n}\sin\left(k\theta + \frac{2\pi k}{n}\right) &= -\sum_{\hat{a}_{j,n}\in C_n}\mathscr{E}\left(\hat{a}_{j,n}\right)\sin\left(j\theta + \frac{2\pi j}{n}\right)
\end{aligned}
\end{equation}
This is a contradiction, $\sin$ waves of different frequency are orthogonally perpendicular ($k\neq j$), so it is not possible to have $\hat{p}_{k,n}$ in $D_n$.

A unique Fourier Analysis frequency domain increases the robustness of the our algorithm. Random perturbations in the contour are less likely to cause the frequency domain to change so significantly that Fourier frequency domain vectors would be incorrectly classified.

### 3.1.5 K-Nearest Neighbor
See 2.1.3

# 4 Algorithm Testing
To evaluate the efficacy of the Fourier Analysis algorithm, accuracy of the algorithm was benchmarked against the Hu moments algorithm with the addition of random contour perturbations. When deciding on a good metric to compare the algorithms, real world conditions was the primary criteria. When segmenting a pixelated or blurry shape from its background, it is common for the contour to be extruded or intruded due to thresholding or contouring systemic errors. To simulate the perturbations, contour points were randomly extruded or intruded with a normal distribution random variable.


\begin{figure}[H]
  \centering
      \includegraphics[width=1.3\textwidth]{PerturbationDensity.jpg}
  \caption{A picture of the same gull
           looking the other way!}
  \label{fig:pertdens}
\end{figure}

Figure \ref{fig:pertdens} is a probability density for the shape form (filled in contour). Columns correspond to shapes and rows correspond to the standard deviation of the random variable used to cause perturbations in the contour. This graphic's purpose is to provide an intuitive understanding of how values of $\sigma$ correspond visually to the possible contours.

### Random Perturbation
\begin{figure}[H]
  \centering
      \includegraphics[width=1.3\textwidth]{RandomDistortions/RandomDistortions.jpg}
  \caption{Fourier Analysis Algorithm Results}
  \label{fig:faalg}
\end{figure}

\begin{figure}[H]
  \centering
      \includegraphics[width=1.3\textwidth]{RandomDistortionsHu/HuMomentPerturbation.jpg}
  \caption{Hu Moments Algorithm Results}
  \label{fig:hualg}
\end{figure}

### Sampling Rate



# Appendix A - Auxiliary Algorithms

For more challenging images, it may be necessary to convert the image into the Hue Saturation Value (HSV) color space and perform more complex thresholding algorithm variants, for example window box averaging with thresholding in the cylindrical coordinate system.

