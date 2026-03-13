# HR Extraction from Video (Face)

> An application of rPPG to extract the Heart Rate of a subject from a video of their face. 


# Table of Contents

* [Project Overview](#project-overview)
* [Project Structure](#project-structure)
* [Dependencies](#dependencies)
  * [Codes and Resources Used](#codes-and-resources-used)
  * [Python Packages Used](#codes-and-resources-used)
* [Data](#data)
  * [Source Data](#source-data)
  * [Data Acquisition](#data-acquisition)
* [Setup Instructions](#setup-instructions)
* [Code Structure](#code-structure)
* [Results and Evaluation](#results-and-evaluation)
* [Proposed Improvements](#proposed-improvements)
* [Acknowledgements/References](#acknowledgementsreferences)


# Project Overview

This implementation is a submission for the final project of the class of "Digital Image Processing" (DUTH ECE: 8th Semester 2025), for which students were tasked to solve a problem of their choice using only Digital Image Processing (DIP) techniques.

The problem tackled here is the Heart Rate (HR) estimation from a facial video footage. The proposed solution is an implementation of remote Photoplethysmography, in which DIP is used to meassure the subtle channel color changes of a focused area of the skin (in this case of the forehead) caused by the variable blood volume. By meassuring the frequency of the normalized and denoised Blood Volume Pulse (BVP) signal the Heart Rate of the subject can be estimated in an offline way.

![Sample frame and Results](media/frame_n_hr.png)


# Project Structure

The repository consists of the following files:
```bash
├── media
|   └── frame_n_hr.png
├── 58545_DIP_HR_Estimation.pdf
├── HR_Extraction_from_Video.ipynb
└── README.md
```
* `media`: Directory containing pictures used in README.md
* `58545_DIP_HR_Estimation.pdf`: An extensive report *(in greek)* of the problem and the applied methods.
* `HR_Extraction_from_Video.ipynb`: The heart of the project. A Jupyter Notebook containing the step-by-step implementation of the solution *(text cells in greek)*. 


# Dependencies

## Codes and Resources Used
* **Editor Used:** Google Colab / Jupyter Notebook
* **Python Version:** 3.11 (Google Colab Default Runtime at the time of the project)

## Python Packages Used
All the necessary dependencies needed for the reproduction of the project are categorized as follows:

> **Note:** Since this project is developed entirely within Google Colab, the package versions listed below correspond to the default Colab environment at the time of the last code update (June 2025).

* **General Purpose & Utilities:**
  * `os` *(Built-in with Python 3.11)*: For interacting with the operating system and handling directory paths.
  * `zipfile` *(Built-in with Python 3.11)*: For extracting the dataset archives.
  * `tqdm` *(v4.66.x)*: For displaying progress bars during loop executions.
  * `google.colab` *(Native Colab library)*: For mounting Google Drive and displaying images within the Colab environment.

* **Data Manipulation & Computer Vision:** 
  * `numpy` *(v1.26.x)*: For numerical operations, matrix handling, and array manipulation.
  * `cv2` *(OpenCV v4.9.x)*: For computer vision tasks, specifically video capturing, frame extraction, color-space conversion and face/ROI detection.

* **Signal Processing:**
  * `scipy.signal` & `scipy.fft` *(v1.12.x)*: For applying Butterworth filters, Fast Fourier Transforms (FFT), Short-Time Fourier Transforms (STFT), and finding signal peaks.

* **Data Visualization:** 
  * `matplotlib.pyplot` *(v3.8.x)*: Used for all the plots within the project.


# Data

## Source Data
The dataset used in this project is the **UBFC-rPPG Dataset**, which is widely used in academic research for remote photoplethysmography (rPPG).

* **Source Link:** [UBFC-rPPG Dataset](https://sites.google.com/view/ybenezeth/ubfcrppg)
* **Data Description:** The dataset consists of uncompressed video recordings of subjects filmed in 30fps and 640x480 resolution by a simple low cost webcam (Logitech C920 HD Pro) and of their corresponding ground truth obtained by a transmissive pulse oximeter (CMS50E). The dataset is divided into two sets depending on the environmental conditions.
  1. **Dataset 1 (simple):** 8 subjects that were asked to sit still but there was some movement in the (office) background. For each subject the following resources are provided:

      ```bash
      ├── gtdump.axi
      │   ├── Column 1: Timestep (ms)
      │   ├── Column 2: Heart rate (HR)
      │   ├── Column 3: SpO2
      │   └── Column 4: PPG signal
      └── vid.avi
      ```
      where `vid.avi` is the video of the subject and `gtdump.axi` the ground truth.
  
  2. **Dataset 2 (realistic):** 42 subjects that were tasked to play a time sensitive mathematical game. The subjects were not instructed to stay still and the background was static (green screen). For each subject the following resources are provided:

      ```bash
      ├── ground_truth.txt
      │   ├── Line 1: PPG signal
      │   ├── Line 2: Heart rate (HR)
      │   └── Line 3: Timestep (seconds, scientific notation)
      └── vid.avi
      ```
      where `vid.avi` is the video of the subject and `ground_truth.txt` the ground truth.
  
For the development and testing of the software, only subject 45 of Dataset 2 is used, while data of subject 11 of Dataset 1 are also used for highlighting some of the weaknesses of the proposed algorithm.

> **Note:** While only the **two above-specified subjects** are used in the final Jupyter file, the implemented algorithms are fully generalized and the code can successfully run on any subject's data within the dataset, provided they come from the same subset. More specifically, any subject of Dataset 2 can replace subject 45 and any subject of Dataset 1 can replace subject 11, but not otherwise.

## Data Acquisition
To acquire the data necessary to reproduce the project, follow the steps below:

1. **Access the Data:** Navigate to [this Google Drive folder](https://drive.google.com/drive/folders/1o0XU4gTIo46YfwaWjIgbtCncc-oF44Xk) containing the data of the UBFC-rPPG Dataset.
2. **Download the Data:** Locate the `subject45` folder in `DATASET_2` and the `11-gt` folder in `DATASET_1` and download them. You should now have 2 separate `.zip` files of those folders.
3. **Do Not Unzip:** You do not need to manually extract the files on your machine as the provided code handles the unzipping. Keep the file as `.zip`.


# Setup Instructions

1. **Load the Notebook:** Upload and open the `HR_Extraction_from_Video.ipynb` file in Google Colab.
2. **Configure the Dataset Path:** You have two options for loading the `.zip` files of the subjects into the environment:
   * **Option A (Via Google Drive):** Upload the `.zip` files to your Google Drive in a directory of your choice. This may take a few minutes. Then run the first code cell of `HR_Extraction_from_Video.ipynb` to mount your Drive. For the second code cell make sure to change the variable `zip_path` to match the exact directories of the files in your personal Drive.
   * **Option B (Local Colab Upload):** Upload the `.zip` files directly into Colab's temporary session storage using the ***Files*** section on the left sidebar. If you choose this route, running the first code cell of `HR_Extraction_from_Video.ipynb` is unnecessary, but remember to update the variable `zip_path` in the second code cell to match the exact directories of the files in the local session storage instead of in Drive.
3. **Force-install Specific Package Versions:** Because cloud environments update their software frequently, newer versions of libraries might cause compatibility issues. If you encounter any unexpected errors while running the cells, please force-install the specific package versions listed in the [Python Packages Used](#codes-and-resources-used) section. To do that create a new code cell and run, for example, the command `!pip install opencv-python==4.9.0.80`.


# Acknowledgements/References
