# Seven Segment Scan

This repository contains part of the code written for my master thesis. The goal of the thesis was to create an application for people with visual impairements to aid them with the use of household objects containing 7 segment displays.

## About

The code itself attempts to use OCR techniques to locate and read a seven segment display in a provided image.
For this, [SSOCR](https://github.com/jiweibo/SSOCR) and [SegoDec](https://github.com/scottmudge/SegoDec) were used as a jumping-off point.

This code was part of a React Native turbo module, though this repository only contains stand-alone C++ code.

**This repo is not meant for active use**

If you want to have access to the full code produced for my thesis, contact me and I might give you access to the private repo.
This includes the module, a React Native app used to test the module and several python scripts testing out the initial functionality.

## Interesting bits

- [object detection](Seven-Segment-Scan/main/objectdetect.cpp): `detect_digits(cv::Mat& inputImage)`

This algorithm uses the specific shape that 7 segment displays have to locate the position of the digits in the display in a process I called "Halve Reduction" in my paper.

![half-reduc](https://github.com/user-attachments/assets/71d3eedd-583d-49fe-88ec-1ea57087f117)


- [image procesing](Seven-Segment-Scan/main/mageprocessing.cpp): `crop_border(cv::Mat& inputImage, cv::Mat& outputImage)`

This algorithm crops the image so it only contains the digits. This is done using `calc_crop_aggresive(cv::Mat& sum, int& low, int& high, const float alpha)`, which basically reduces information from the 2D image to a 1D array and uses this to decide a cut-off point in a single loop.

- [image procesing](Seven-Segment-Scan/main/mageprocessing.cpp): `colour_profiling(cv::Mat& inputImage, cv::Mat& outputImage)`

A dynamic version on what SSOCR and SegoDec did. While these both required you to input parameters to have a succesfull parsing of an image, this algorithm will calculate the treshold.
In theory atleast, as can be seen by the large amount of commented-out code, this is not reliable. The rate of recognising was around 53%. 
It works quite well however if there are no shadows or any light gradiant present in the image. Since the application was supposed to be used in a household context, this was not an acceptable requirement.
