---
title:  "Find faces in gray-scale images using SVD"
search: true
categories: 
  - Machine Learning
last_modified_at: 2023-04-17
---

During my studies in systems engineering me and a fellow student worked on a machine learning algorithm to detect faces in a picture. The algorithm is based on singular value decomposition (SVD) which is used in applications like the least square method. With a similar approach we generate from a regression of gray-scale faces a "middle face" from a dataset of 300 images of format 68x56 px. In order to do that we use the algorithm $$Q = \sum_{i = 1}^{k}{\sigma_i \bf{u_i} \bf{v_i^T}}$$, where $$\sigma_i$$ is the i-th singular value, $$\bf{u_i}$$ is the i-th colum vector of matrix $$U$$ and $$\bf{v_i^T}$$ is the i-th row vector of matrix $$V$$ from the SVD. By only using the first couple of singular values (making $$k$$ much smaller than the original amount of singular values from our SVD) we are able to diminish the dimension of our image matrix. From this diminished form we can calculate the norm between the "middle face" matrix and the diminished matrix of the image we want to classify if there is a face in the image. From the test data we then calculate a threshold which defines if the image is identified as a face or not (in our case the threshold was set at $$\varepsilon = 14.0$$ due to all faces having a $$\varepsilon_{max}$$ lower then $$12.329$$ and the reference image without a face having an $$\varepsilon = 22.269$$). This application allows us to automatically classify if an image contains a face or not.

Visualised "middle face":
![MiddleFace](/assets/image/findeFaces/middle.png)

Norm of the test dataset of images containing a face and an example of a correctly classified image with a face:
![Norm](/assets/image/findeFaces/IdentifiedFace.png)

Using this application we are now able to find a face in a bigger image using a kernel which scans the image and compares each cutout to the middle image. The cutout where the norm is minimal receives the highest probability to contain the face.   

Skyline image where the face should be found:
![Skyline](/assets/image/findeFaces/Skylinebild.png)

Least square of each kernel scanning the skyline image:
![LeastSquare](/assets/image/findeFaces/QuadratischeAbweichung.png)

Kernel identified as the cutout with the highest probability to contain a face:
![FoundImage](/assets/image/findeFaces/foundFace.png)

**Read the full report in german [here](/assets/pdf/Projekt17_GruppeD.pdf)**



