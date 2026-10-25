SYNTHETIC APERTURE RADAR (SAR) IMAGE FORMATION AND ANALYSIS USING MATLAB
==========================================================================

1. PROJECT TITLE
----------------
Synthetic Aperture Radar (SAR) Image Formation and Analysis Using MATLAB

2. OBJECTIVE
------------
This project simulates a side-looking stripmap Synthetic Aperture Radar
(SAR) system and forms focused SAR images from simulated radar echoes.

The complete processing chain is:

Aircraft/SAR platform
        ->
LFM waveform generation
        ->
Transmission and target reflection model
        ->
Raw complex SAR data
        ->
Matched-filter range compression
        ->
Range Migration Algorithm image formation
        ->
Back Projection Algorithm image formation
        ->
Target detection and localization
        ->
Performance comparison and result saving

The final SAR images are generated from simulated radar echo data. They are
not artificial plots of target coordinates.

3. FEATURES
-----------
- Physically based monostatic radar echo simulation
- Three stationary point targets with different ground ranges and RCS values
- Linear Frequency Modulated (LFM) pulsed waveform
- Radar Toolbox waveform generation:
  phased.LinearFMWaveform
- Radar Toolbox matched filtering:
  phased.RangeResponse
  getMatchedFilter
- Raw SAR data visualization
- Matched-filter range compression
- Range Migration Algorithm (RMA) with Stolt interpolation
- Time-domain Back Projection Algorithm (BPA)
- Peak-based target detection/localization
- RMA and BPA processing-time comparison
- Automatic saving of MAT, CSV, and PNG result files

4. REQUIREMENTS
---------------
Required:
- MATLAB R2024b or newer recommended
- Radar Toolbox

No additional toolbox is required by the project code.

5. FOLDER STRUCTURE
-------------------
SAR_Project/
|
|-- main_SAR.m
|-- README.txt
|
|-- config/
|   |-- sarParameters.m
|
|-- simulation/
|   |-- createSARPlatform.m
|   |-- createTargets.m
|   |-- generateSARWaveform.m
|   |-- simulateSARData.m
|
|-- processing/
|   |-- rangeCompression.m
|   |-- rmaImageFormation.m
|   |-- bpaImageFormation.m
|   |-- detectTargets.m
|
|-- visualization/
|   |-- plotSARGeometry.m
|   |-- plotRawData.m
|   |-- plotRangeCompressedData.m
|   |-- plotSARImages.m
|   |-- plotComparison.m
|
|-- results/
    |-- SAR_Project_Results.mat
    |-- RMA_Target_Detections.csv
    |-- BPA_Target_Detections.csv
    |-- Processing_Time_Comparison.csv
    |-- Figure_1_SAR_Geometry.png
    |-- Figure_2_Raw_SAR_Data.png
    |-- Figure_3_Range_Compressed_Data.png
    |-- Figure_4_RMA_SAR_Image.png
    |-- Figure_5_BPA_SAR_Image.png
    |-- Figure_6_RMA_vs_BPA_Comparison.png

6. INSTALLATION AND SETUP
-------------------------
1. Create the folder structure exactly as shown above.
2. Copy each MATLAB file into its specified folder.
3. Start MATLAB.
4. Set Current Folder to the SAR_Project folder.
5. Verify that Radar Toolbox is installed:
       ver
6. Run:
       main_SAR

7. HOW TO RUN
-------------
From the MATLAB Command Window:

    main_SAR

Do not run helper functions individually.

main_SAR automatically:
- Adds project folders to the MATLAB path
- Creates results/ if it does not already exist
- Simulates raw SAR data
- Forms RMA and BPA images
- Detects targets
- Saves all outputs

8. SYSTEM PARAMETERS
--------------------
Carrier frequency:             4 GHz
Bandwidth:                     50 MHz
Pulse width:                   3 microseconds
Sampling frequency:            100 MHz
PRF:                           1000 Hz
Platform velocity:             100 m/s
Platform altitude:             500 m
Synthetic aperture length:     80 m
Number of pulses:              approximately 801
Theoretical range resolution:  approximately 3 m

9. COORDINATE SYSTEM
--------------------
x-axis:
    Along-track direction, also called azimuth or cross-range.
    The aircraft flies along the x-axis.

y-axis:
    Ground-range or cross-track direction.
    Radar antenna looks broadside in the positive y direction.

z-axis:
    Vertical altitude direction.

The platform follows:
    [x_p(t), y_p(t), z_p(t)] = [v*t, 0, H]

Targets are stationary on the ground:
    [x_t, y_t, z_t] = [x_t, y_t, 0]

10. EXPECTED FIGURES
--------------------
Figure 1:
    SAR ground-truth geometry in top and 3-D views.

Figure 2:
    Raw SAR echo data. Delayed LFM chirp returns are visible as
    sloped/curved energy patterns across fast time and aperture position.

Figure 3:
    Range-compressed phase history. Target returns become narrow in
    the range direction, while their azimuth phase histories remain.

Figure 4:
    Focused SAR image using the Range Migration Algorithm.

Figure 5:
    Focused SAR image using time-domain Back Projection.

Figure 6:
    Side-by-side RMA/BPA images, processing times, and localization errors.

11. RESULTS FILES
-----------------
SAR_Project_Results.mat contains:
- p
- platform
- targets
- waveformData
- rawData
- rangeCompressedData
- rangeAxis
- rmaImage
- rmaRangeAxis
- rmaAzimuthAxis
- bpaImage
- bpaRangeAxis
- bpaAzimuthAxis
- rmaDetectionTable
- bpaDetectionTable
- comparisonTable
- processing times

12. THEORY OVERVIEW
-------------------
Wavelength:
    lambda = c / fc

Range resolution:
    delta_R approximately c / (2B)

Slant range from platform to a target:
    R = sqrt((x_t-x_p)^2 + (y_t-y_p)^2 + (z_t-z_p)^2)

Two-way delay:
    tau = 2R / c

The received echo has a carrier phase that depends on range:
    exp(-j*4*pi*fc*R/c)

Range compression:
    The received chirp is matched-filtered with the time-reversed complex
    conjugate of the transmitted LFM pulse. This compresses each long LFM
    pulse to a narrow range response.

RMA:
    RMA transforms range-compressed data into a 2-D wavenumber domain.
    It applies bulk phase correction and Stolt interpolation to correct
    range migration before inverse transformation.

BPA:
    BPA projects the measured range-compressed data back onto every pixel.
    The correct range delay and phase are calculated for each aperture
    position and pixel, then coherently summed.

13. TROUBLESHOOTING
-------------------
Problem:
    "Undefined function or variable 'phased.LinearFMWaveform'"

Cause:
    Radar Toolbox is not installed or licensed.

Fix:
    Install/enable Radar Toolbox. Run:
        ver
    and confirm Radar Toolbox appears.

Problem:
    "Undefined function 'physconst'"

Cause:
    Radar Toolbox / Phased Array functionality is unavailable.

Fix:
    Install Radar Toolbox.

Problem:
    BPA is slow.

Cause:
    BPA computes a full image by summing contributions from all pulses.

Fix:
    Reduce these values in config/sarParameters.m:
        p.bpaAzimuthPixelSpacing
        p.bpaGroundRangePixelSpacing
    Increasing spacing reduces the number of pixels and speeds up BPA.

Problem:
    Out-of-memory error.

Fix:
    Reduce:
        p.fs
        p.syntheticApertureLength
        p.maxGroundRange
    or increase BPA image pixel spacing.

Problem:
    Empty or very weak image.

Fix:
    Ensure MATLAB Current Folder is SAR_Project.
    Ensure every file is in the correct subfolder.
    Confirm that no source file was modified incorrectly.
    Confirm the selected range limits cover all targets.

Problem:
    Dimension mismatch error.

Fix:
    Confirm all code has been copied completely.
    Do not transpose or modify vectors unless you understand the
    row/column convention used by the project.

14. REFERENCES
--------------
MathWorks:
"Stripmap Synthetic Aperture Radar (SAR) Image Formation."

MathWorks documentation:
- phased.LinearFMWaveform
- phased.RangeResponse
- getMatchedFilter
- Radar Toolbox

Cafforio, C., Prati, C., and Rocca, F.
"SAR Data Focusing Using Seismic Migration Techniques."
IEEE Transactions on Aerospace and Electronic Systems, 1991.

Yegulalp, A. F.
"Fast Backprojection Algorithm for Synthetic Aperture Radar."
Proceedings of the IEEE Radar Conference, 1999.

Na, Y., Lu, Y., and Sun, H.
"A Comparison of Back-Projection and Range Migration Algorithms for
Ultra-Wideband SAR Imaging."
IEEE SAM Workshop, 2006.