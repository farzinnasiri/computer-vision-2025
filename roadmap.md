# Instruction Set for Visual Analysis of Moving Vehicles

You have one or more videos of cars moving on a road, recorded from a fixed camera. You need to detect and track these vehicles, estimate their 3D positions, compute their speed, and predict time-to-impact. Follow these steps:

---

## 1. Acquire or Estimate Camera Parameters
1. Gather the video frames.  
2. If actual camera parameters (focal length, principal point, etc.) are unknown, infer them from:
   - Known distances in the scene (e.g., lane width or typical car widths).  
   - Parallel or perpendicular lines (e.g., lane markings) to find vanishing points and approximate the focal length.  
3. Store these parameters (or your best guess) in an internal data structure.

---

## 2. Prepare and Preprocess Video
1. Decide on the resolution or whether to resize frames for efficiency.  
2. (Optional) Undistort frames if significant lens distortion is visible.  
3. Set up a method to read each frame (e.g., an iterator over the video).

---

## 3. Detect Vehicles or Lights in Each Frame
1. If using simple methods (no deep learning):
   - Use background subtraction to highlight moving objects.  
   - Color threshold for bright/red lights if it’s nighttime.  
   - Apply morphological operations to clean up noise.  
   - Label connected components and filter by size/shape.  
2. If using partial deep learning:
   - Use a pretrained object detector (like YOLO) to get bounding boxes of vehicles.  
   - Extract bounding box regions for further analysis.  
3. Keep a list of detected objects (cars or lights) for each frame.

---

## 4. Track Each Detected Car Across Frames
1. Assign IDs to detections so the same car is linked from one frame to the next.  
2. Use a simple distance-based matching or a multi-object tracker (e.g., Kalman filter, Hungarian algorithm).  
3. For each tracked car, maintain a record of its bounding boxes or specific keypoints (like taillights) over time.

---

## 5. Identify Keypoints and Correspondences (For 3D Geometry)
1. Within each tracked car, locate distinctive points (e.g., left light, right light).  
2. Match these points from one frame to the next:
   - If you have symmetrical lights, pair them by checking similarity in color/size and roughly horizontal alignment.  
3. Collect point pairs (e.g., \((L1, R1)\) at time \(t\) and \((L2, R2)\) at time \(t + \Delta t\)) for each car.

---

## 6. Classify Motion Type (Straight or Curved)
1. Use the 2D lines \((L1 \to L2)\) and \((R1 \to R2)\).  
2. If these lines suggest a single vanishing point pair that’s orthogonal when back-projected, the car is moving straight.  
3. If there is a center of rotation in the image plane, the car is turning with constant curvature.  
4. Store the motion type (straight or curved) for each car segment over the relevant frames.

---

## 7. Reconstruct 3D Position
1. Based on the motion classification:
   - **Straight Motion**: Compute the vanishing line from vanishing points and use the known distance between lights to scale the 3D setup.  
   - **Constant Curvature**: Find the center of rotation, then proceed with a similar geometry approach (using known car dimensions as constraints).  
2. Estimate the plane \(\pi\) that contains the car’s rear features.  
3. Determine how far the plane is from the camera using geometry (e.g., a theorem-of-cosines approach or direct similar triangles).  
4. Convert these results to a world coordinate (e.g., distance \(d\) from camera, orientation in yaw/pitch).

---

## 8. Compute Speed and Time-to-Impact
1. Track the 3D distance or position of the car over multiple frames.  
2. Calculate speed by \(\Delta \text{distance} / \Delta \text{time}\) or by 3D vector differences.  
3. Estimate time-to-impact by \(\text{distance} / \text{speed}\) if the car is moving toward the camera.  
4. If turning, approximate or simulate the path to see if it intersects the camera location; compute TTI if it does.



