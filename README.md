# 1. Basic Image Operations.
#     - a. Read and display image.
#     - b. Resize a given image.
#     - c. Convert a given color image into a corresponding gray-scale image.
#     - d. Convert a given color/gray-scale image into black & white image and draw the image profile.
#     - e. Separate a given color image into three R, G & B planes.
#     - f. Create a color image using separate three R, G and B planes.
#     - g. Write given 2-D data in image file.


import cv2
import numpy as np
import matplotlib.pyplot as plt
import os

# Create result directory
os.makedirs("result/program1", exist_ok=True)

# Load input image
img = cv2.imread("result/input.png")
if img is None:
    # Fallback: generate a sample image
    img = np.zeros((300, 300, 3), dtype=np.uint8)
    img[50:250, 50:250] = [0, 0, 255]  # Blue square
    img[100:200, 100:200] = [0, 255, 0]  # Green square
cv2.imwrite("result/program1/input.jpg", img)

# a. Read and display image
img = cv2.imread("result/program1/input.jpg")

# b. Resize a given image
resized = cv2.resize(img, (150, 150))
cv2.imwrite("result/program1/resized.jpg", resized)

# c. Convert a given color image into a corresponding gray-scale image
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
cv2.imwrite("result/program1/grayscale.jpg", gray)

# d. Convert a given color/gray-scale image into black & white image
_, bw = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)
cv2.imwrite("result/program1/black_white.jpg", bw)

# Image profile
row_profile = gray.mean(axis=1)
col_profile = gray.mean(axis=0)

# e. Separate a given color image into three R, G & B planes
b, g, r = cv2.split(img)
cv2.imwrite("result/program1/b_plane.jpg", b)
cv2.imwrite("result/program1/g_plane.jpg", g)
cv2.imwrite("result/program1/r_plane.jpg", r)

# f. Create a color image using separate three R, G and B planes
merged = cv2.merge((b, g, r))
cv2.imwrite("result/program1/merged_rgb.jpg", merged)

# g. Write given 2-D data in image file
cv2.imwrite("result/program1/output_2d.jpg", gray)

plt.plot(row_profile, label='Row profile')
plt.plot(col_profile, label='Column profile')
plt.title('d. Image profile')
plt.xlabel('Index')
plt.ylabel('Mean intensity')
plt.legend()
plt.grid(True)
plt.savefig("result/program1/image_profile.png")
plt.close()

print("All outputs saved to result/program1/")


---------

# 2. Point Processing Methods.
#     - a. Obtain negative image.
#     - b. Obtain flip image.
#     - c. Thresholding.
#     - d. Contrast stretching.


import cv2
import numpy as np
import os

os.makedirs("result/program2", exist_ok=True)

# Load input image
img = cv2.imread("result/input.png", 0)  # Load as grayscale
if img is None:
    # Fallback: Generate synthetic image
    img = np.linspace(0, 255, 256, dtype=np.uint8).reshape(16, 16)
    img = cv2.resize(img, (256, 256), interpolation=cv2.INTER_NEAREST)
cv2.imwrite("result/program2/original.jpg", img)

# a. Negative image
negative = 255 - img
cv2.imwrite("result/program2/negative.jpg", negative)

# b. Flip image
hor_flipped = cv2.flip(img, 1)  # horizontal flip
cv2.imwrite("result/program2/horizontal_flipped.jpg", hor_flipped)
ver_flipped = cv2.flip(img, 0)  # vertical flip
cv2.imwrite("result/program2/vertical_flipped.jpg", ver_flipped)

# c. Thresholding
_, thresholded = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)
cv2.imwrite("result/program2/thresholded.jpg", thresholded)

# d. Contrast stretching
min_val, max_val = np.min(img), np.max(img)
stretched = ((img - min_val) / (max_val - min_val) * 255).astype(np.uint8)
cv2.imwrite("result/program2/contrast_stretched.jpg", stretched)

print("All outputs saved to result/program2/")


----------

# 3. Image Arithmetic Operations.
#     - a. Addition of two images.
#     - b. Subtract one image from other image.
#     - c. Calculate mean value of image.
#     - d. Different brightness by changing mean value.


import cv2
import numpy as np
import os

os.makedirs("result/program3", exist_ok=True)

# Create sample images
img1 = np.zeros((256, 256), dtype=np.uint8)
cv2.rectangle(img1, (50, 50), (150, 150), 100, -1)

img2 = np.zeros((256, 256), dtype=np.uint8)
cv2.circle(img2, (150, 150), 70, 150, -1)

cv2.imwrite("result/program3/image1.jpg", img1)
cv2.imwrite("result/program3/image2.jpg", img2)

# a. Addition of two images
added = cv2.add(img1, img2)
cv2.imwrite("result/program3/added.jpg", added)

# b. Subtract one image from other image
subtracted1 = cv2.subtract(img1, img2)
subtracted2 = cv2.subtract(img2, img1)
cv2.imwrite("result/program3/subtracted_img1_img2.jpg", subtracted1)
cv2.imwrite("result/program3/subtracted_img2_img1.jpg", subtracted2)

# c. Calculate mean value of image
mean1 = np.mean(img1)
mean2 = np.mean(img2)
with open("result/program3/mean_values.txt", "w") as f:
    f.write(f"Mean of image1: {mean1}\n")
    f.write(f"Mean of image2: {mean2}\n")

# d. Different brightness by changing mean value
# Use calculated mean values (mean1, mean2) to adjust brightness
# Brighten img1 by adding mean2, darken by subtracting mean2
brightened = cv2.add(img1, int(mean2))
darkened = cv2.subtract(img1, int(mean2))

cv2.imwrite("result/program3/brightened.jpg", brightened)
cv2.imwrite("result/program3/darkened.jpg", darkened)

print("All outputs saved to result/program3/")

-------
# 4. Image Logical Operations.
#     - a. AND operation between two images.
#     - b. OR operation between two images.
#     - c. Calculate intersection of two images.
#     - d. Water marking using X-OR operation.
#     - e. NOT operation (negative image).


import cv2
import numpy as np
import os

os.makedirs("result/program4", exist_ok=True)

# Create sample binary images
img1 = np.zeros((200, 200), dtype=np.uint8)
cv2.rectangle(img1, (50, 50), (120, 120), 255, -1)

img2 = np.zeros((200, 200), dtype=np.uint8)
cv2.circle(img2, (100, 100), 50, 255, -1)

cv2.imwrite("result/program4/image1.jpg", img1)
cv2.imwrite("result/program4/image2.jpg", img2)

# a. AND operation between two images
and_img = cv2.bitwise_and(img1, img2)
cv2.imwrite("result/program4/and.jpg", and_img)

# b. OR operation between two images
or_img = cv2.bitwise_or(img1, img2)
cv2.imwrite("result/program4/or.jpg", or_img)

# c. Calculate intersection of two images
intersection = cv2.bitwise_and(img1, img2)
cv2.imwrite("result/program4/intersection.jpg", intersection)

# d. Water marking using X-OR operation
logo = np.zeros((200, 200), dtype=np.uint8)
cv2.putText(logo, "DIP", (40, 110), cv2.FONT_HERSHEY_SIMPLEX, 1.5, 255, 3)
watermarked = cv2.bitwise_xor(img1, logo)
extracted = cv2.bitwise_xor(watermarked, logo)
cv2.imwrite("result/program4/logo.jpg", logo)
cv2.imwrite("result/program4/watermarked_xor.jpg", watermarked)
cv2.imwrite("result/program4/extracted_xor.jpg", extracted)

# e. NOT operation (negative image)
not_img1 = cv2.bitwise_not(img1)
cv2.imwrite("result/program4/not_image1.jpg", not_img1)

print("All outputs saved to result/program4/")

-------
# 5. Histogram Calculation and Equalization.
#     - a. Using inbuilt function.
#     - b. Without using inbuilt function.


import cv2
import numpy as np
import os
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

os.makedirs("result/program5", exist_ok=True)

# Load image
img = cv2.imread("result/input.png")
if img is None:
    for p in ["input.jpg", "result/program1/input.jpg", "result/program5/input_ref.jpg", "result/program7/input_ref.jpg"]:
        img = cv2.imread(p)
        if img is not None:
            break
if img is None:
    img = np.zeros((256, 256, 3), dtype=np.uint8)
    cv2.rectangle(img, (50, 50), (200, 200), (120, 180, 220), -1)

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
cv2.imwrite("result/program5/input_img.jpg", gray)
cv2.imwrite("result/program5/grayscale.jpg", gray)

# a
eq_builtin = cv2.equalizeHist(gray)
cv2.imwrite("result/program5/equalized_builtin.jpg", eq_builtin)

# b
cdf = cv2.calcHist([gray], [0], None, [256], [0, 256]).flatten().cumsum()
cdf_min = cdf[cdf != 0].min() if np.any(cdf != 0) else 0
cdf_max = cdf[-1]
denom = cdf_max - cdf_min
if denom <= 0:
    lut = np.zeros(256, dtype=np.uint8)
else:
    lut = ((cdf - cdf_min) * 255.0 / denom)
    lut = np.clip(lut, 0, 255).astype(np.uint8)
img_eq_manual = lut[gray]
cv2.imwrite("result/program5/equalized_manual.jpg", img_eq_manual)

# Plot equalization calculation graphs
hist_original = cv2.calcHist([gray], [0], None, [256], [0, 256]).flatten()
hist_equalized = cv2.calcHist([img_eq_manual], [0], None, [256], [0, 256]).flatten()

# Original histogram
fig, ax = plt.subplots(figsize=(6, 5))
ax.plot(hist_original, color='black')
ax.set_title('Original Histogram')
ax.set_xlabel('Pixel Intensity')
ax.set_ylabel('Frequency')
ax.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig("result/program5/original_histogram.png")
plt.close()

# Equalized histogram
fig, ax = plt.subplots(figsize=(6, 5))
ax.plot(hist_equalized, color='purple')
ax.set_title('Equalized Histogram (Manual)')
ax.set_xlabel('Pixel Intensity')
ax.set_ylabel('Frequency')
ax.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig("result/program5/equalized_histogram.png")
plt.close()

print("All outputs saved to result/program5/")

-------
# 6. Geometric Transformations.
#     - a. Translation.
#     - b. Scaling.
#     - c. Rotation.
#     - d. Shrinking.
#     - e. Zooming.


import cv2
import numpy as np
import os

os.makedirs("result/program6", exist_ok=True)

# Create sample image
img = np.zeros((150, 150, 3), dtype=np.uint8)
cv2.rectangle(img, (30, 30), (90, 90), (0, 255, 0), -1)
cv2.imwrite("result/program6/original.jpg", img)

# a. Translation
h, w = img.shape[:2]
M = np.float32([[1, 0, 50], [0, 1, 30]])
translated = cv2.warpAffine(img, M, (w, h))
cv2.imwrite("result/program6/translated.jpg", translated)

# b. Scaling
scaled = cv2.resize(img, None, fx=2.5, fy=1.5, interpolation=cv2.INTER_LINEAR)
cv2.imwrite("result/program6/scaled.jpg", scaled)

# c. Rotation
M_rot = cv2.getRotationMatrix2D((w/2, h/2), 45, 1)
rotated = cv2.warpAffine(img, M_rot, (w, h))
cv2.imwrite("result/program6/rotated.jpg", rotated)

# d. Shrinking
shrunk = cv2.resize(img, None, fx=0.5, fy=0.5, interpolation=cv2.INTER_AREA)
cv2.imwrite("result/program6/shrunk.jpg", shrunk)

# e. Zooming
zoomed = cv2.resize(img, None, fx=3.0, fy=3.0, interpolation=cv2.INTER_CUBIC)
cv2.imwrite("result/program6/zoomed.jpg", zoomed)

print("All outputs saved to result/program6/")

--------
# 7. Noise Models and Restoration.
#     - a. Image restoration.
#     - b. Remove salt and pepper noise.
#     - c. Minimize Gaussian noise.
#     - d. Median filter and Wiener filter.


import cv2
import numpy as np
import os
import random
from scipy.signal import wiener

random.seed(42)
np.random.seed(42)

os.makedirs("result/program7", exist_ok=True)

# Load input image
img = cv2.imread("result/input.png", 0)
if img is None:
    for p in ["input.jpg", "result/program1/input.jpg", "result/program5/grayscale.jpg"]:
        if os.path.exists(p):
            img = cv2.imread(p, 0)
            if img is not None:
                break
if img is None:
    img = np.zeros((256, 256), dtype=np.uint8)
    cv2.rectangle(img, (50, 50), (200, 200), 150, -1)

cv2.imwrite("result/program7/original.jpg", img)

# a. Image restoration - degrade original (blur + noise)
degraded = cv2.GaussianBlur(img, (5, 5), 1.5)
noise = np.random.normal(0, 5, img.shape)
degraded = np.clip(degraded.astype(float) + noise, 0, 255).astype(np.uint8)
cv2.imwrite("result/program7/degraded.jpg", degraded)
restored = wiener(degraded.astype(np.float32), mysize=5, noise=None)
restored = np.clip(restored, 0, 255).astype(np.uint8)
cv2.imwrite("result/program7/restored.jpg", restored)

# b. Add and Remove Salt and pepper noise
sp_noisy = img.copy()
total = img.size
n_s = int(0.05 * total * 0.5)
n_p = int(0.05 * total * 0.5)
for _ in range(n_s):
    r, c = random.randint(0, img.shape[0]-1), random.randint(0, img.shape[1]-1)
    sp_noisy[r, c] = 255
for _ in range(n_p):
    r, c = random.randint(0, img.shape[0]-1), random.randint(0, img.shape[1]-1)
    sp_noisy[r, c] = 0
cv2.imwrite("result/program7/salt_pepper_noisy.jpg", sp_noisy)
sp_removed = cv2.medianBlur(sp_noisy, 3)
cv2.imwrite("result/program7/sp_removed.jpg", sp_removed)

# c. Minimize Gaussian noise
g_noisy = np.clip(img.astype(float) + np.random.normal(0, 10, img.shape), 0, 255).astype(np.uint8)
cv2.imwrite("result/program7/gaussian_noisy.jpg", g_noisy)

# Minimize Gaussian noise using Gaussian blur
g_minimized = cv2.GaussianBlur(g_noisy, (5, 5), 0)
cv2.imwrite("result/program7/gaussian_minimized.jpg", g_minimized)

# d. Median filter  on Salt & Pepper noise
sp_median = cv2.medianBlur(sp_noisy, 3)
cv2.imwrite("result/program7/sp_median.jpg", sp_median)
# d. Wiener filter on Salt & Pepper noise
sp_wiener = np.clip(wiener(sp_noisy.astype(np.float32), mysize=5, noise=None), 0, 255).astype(np.uint8)
cv2.imwrite("result/program7/sp_wiener.jpg", sp_wiener)

print("All outputs saved to result/program7/")

--------
# 8. Spatial Filtering.
#     - a. Understand 1-D and 2-D convolution process.
#     - b. Use 3x3 mask for low pass filter and high pass filter.


import cv2
import numpy as np
import os
import random

random.seed(42)
np.random.seed(42)

os.makedirs("result/program8", exist_ok=True)

# Load image
img = cv2.imread("result/input.png", 0)  # Load as grayscale
if img is None:
    for p in ["input.jpg", "result/program1/input.jpg", "result/program5/grayscale.jpg"]:
        if os.path.exists(p):
            img = cv2.imread(p, 0)
            if img is not None: break
if img is None:
    img = np.zeros((128,128),dtype=np.uint8); cv2.rectangle(img,(20,20),(100,100),255,-1)

cv2.imwrite("result/program8/original.jpg", img)
imgf = img.astype(np.float32)

# 1D conv
k1 = np.array([1,2,1],dtype=np.float32)/4.0
conv1d = np.convolve(imgf[0,:], k1, mode='same')

# LP
klp = np.ones((3,3),dtype=np.float32)/9.0
lp = cv2.filter2D(imgf,-1,klp)
cv2.imwrite("result/program8/low_pass.jpg", lp.astype(np.uint8))
# HP
khp = np.array([[-1,-1,-1],[-1,8,-1],[-1,-1,-1]],dtype=np.float32)
hp = cv2.filter2D(imgf,-1,khp)
hp = np.clip(hp,0,255).astype(np.uint8)
cv2.imwrite("result/program8/high_pass.jpg", hp)

print("All outputs saved to result/program8/")

-------
# 9. Frequency Domain Filtering.
#     - a. Apply FFT on given image.
#     - b. Perform low pass and high pass filtering in frequency domain.
#     - c. Apply IFFT to reconstruct image.


import cv2
import numpy as np
import os

os.makedirs("result/program9", exist_ok=True)

img = cv2.imread("result/input.png", 0)  # Load as grayscale
if img is None:
    for p in ["input.jpg","result/program1/input.jpg","result/program5/grayscale.jpg"]:
        if os.path.exists(p):
            img = cv2.imread(p,0)
            if img is not None: break
if img is None:
    img = np.zeros((256,256),dtype=np.uint8); cv2.rectangle(img,(50,50),(200,200),255,-1)

cv2.imwrite("result/program9/original.jpg", img)

# a. Apply FFT on given image
f = np.fft.fft2(img); fshift = np.fft.fftshift(f)
mag = 20*np.log(np.abs(fshift)+1e-10)
mag = np.clip(mag,0,255).astype(np.uint8)
cv2.imwrite("result/program9/magnitude_spectrum.png", mag)

# b. Perform low pass and high pass filtering in frequency domain
r,c = img.shape; cr,cc = r//2,c//2
mask_lp = np.zeros((r,c),np.uint8); cv2.circle(mask_lp,(cc,cr),30,255,-1)
mask_hp = cv2.bitwise_not(mask_lp)

fsl = fshift*(mask_lp/255.0); fsh = fshift*(mask_hp/255.0)

# c. Apply IFFT to reconstruct image
il = np.abs(np.fft.ifft2(np.fft.ifftshift(fsl))); ih = np.abs(np.fft.ifft2(np.fft.ifftshift(fsh)))
il = np.clip(il,0,255).astype(np.uint8); ih = np.clip(ih,0,255).astype(np.uint8)
cv2.imwrite("result/program9/low_pass_filtered.jpg", il)
cv2.imwrite("result/program9/high_pass_filtered.jpg", ih)

# Save filter masks
cv2.imwrite("result/program9/low_pass_mask.jpg", mask_lp)
cv2.imwrite("result/program9/high_pass_mask.jpg", mask_hp)

print("All outputs saved to result/program9/")

------
# 10. Edge Detection.
#     - Edge detection using different edge detection mask.


import cv2
import numpy as np
import os

os.makedirs("result/program10", exist_ok=True)

img = cv2.imread("result/input.png", 0)  # Load as grayscale
if img is None:
    for p in ["input.jpg","result/program1/input.jpg","result/program5/grayscale.jpg"]:
        if os.path.exists(p):
            img = cv2.imread(p,0)
            if img is not None: break
if img is None:
    img = np.zeros((256,256),dtype=np.uint8); cv2.rectangle(img,(50,50),(200,200),255,-1)

cv2.imwrite("result/program10/original.jpg", img)

# Sobel
sx = cv2.Sobel(img,cv2.CV_64F,1,0,ksize=3); sy = cv2.Sobel(img,cv2.CV_64F,0,1,ksize=3)
sob = np.clip(np.hypot(sx,sy),0,255).astype(np.uint8)

# Prewitt
kx=np.array([[1,1,1],[0,0,0],[-1,-1,-1]]); ky=np.array([[-1,0,1],[-1,0,1],[-1,0,1]])
px=cv2.filter2D(img,-1,kx); py=cv2.filter2D(img,-1,ky)
pre = np.clip(np.hypot(px.astype(float),py.astype(float)),0,255).astype(np.uint8)

# Roberts
rx=np.array([[1,0],[0,-1]]); ry=np.array([[0,1],[-1,0]])
rxv=cv2.filter2D(img,-1,rx); ryv=cv2.filter2D(img,-1,ry)
rob = np.clip(np.hypot(rxv.astype(float),ryv.astype(float)),0,255).astype(np.uint8)

# Gaussian (Laplacian of Gaussian - LoG)
# First apply Gaussian blur, then Laplacian
gaussian_blur = cv2.GaussianBlur(img, (5, 5), 1.4)
log = cv2.Laplacian(gaussian_blur, cv2.CV_64F, ksize=3)
log = np.clip(np.abs(log), 0, 255).astype(np.uint8)
cv2.imwrite("result/program10/gaussian_log.jpg", log)

# Canny
can = cv2.Canny(img,50,150)
cv2.imwrite("result/program10/sobel.jpg", sob)
cv2.imwrite("result/program10/prewitt.jpg", pre)
cv2.imwrite("result/program10/roberts.jpg", rob)
cv2.imwrite("result/program10/canny.jpg", can)

print("All outputs saved to result/program10/")

-----
# 11. Morphological Operations.
#     - Image morphological operations: erosion and dilation.


import cv2
import numpy as np
import os

os.makedirs("result/program11", exist_ok=True)

img = cv2.imread("result/input.png", 0)  # Load as grayscale
if img is None:
    for p in ["input.jpg","result/program1/input.jpg","result/program5/grayscale.jpg"]:
        if os.path.exists(p):
            img = cv2.imread(p,0)
            if img is not None: break
if img is None:
    img = np.zeros((200,200),dtype=np.uint8); cv2.putText(img,'DIP',(40,120),cv2.FONT_HERSHEY_SIMPLEX,2.5,255,5)

cv2.imwrite("result/program11/original.jpg", img)

k = np.ones((5,5),np.uint8)
er = cv2.erode(img,k,iterations=1)
di = cv2.dilate(img,k,iterations=1)
cv2.imwrite("result/program11/erosion.jpg", er)
cv2.imwrite("result/program11/dilation.jpg", di)

print("All outputs saved to result/program11/")

------

