{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Reflections"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 1. Desciption of the pipeline"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "An 8 stage pipeline has been implemented to achieve the final objective of lane detection.\n",
    "\n",
    "1) Stage-1 of the pipeline selects only the yellow and the white pixels from the incoming image. The rational here is that most of the times, lanes are marked with either white or yellow.\n",
    "\n",
    "2) Stage-2 of converts the image from RGB scale to Gray scale.\n",
    "\n",
    "3) Stage-3 defines a mask in terms of a quadrilateral which is our region of interest i.e. this is the region where we expect the lanes to be found with a high probability. This mask is then applied to the gray scale image to obtain a masked gray scale image.\n",
    "\n",
    "4) Stage-4 performs Gaussian filtering. This reduces the noise in the image. The kernel length should be selected carefully since if excessive filtering is applied, it will blur the image making it hard to detect edges.\n",
    "\n",
    "5) Stage-5 performs the Canny edge detection transform on the masked image to detect edges in the image. \n",
    "\n",
    "6) Stage-6 performs the Hough transform to detect all the lines in the region of interest.\n",
    "\n",
    "7) Stage-7 replaces the smaller line segments found in step-6 and replaces them with one solid line on each of the sides. To achieve this, we classify all the points found by the Hough transform which constitute a line segment, either to the left lane or to the right lane. This is done by finding out the slope of each line segment and classifying them based on whether it is positive or negative. To eliminate the line segments which have positive slope by lie in the left lane and vice versa, we put an additional constraint of checking the relative position of the point with respect to middle of the image. Once all the points are classified, a linear regression is performed on the points to get parameters of the two lanes. Finally, the top and the bottom y-coordinate of the quadrilateral defined in step 3 is used to find out the corresponding x coordinates. These lines are drawn on a blank image then.\n",
    "\n",
    "8) Stage-8 is the final stage of the pipeline. It combines the original image with the image obtained from stage-7 to get the final image which has the lanes identified.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Shortcomings"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "1) I found the pipeline to be extremely sensitive to the defined region of interest. It had to be hand tuned to ensure that lanes in all possible test scenarios can be covered. However, this approach cannot scale as the pipeline is applied to more and more test samples. The challenge video exposes this issue as well. \n",
    "\n",
    "2) It is assumed that the lane makings are either in white or yellow. Although I believe this to be a fair assumption, however it is not generic enough to cover all scenarios.\n",
    "\n",
    "3) Curves are not identified in this pipeline.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Improvements"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "There are two important directions in which this project can be improved.\n",
    "\n",
    "1) From a software perspective:  The current code is not exception safe. If any of the stages throw an exception it should be handled gracefully and proper actions must be defined.\n",
    "\n",
    "2) From an algorithmic perspective: The pipeline can be further enhanced by using Hough transform for detecting curves as well.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.5.2"
  },
  "widgets": {
   "state": {},
   "version": "1.1.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
