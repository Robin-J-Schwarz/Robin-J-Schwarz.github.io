---
title:  "Find faces in gray-scale images using SVD"
search: true
categories: 
  - Machine Learning
last_modified_at: 2023-04-17
---

During my studies in systems engineering me and a fellow student worked on a machine learning algorithm to detect faces in a picture. The algorithm is based on singular value decomposition (SVD). SVD returns for a matrix $$Q$$ of form $$\mathbb{R}^{n\times m}$$ the matrices $$U^{n\times n}$$, $$\Sigma^{n\times m}$$ and $$V^{T}$$ of form $$\mathbb{R}^{m\times m}$$.

Using the algorithm: $$Q = \sum_{i = 1}^{k}{\sigma_i \bf{u_i} \bf{v_i^T}}$$ we are able to diminish the original matrix of the grey-scale image, which is the image we want to process, to a lower dimension. Using the norm of the diminished matrix and compared to a previous diminished matrix using 300 test images, we are able to set a threshold which defines if the picture contains a face or not. For our dataset of images our threshold $$\varepsilon$$ the value was determined through testing to be $$\varepsilon = 14.0$$.

Using this threshold we are able to find a face in a bigger image using a kernel which scans the image. Comparing the diminished matrix of each kernel with the previous testing matrix we can determine were the face is.  

**Read the full report in german [here](/assets/pdf/Projekt17_GruppeD.pdf)**

![Skyline](/assets/image/findeFaces/Skylinebild.png)
![SquaredDeviation](/assets/image/findeFaces/QuadratischeAbweichung.png)
![FoundFaceInImage](/assets/image/findeFaces/foundFace.png)


