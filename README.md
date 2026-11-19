Turkish Lira Coin Counter

A base MATLAB function that detects Turkish Lira coins (1 kr – 5 TL) in an image, classifies them by diameter matching, and calculates the total value in TL.
Features

  Reference Diameter Calibration: Refines the pixel-to-millimeter scale using the value in kurus (largestCoinKr) of the largest coin present in the image.

   Transparency & Background Estimation: Uses the alpha channel when available, or estimates dominant background colors from the image borders.

   Circular Object Filtering: Uses connected components (conncomp), aspect ratio, and area fill criteria to filter out non-coin or irregular shapes.

   Visualization & Summary Output: Overlays green circles/ellipses and coin labels on the detected coins while returning a count table (counts) and total value (totalTL).

 Coin,Value (Kurus),Diameter (mm)
1 kr,1,16.50
5 kr,5,17.50
10 kr,10,18.50
25 kr,25,20.50
50 kr,50,23.85
1 TL,100,26.15
5 TL,500,28.15

Usage : 
  [totalTL, counts] = coincounter2('coins.jpg', 100);

Arguments

   imageFile (char / string): Path to the input image file.

  largestCoinKr (numeric): Value in kurus of the largest coin present in the image (1, 5, 10, 25, 50, 100, or 500). For example, pass 100 if 1 TL is the largest coin in the photo.

Outputs

  totalTL (double): Total value of all detected coins in Turkish Lira.

   counts (table): Table containing Coin and Count columns for each supported denomination.

How It Works

  Preprocessing & Downsampling: Reads the image, converts indexed or grayscale images to RGB, and applies a stride (step) on very large images to keep processing fast.

  Foreground Masking: Builds a mask from transparency or thresholds color differences (> 18/255) against background colors estimated from the outer border pixels.

  Connected Components & Filtering: Constructs a grid graph from horizontal and vertical neighbors, finds connected regions via conncomp, and keeps candidates with minimum size (>= 14 px), aspect ratio (>= 0.8), and fill ratio (>= 0.45 * w * h).

  Scale Search & Matching: Evaluates 161 candidate scales across 96%–104% of the initial scale derived from the largest detected diameter and sizesMm(reference), choosing the scale with the lowest sum of squared millimeter errors.

  Reporting & Plotting: Prints non-zero coin counts and the total TL amount to the command window, then displays the annotated image in a new figure.

Tips & Limitations

   Plain, Contrasting Background: Busy backgrounds or colors close to the coins can cause noisy masks.

  Touching Coins & Perspective: Overlapping coins or steep camera angles can skew measured diameters; shoot from a top-down angle on a flat surface for best results.

   Accurate Largest Coin Input: Make sure largestCoinKr matches a coin that is actually present in the image so the scale stays accurate.

Requirements

   MATLAB (base functions with graph and conncomp support).
