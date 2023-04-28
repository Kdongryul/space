####Feature Extraction####

#Load image
Import the image fair with filepath and name

#Find featrue correspondence with SIFT

#Draw matches
Detect the keypoints and compute descriptors for the two input images matching the descriptors with SIFT algorithm



####Essential Matrix estimation####

#Preprocessing
2D pixel coordinates generation from kepoints in the second image that correspond to the keypoints in the first image
and generated pixel coordinates will be converted to Numpy array.

#Find Essential Matrix and Inliers by using RANSAC and Intrinsic parameter
Find the essential matrix with RANSAC algorithm. you can change the repeat number of RANSAC and threshold. and if the intrinsic parameter of camera is changed, you can modify the focal(focal length) and pp(principal point)



#####Essential Matrix Decomposition(Find the reasonable camera position)

#Find the possible camera matrix
Essential matrix will be decomposed into R(rotation matrix) and t(translation matrix) to find 4 camera position.(SVD method is used to decompose)

#Choose the camera position
By using flatten, multidimension array will be convert to a one-dimensional array. this process let the x and y coordinates of keypoint make the single point.
In concatenate process, two points from a and b flatten process, merged into single array c. 
we know the tmp is the camera matrix(3x4), and c single array that connect the pixel from two images. By calculating the cross product between transpose of c(c.T) and tmp, we can get the d which means the estimated postion of 3d point in homogeneous coordinates
The camera position which has the negative d value isn't proper.  

#Rescale to Homogeneous Coordinate
In the next code, 2D points coordinates pts1[i] and pts2[i] is converted to 1D array using flatten() method to use for matrix multiplication later.
The append() method is used to concatenate the two 1D arrays tmp1 and tmp2 along the first axis (i.e., vertically) to form a single 2D array. The resulting concatenated array contains the homogenous coordinates of the corresponding points in pts1 and pts2.
this series of processes(flatten and append) is preperance of triangulation. The concatenated 2D arrays a and b are then returned after reshaping them back into 2D arrays of shape (length, 3) where length is the number of points.

#Camera matrix initialization
Rt0 and Rt1 represent the camera matrices of two views.

#triangulation
The LinearTriangulation function takes Rt0, Rt1, p1 and p2 as input.
p1 and p2 are the corresponding 2D points in view 1 and view 2, respectively. 
The function first computes the matrix A using the camera matrices and the 2D points. 
Then, the singular value decomposition (SVD) of the matrix A^T A is computed, and the 3D point is obtained by taking the last column of the V matrix, which corresponds to the smallest eigenvalue.

####Visulize####
To visualize 3d points make_3dpoint(p1,p2) function is used. 'p1' and 'p2' means the list of 2D points respectively. and the linear triangluation is also used to estimate the 3D point coordinates(p3ds)
p3ds is represented 3d point as a (3,1) row vector. And np.array(p3ds) function make the (3,num_points) form matrix. if this matrix is decomposed, it change into (num_points,3) which is more advantageous for visulization.
Because the scatter3D function needs three seprate arrays, X,Y,Z arrays are concatenated to create a single array for each of three axes of 3D plot. 
In the ax.scatter3D(), c means colormap of dots(Colormap reference:https://matplotlib.org/3.1.0/tutorials/colors/colormaps.html), and marker='o' means assign the shape of dots.
