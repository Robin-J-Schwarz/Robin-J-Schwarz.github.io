---
title:  "Find faces in gray-scale images using SVD"
search: true
categories: 
  - Machine Learning
last_modified_at: 2023-04-17
---

During my studies in systems engineering me and a fellow student worked on an machine learning algorithm to detect faces in a picture. The algorithm is based on singular value decomposition (SVD). SVD returns for a matrix $Q$ of form $\mathbb{R}^{n\times m}$ the matrices $U^{n\times n}$, $\Sigma^{n\times m}$ and $V^{T}$ of form $\mathbb{R}^{m\times m}$.<br> 

Using the algorithm: $$Q = \sum_{i = 1}^{k}{\sigma_i \bf{u_i} \bf{v_i^T}}$$ we are able to diminish the matrix $Q$, which is the image we want to process. $Q$  



**Read the full report on german [here] (/assets/pdf/Projekt17_GruppeD.pdf)**

![Skyline](/assets/image/findeFaces/Skylinebild.png)
![SquaredDeviation](/assets/image/findeFaces/QuadratischeAbweichung.png)
![FoundFaceInImage](/assets/image/findeFaces/foundFace.png)
