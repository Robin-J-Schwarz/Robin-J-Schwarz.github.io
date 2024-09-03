---
title:  "Detect faces in gray-scale images using SVD"
search: true
categories: 
  - Machine Learning
last_modified_at: 2023-04-17
---

During my undergraduate degree in systems engineering, I worked with a fellow student on a machine learning algorithm to detect faces in an image. The algorithm is based on singular value decomposition (SVD), which is used in applications such as least squares. Using a similar approach, we generate a "mean face" from a regression of greyscale faces from a dataset of 300 68x56 px images. To do this, we use the algorithm $$Q = \sum_{i = 1}^{k}{\sigma_i \bf{u_i} \bf{v_i^T}}$$, where $$\sigma_i$$ is the i-th singular value, $$\bf{u_i}$$ is the i-th column vector of matrix $$U$$ and $$\bf{v_i^T}$$ is the i-th row vector of the matrix $$V$$ from the SVD. By using only the first couple of singular values (making $$k$$ much smaller than the original set of singular values from our SVD), we are able to reduce the dimension of our image matrix. From this diminished form we can calculate the norm between the "mean face" matrix and the diminished matrix of the image we want to classify if there is a face in the image. From the test data, we then compute a threshold that defines whether the image is identified as a face or not (in our case, the threshold was set to $$\varepsilon = 14.0$$ because all faces have a $$\varepsilon_{max}$$ lower than $$12.329$$ and the reference image without a face has a $$\varepsilon = 22.269$$). This application allows us to automatically classify whether an image contains a face or not.

Visualised "mean face":

![MiddleFace](/assets/image/findeFaces/middle.png)

Norm of the test dataset of images containing a face and an example of a correctly classified image with a face:

![Norm](/assets/image/findeFaces/IdentifiedFace.png)

With this application we are now able to find a face in a larger image using a kernel that scans the image and compares each section with the "mean face". The section where the norm is minimal is given the highest probability of containing the face.

Skyline image where the face should be found:

![Skyline](/assets/image/findeFaces/Skylinebild.png)

Least square of each kernel scanning the skyline image:

![LeastSquare](/assets/image/findeFaces/QuadratischeAbweichung.png)

Kernel identified as the cutout with the highest probability to contain a face:

![FoundImage](/assets/image/findeFaces/foundFace.png)

**Read the full report in german [here](/assets/pdf/Projekt17_GruppeD.pdf)**



