# Visual Waste Management System - Project Specification

## 1. Project Overview

This document outlines the specifications for the Visual Waste Management System. The primary goal of this project is to automate the process of measuring the contour of scrap or leftover materials. The system will capture images of the material within a defined working area, process these images to calculate the material's outline, and generate a DXF file representing this outline. The final DXF file will be stored on a Network-Attached Storage (NAS) for record-keeping and further use.

This system is designed for single-shot calculations and does not require real-time processing capabilities.

## 2. Goals and Scope

### 2.1. In-Scope
*   **Image Acquisition:** Capture images from four CCD cameras to cover the specified working area.
*   **Contour Detection:** Process the captured images to identify and calculate the outer contour of the target material.
*   **DXF Generation:** Convert the calculated contour data into a standard DXF file format.
*   **Data Storage:** Automatically save the generated DXF file to a designated location on the company's NAS.

### 2.2. Out-of-Scope (To Be Confirmed)
*   Real-time video processing or tracking.
*   Physical handling or manipulation of the materials.
*   User interface for system operation beyond a simple trigger mechanism (unless specified).
*   Advanced analysis of the material (e.g., color, texture, type recognition).

## 3. Performance Requirements

*   **Working Area:** 2m * 4m
*   **Contour Accuracy:** The generated DXF contour must be accurate to within **+/- 5mm** of the actual physical material's dimensions.
*   **Processing Cycle:** The system should complete the entire cycle (image capture to DXF file saved) for a single measurement upon a defined trigger.

## 4. System Architecture

### 4.1. Hardware Components
| Component             | Model/Specification                | Notes                                    |
| --------------------- | ---------------------------------- | ---------------------------------------- |
| Camera                | (x4) MV-CH120-20GC                 | 12MP resolution.                         |
| Lens                  | (x4) MVL-KF0618M-12MPE             |                                          |
| Processing Unit       | Embedded Computer with N305 Processor | Needs confirmation on RAM and storage.   |
| Network Switch        | 6 or 8 Port PoE Router             | Powers the cameras and connects devices. |
| Camera Enclosure      | Lens dust protection box           | Protects lenses in the work environment. |
| Storage               | NAS (Network-Attached Storage)     | Customer-provided.                       |

### 4.2. Software & Functional Flow
1.  **Trigger:** The process is initiated by a trigger (e.g., manual input, external sensor).
2.  **Image Capture:** The four PoE cameras simultaneously capture images of the working area.
3.  **Image Processing:**
    *   The images are sent to the embedded computer.
    *   The software may need to stitch or merge the four images to create a complete view of the 2m x 4m area.
    *   A contour detection algorithm is applied to identify the material's outline.
4.  **DXF Conversion:** The detected outline (a set of coordinates) is converted into the DXF file format.
5.  **File Transfer:** The generated DXF file is named according to a predefined convention and saved to the specified folder on the NAS.

## 5. Key Performance Indicators (KPIs) & Acceptance Criteria

| KPI                   | Target                                                               | Measurement Method                                                                                               |
| --------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Contour Accuracy**  | +/- 5mm                                                              | Place an object with known dimensions (e.g., a 1m x 1m template) in the working area. The generated DXF must match the template's dimensions within the tolerance. This test will be performed 10 times with the object in different positions. |
| **System Reliability**| >99% success rate for a complete cycle.                              | Over a test run of 200 cycles, fewer than 2 cycles should fail due to software or processing errors.            |
| **Processing Time**   | TBD (e.g., under 60 seconds from trigger to file save).              | Measure the time taken for each of the 200 test cycles.                                                          |

---

## 6. Points for Discussion with the Client

The following points require clarification to finalize the project scope and ensure a successful outcome.

### 6.1. Technical & Environmental
1.  **Lighting Conditions:** How consistent and stable is the ambient lighting in the working environment? Fluctuations in light can significantly impact image quality and contour detection accuracy. *Do we need to install a dedicated, controlled lighting system?*
2.  **Camera Mounting & Calibration:** What is the planned physical setup for the four cameras? We will need a rigid mounting structure and a precise calibration process to stitch the images accurately and meet the +/- 5mm requirement.
3.  **Processing Unit Specs:** While the N305 CPU is identified, we need to confirm the full specifications of the embedded computer (RAM, storage). This is critical to ensure it can handle the image processing load from four 12MP cameras.
4.  **Trigger Mechanism:** How should the measurement process be initiated? (e.g., a physical button, a software command, a sensor detecting material presence).

### 6.2. Software & Data
5.  **DXF File Specifications:**
    *   Is there a specific **DXF version** (e.g., R14, 2000, 2018) required for compatibility with your other systems?
    *   What is the required **file naming convention**? (e.g., `YYYYMMDD_HHMMSS.dxf`, `MaterialID_Timestamp.dxf`).
6.  **NAS Access Details:**
    *   What is the network path to the target folder on the NAS?
    *   What credentials or permissions are required for the system to write files to this location?
7.  **Error Handling:** How should the system report an error if it occurs? (e.g., if a contour cannot be detected, or if the NAS is unreachable). Should it save a log file, send an alert, or simply do nothing?

### 6.3. Validation
8.  **Accuracy Validation Procedure:** We have proposed a validation method using a known-sized template. Is this procedure acceptable for the final sign-off? Are there other specific test cases or materials we should consider?

---
