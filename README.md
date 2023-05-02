Download Link: https://assignmentchef.com/product/solved-cis580-homework-5-recover-the-camera-pose-with-two-different-approaches-pnp-and-p3p
<br>
<h1> Homework 5</h1>

<h2>Introduction to Augmented Reality using AprilTags</h2>

In this assignment, you are given a set of images with an AprilTag. You are also given the size of the tag and the pixel locations of its corners on the images. Your task is to recover the camera pose with two different approaches: PnP with coplanar assumption, and P3P followed by Procrustes. After you recover the camera pose, we will place a virtual camera at that same position to render a virtual bird as if it stands on the tag.

<img decoding="async" data-recalc-dims="1" data-src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/05/904.png?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/05/904.png?w=980&amp;ssl=1" data-recalc-dims="1">

 </noscript>         (a) sample image                                (b) sample result

Figure 1: Projecting a bird mesh on the AprilTag center

<h2>1            Problem 1 – 5pts, Establish a World Coordinate</h2>

(a) world coordinate setup

To know where the camera is in the world, we must first define a world coordinate system. For this assignment, since we want to place a bird onto the tag, it is convenient to place the world coodinate at the center of the tag as shown in the figure above.

From the lecture, we know that to recover camera pose from 2D-3D correspondence, we need at least 3 points. Here the AprilTag provides us 4 points. We used the detection software for AprilTag to find the 2D pixel coordinates of a, b, c, d, which have been provided to you in <strong>corners.npy </strong>and will be used in <strong>run PnP.py </strong>and <strong>run P3P.py</strong>.

Your first task is to find the 3D world coordinates of a, b, c, d by completing <strong>est Pw.py </strong>given the length of the side of the tag. With the world coordinate placed at the center of the tag, this should not be difficult. This function will be used for the next two sections.

<h3>1.1      Files to complete and submit</h3>

<ol>

 <li><strong>est Pw.py</strong></li>

</ol>

This function is responsible for finding world coordinates of a, b, c and d given tag size

<h2>2            Problem 2 – 25pts, Solve PnP with Coplanar assumption</h2>

After getting the 2D-3D correspondence, we will recover the camera pose with two different approaches. This section covers the first method which closely follows lecture slides ”Extrinsics from Collineation – Pose from Projective Transformations”

Since the world frame is conveniently placed on the tag, the z component of the corners should be zero. Following the slides, we have

where K  is a homography H. To recover H, you will use <strong>est homography.py </strong>that you’ve programmed in hw2. We also provided a completed version. You are free to choose either.

After getting H, you should follow the slides to recover R and t. Note that in the slide, <em>R </em>= <em>R<sub>cw </sub></em>and <em>t </em>= <em>t<sub>cw</sub></em>, but the function requires to return <em>R<sub>wc </sub></em>and <em>t<sub>wc</sub></em>, which describe camera pose in the world. You will complete all of above inside <strong>PnP.py</strong>

<h3>2.1     Visualization</h3>

Once you complete <strong>est Pw.py </strong>and <strong>PnP.py</strong>, run <strong>python run PnP.py </strong>in terminal. This will generate a GIF called bird collineation.gif.

<h3>2.2      Files to complete and submit</h3>

<ol>

 <li><strong>py</strong></li>

</ol>

This function is responsible for recovering R and t from 2D-3D correspondence

<ol start="2">

 <li><strong>bird collineation.gif</strong></li>

</ol>

This GIF visualizes the bird for method 1.

<h2>3            Problem 3 – 70pts, Solving the Perspective 3-Point problem</h2>

Here you will go through the process of calculating the camera pose by first calculating the 3D coordinates of any 3 (out of 4) corners of the AprilTag in the camera frame. You already know the 3D coordinates of the same points in the world frame from Part 1, so you will use this correspondence to solve for Camera Pose <em>R,t </em>in the world frame by implementing Procrustes.

<h3>3.1      P3P Derivation 20 pts</h3>

Figure 3: Illustrates the geometry of the three point space resection problem. The triangle side lengths <em>a,b,c </em>are known and the angles <em>α,β,γ </em>are known. The problem is to determine the lengths <em>s</em><sub>1</sub><em>,s</em><sub>2</sub><em>,s</em><sub>3 </sub>from which the 3D vertex point positions (in the camera frame) <em>P</em><sub>1</sub><em>,P</em><sub>2</sub><em>,P</em><sub>3 </sub>can be immediately determined.

You only need to solve till you get a 4th degree polynomial. Basically get the coefficients of this polynomial in terms of <em>a,b,c,α,β,γ</em>, then we will code them up in the next part to get the roots of the polynomial.

You already have a part of the proof for P3P in class (PNP slides). You can refer this document if you face difficulty in solving P3P. <a href="https://haralick-org.torahcode.us/journals/three_point_perspective.pdf">P3P reference paper</a> (you need to use Grunert’s solution)

<h3>3.2      Coding Sections</h3>

<strong>3.2.1       P3P – 25 pts</strong>

In this function, you need to implement P3P to basically calculate the 3D coordinates of the 3 points in the camera frame. Refer slides ”Perspective N Point Problem”

You will define <em>a,b,c </em>and <em>α,β,γ </em>and then compute the coefficients of the polynomial you got in the previous part. Then you will solve the polynomial to get its roots which will then give you the distances <em>s</em>1<em>,s</em>2<em>,s</em>3. You will then use these distances to get the 3D coordinates of the 3 points in the camera frame.

Important Points to consider:

<ul>

 <li>you have 4 corners and are free to use any 3 of them.</li>

 <li>You can use numpy.roots to get the roots of the polynomial</li>

 <li>You will get total of 4 roots out of which 2 will be real, you need to select the one that gives all the distances of the 3 points from the camera to be positive.</li>

</ul>

<strong>3.2.2       Procrustes – 25 pts</strong>

You need to use the correspondence of the same 3 points in the world frame and the camera frame and solve for camera pose using the Procrustes method. Use the Procrustes slides from class for your reference.

<em>N</em>=3 min <sup>X </sup>||<em>A<sub>i </sub></em>− <em>RB<sub>i </sub></em>+ <em>T</em>||<sup>2</sup>

<em>R,T i</em>

<h3>3.3     Visualization</h3>

Once you complete <strong>P3P.py</strong>, run <strong>python run P3P.py </strong>in terminal. This will generate a GIF called bird P3P.gif.

<strong>3.3.1        Files to complete and submit</strong>

<ol>

 <li><strong>py</strong></li>

</ol>

This file has two functions P3P and Procrustes that you need to write. P3P solves the polynomial and calculates the distances of the 3 points from the camera and then uses the corresponding coordinates in the camera frame and the world frame. You need to call Procrustes inside P3P to return R,t.

<ol start="2">

 <li><strong>bird P3P.gif</strong></li>

</ol>

This GIF visualizes the bird for method 2.