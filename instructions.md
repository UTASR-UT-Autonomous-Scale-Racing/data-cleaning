# Data Cleaning Strategies :contentReference[oaicite:0]{index=0}

## 1. CLAHE (Contrast Limited Adaptive Histogram Equalization)

- **Explanation:** Standard Histogram Equalization often blows out the sky to see the ground. CLAHE breaks the image into small tiles and equalizes them individually.
- **The Logic:** This ensures that if half the track is in a tunnel and half is in the sun, both segments have visible edges for the model to detect. It prevents the model from being "blinded" by harsh RC track lighting.
- **Resources:** OpenCV Histograms - CLAHE.

## 2. Laplacian Filtering (Blur Detection)

- **Explanation:** The Laplacian operator calculates the "sharpness" of an image by looking at the intensity of edges.
- **The Logic:** High-speed RC cars vibrate intensely. A sharp image has high variance; a blurry one has low variance. You set a threshold and automatically delete frames that are too "mushy" for the model to learn features.
- **Resources:** Blur detection with OpenCV.

## 3. SSIM Culling (Structural Similarity Index)

- **Explanation:** SSIM looks at "perceptual" changes like texture and contrast between two consecutive frames.
- **The Logic:** If your RC car is idling at the start line, you don't need 300 identical frames. SSIM identifies frames that are 95%+ identical so you can delete the extras.
- **Resources:** Scikit-Image Structural Similarity.

## 4. PCA (Redundancy & Diversity Analysis)

- **Explanation:** PCA reduces the high-dimensional pixel data (thousands of pixels) into a few "Principal Components" that represent the most significant visual variations in your dataset.
- **The Logic:**
  - **Clustering:** If you plot your images on a 2D PCA graph, similar images (e.g., all "Left Turns") group together.
  - **Balancing:** If one cluster is massive (Straightaways) and another is tiny (Chicanes), you can delete frames from the massive cluster to "even out" the dataset.
- **Resources:** Scikit-Learn PCA Documentation.  
  https://math.stackexchange.com/questions/1520832/real-life-examples-for-eigenvalues-eigenvectors/3985012#3985012

---

## Summary Table

| Step | Technique    | Purpose                                   | Target Metric            |
| ---- | ------------ | ----------------------------------------- | ------------------------ |
| 1    | CLAHE        | Normalizes lighting/shadows.              | Balanced Color Histogram |
| 2    | Laplacian    | Removes motion-blurred frames.            | Variance Threshold > 100 |
| 3    | SSIM Culling | Removes adjacent near-duplicates.         | Similarity Score < 0.90  |
| 4    | PCA Culling  | Ensures even distribution of track types. | Even Cluster Density     |
