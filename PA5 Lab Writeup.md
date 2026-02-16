# CSE 8A PA5 Lab

February 17, 2026

---

## Introduction

Welcome to your PA5 Lab!

Today you will learn about image processing using nested loops and 2D lists of tuples. You will practice working with RGB color values and learn how to transform images by modifying pixel data.

These are the core building blocks you'll need to complete PA5 successfully!

---

## Learning Goals
By the end of this lab, you will be able to:

- Work with 2D lists of tuples representing images
- Use nested for loops to iterate through every pixel in an image
- Access and modify RGB color values in tuples
- Understand how to create new images without modifying the original
- Apply mathematical transformations to color values

---

## Part 0. Environment Setup

### Step 1: Download Starter Files

**IMPORTANT: Do this first!**

You need to download the `CSE8AImage.py` file and the sample test images. You have two options:

**Option 1: Download from GitHub (Recommended)**

1. Click the green "Code" button
2. Select "Download ZIP"
3. Extract the ZIP file to your working folder
4. You should now have `CSE8AImage.py` and the sample images in your folder

**Option 2: Download from Canvas**

1. Go to Canvas and find the PA5 assignment
2. Download `CSE8AImage.py` and save it in your working folder
3. Download the sample test images and save them in the same folder

Make sure `CSE8AImage.py` is in the **same folder** as your Python files for this assignment to work.

**If you have already followed the setup slides (slides 23-24 on Mac, or slides 27-28 on Windows), you can skip to Part 1!**

Otherwise, if you aren't sure or you don't remember if you completed the setup, you can follow these steps to double check.

### Step 2: Install numpy and pillow

Before you can use the `CSE8AImage` library, you need to install two Python packages: `numpy` and `pillow`.

---

### Setup Instructions for Mac

#### Check if you have pip installed

Open the **Terminal** app and type the following command:

```bash
pip3 --version
```

If you see a version number (e.g., `pip 23.0.1`), you have pip installed. Skip to `"Install numpy and pillow for Mac"` below.

If you get an error message, you need to install `pip` first.

#### Install pip (if needed)

In Terminal, run these commands:

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

```bash
python3 get-pip.py
```

#### Install numpy and pillow for Mac

In Terminal, try the following command:

```bash
pip3 install numpy pillow
```

**If that doesn't work on your computer**, try these alternatives in order:

```bash
python3 -m pip install numpy pillow
```

```bash
python -m pip install numpy pillow
```

```bash
py -m pip install numpy pillow
```

Upon successful installation, you should see messages similar to the following:

```
Downloading numpy-2.1.3-cp311-cp311-macosx_14_0_arm64.whl (5.4 MB)
Downloading pillow-11.0.0-cp311-cp311-macosx_11_0_arm64.whl (3.0 MB)
Installing collected packages: pillow, numpy
Successfully installed numpy-2.1.3 pillow-11.0.0
```

If you already have the packages installed, you'll see messages similar to the following:

```
Requirement already satisfied: numpy in /Library/Frameworks/...
Requirement already satisfied: pillow in /Library/Frameworks/...
```

---

### Setup Instructions for Windows

#### Check if you have pip installed

Open **Command Prompt** and type the following command:

```bash
pip --version
```

If you see a version number, you have pip installed. Skip to `"Install numpy and pillow for Windows"` below.

If you get an error message, you need to install `pip` first.

#### Install pip (if needed)

In Command Prompt, run these commands:

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```

#### Install numpy and pillow for Windows

In Command Prompt, try the following command:

```bash
pip install numpy pillow
```

**If that doesn't work on your computer**, try these alternatives in order:

```bash
python -m pip install numpy pillow
```

```bash
py -m pip install numpy pillow
```

```bash
python3 -m pip install numpy pillow
```

Upon successful installation, you'll see messages similar to the following:

```
Downloading numpy-2.1.3-cp311-cp311-win_amd64.whl (5.4 MB)
Downloading pillow-11.0.0-cp311-cp311-win_amd64.whl (3.0 MB)
Installing collected packages: pillow, numpy
Successfully installed numpy-2.1.3 pillow-11.0.0
```

If you already have the packages installed, you'll see messages similar to the following:

```
Requirement already satisfied: numpy in ...
Requirement already satisfied: pillow in ...
```

---

### Testing Your Setup

To verify your setup is working correctly:

1. Open **IDLE**
2. Create a new Python file (e.g. `test_lab_5.py`) in the **same folder** where you saved `CSE8AImage.py`
3. Type the following code:

```python
from CSE8AImage import *

# This should run without errors
test_image = create_img(5, 5, (255, 0, 0))
print("Setup successful!")
```

4. Run the file (Run → Run Module)

If you see "Setup successful!" printed, you're ready to start the lab!

**If you encounter any issues**, please see a tutor or TA.

---

## Part 1: Understanding Images as 2D Lists of Tuples

In Python, we represent images as **2D lists of tuples**. Each tuple contains three numbers representing the **RGB color values** (Red, Green, Blue) of a pixel.

### What is RGB?

RGB stands for Red, Green, Blue. Each color value ranges from **0 to 255**:
- `(255, 0, 0)` is pure red
- `(0, 255, 0)` is pure green
- `(0, 0, 255)` is pure blue
- `(0, 0, 0)` is black (no color)
- `(255, 255, 255)` is white (all colors)
- `(128, 128, 128)` is gray (equal amounts of all colors)

### Example: A Small 2x2 Image

```python
image = [[(255, 0, 0), (0, 255, 0)],
         [(0, 0, 255), (255, 255, 0)]]
```

This represents a 2x2 image with 2 rows and 2 columns:

```
Visual representation with indices:

           Column 0         Column 1
         +-------------+  +-------------+
Row 0    | image[0][0] |  | image[0][1] |
         +-------------+  +-------------+
Row 1    | image[1][0] |  | image[1][1] |
         +-------------+  +-------------+

Pixel values:
image[0][0] = (255, 0, 0)   --> RED
image[0][1] = (0, 255, 0)   --> GREEN
image[1][0] = (0, 0, 255)   --> BLUE
image[1][1] = (255, 255, 0) --> YELLOW
```

### Understanding Image Indexing

To access pixels, we use **double bracket notation**: `image[row][col]`

```
image[0][0] --> Row 0, Column 0 --> (255, 0, 0)   --> RED
image[0][1] --> Row 0, Column 1 --> (0, 255, 0)   --> GREEN
image[1][0] --> Row 1, Column 0 --> (0, 0, 255)   --> BLUE
image[1][1] --> Row 1, Column 1 --> (255, 255, 0) --> YELLOW
```

**Example:**

```python
image = [[(255, 0, 0), (0, 255, 0)],
         [(0, 0, 255), (255, 255, 0)]]

# Get the pixel at row 0, column 1
pixel = image[0][1]
print(pixel)  # Output: (0, 255, 0)

# Get the pixel at row 1, column 0
pixel = image[1][0]
print(pixel)  # Output: (0, 0, 255)
```

### Accessing Color Values in a Pixel

Since each pixel is a tuple with three values `(red, green, blue)`, we can unpack it:

```python
pixel = (255, 0, 0)
(r, g, b) = pixel

print(r)  # 255 (red value)
print(g)  # 0 (green value)
print(b)  # 0 (blue value)
```

Or access by index:
```python
pixel = (255, 0, 0)
red_value = pixel[0]    # 255
green_value = pixel[1]  # 0
blue_value = pixel[2]   # 0
```

---

## Part 2: Working with Nested Loops

To process **every pixel** in an image, we need **nested for loops**:
- The **outer loop** iterates through each **row**
- The **inner loop** iterates through each **column** in that row

### Understanding `len()` with Images

When working with a 2D list like an image:
- `len(image)` gives you the **number of rows** (the height)
- `len(image[0])` or `len(image[row])` gives you the **number of columns** in that row (the width)

**Example:**
```python
image = [[(255, 0, 0), (0, 255, 0)],
         [(0, 0, 255), (255, 255, 0)]]

print(len(image))        # Output: 2 (2 rows)
print(len(image[0]))     # Output: 2 (2 columns in row 0)
print(len(image[1]))     # Output: 2 (2 columns in row 1)
```

### Example: Printing All Pixels Using `len()`

```python
image = [[(255, 0, 0), (0, 255, 0)],
         [(0, 0, 255), (255, 255, 0)]]

# Loop through all pixels using len()
for row in range(len(image)):
    for col in range(len(image[0])):
        pixel = image[row][col]
        print("Pixel at [" + str(row) + "][" + str(col) + "]: " + str(pixel))
```

Output:
```
Pixel at [0][0]: (255, 0, 0)
Pixel at [0][1]: (0, 255, 0)
Pixel at [1][0]: (0, 0, 255)
Pixel at [1][1]: (255, 255, 0)
```

**Key Points:**
- Use `range(len(image))` for the outer loop (iterates through rows)
- Use `range(len(image[0]))` for the inner loop (iterates through columns in that row)
- Access each pixel with `image[row][col]`

---

## Part 3: The CSE8AImage Library

Now that you understand how to use `len()` to iterate through images, let's learn about a helpful library that makes image processing easier!

The `CSE8AImage` library provides helpful functions that make working with images much simpler. Instead of using `len(image)` and `len(image[0])`, we can use clearer function names.

### Why Use the CSE8AImage Library?

- **Simplifies our code**: Instead of `len(image)`, we use `height(image)`
- **Easier to read**: `width(image)` is clearer than `len(image[0])`
- **More reliable**: The library handles edge cases and potential errors for us
- **Additional features**: Provides functions to load, save, and create images

### Available Functions

Here's a quick reference of the library functions you can use:

| Function | Parameters | Description |
|----------|------------|-------------|
| `load_img(filename)` | `filename`: string with path to image | Loads an image file and returns a 2D list of RGB tuples |
| `save_img(img, filename)` | `img`: 2D list of tuples<br>`filename`: string with path | Saves a 2D list of tuples as an image file |
| `create_img(height, width, color)` | `height`: integer<br>`width`: integer<br>`color`: tuple (R,G,B) | Creates a blank image of specified size filled with one color |
| `height(img)` | `img`: 2D list of tuples | Returns the height (number of rows) of the image |
| `width(img)` | `img`: 2D list of tuples | Returns the width (number of columns) of the image |

### Example Usage

```python
from CSE8AImage import *

# Create a 3x4 image (3 rows, 4 columns) filled with red
my_img = create_img(3, 4, (255, 0, 0))

# Get the dimensions
img_height = height(my_img)  # Returns 3
img_width = width(my_img)    # Returns 4

print("Height: " + str(img_height))  # Output: Height: 3
print("Width: " + str(img_width))    # Output: Width: 4
```

**Note:** Remember that `height(img)` gives you the number of rows, and `width(img)` gives you the number of columns!

### Using CSE8AImage Functions with Nested Loops

Now we can rewrite our pixel-printing example using the CSE8AImage library functions:

```python
from CSE8AImage import *

image = [[(255, 0, 0), (0, 255, 0)],
         [(0, 0, 255), (255, 255, 0)]]

# Using CSE8AImage functions - much clearer!
img_height = height(image)  # 2 rows
img_width = width(image)    # 2 columns

# Loop through all pixels using height() and width()
for row in range(img_height):
    for col in range(img_width):
        # Process pixel at [row][col]
        current_pixel = image[row][col]

        (r, g, b) = current_pixel
        # Do something with r, g, b

        print("Pixel at [" + str(row) + "][" + str(col) + "]: " + str(current_pixel))
```

**This is much more readable than using `len()`!**

**Key Points:**
- Use `range(height(img))` for the outer loop (iterates through rows)
- Use `range(width(img))` for the inner loop (iterates through columns)
- The CSE8AImage functions make your code easier to understand!

---

## Part 4: Creating New Images vs. Modifying Originals

**IMPORTANT:** When transforming images, we should **create a new image** instead of modifying the original. This is similar to how `sorted()` creates a new list instead of modifying the original with `.sort()`.

### Why Create New Images?

- Preserves the original image
- Allows us to compare before and after
- Prevents accidental data loss

### Common Approach for Image Transformations

Here's a template you can follow when writing functions that transform images:

```python
from CSE8AImage import *

def transform_image(img):
    # Step 1: Get dimensions using CSE8AImage functions
    img_height = height(img)
    img_width = width(img)
    
    # Step 2: Create new blank image using create_img()
    new_img = create_img(img_height, img_width, (0, 0, 0))
    
    # Step 3: Process each pixel with nested loops
    for row in range(img_height):
        for col in range(img_width):
            # Get original pixel
            (r, g, b) = img[row][col]
            
            # Transform the color values
            new_r = r  # modify as needed
            new_g = g  # modify as needed
            new_b = b  # modify as needed
            
            # Store in new image
            new_img[row][col] = (new_r, new_g, new_b)
    
    # Step 4: Return the new image
    return new_img
```

This is a common pattern you'll use when working with images!

---

## Part 5: Exercise - Making an Image Darker

Now let's apply what we've learned! You will write a function that makes an image darker by reducing each color value by a certain percentage.

### How Darkening Works

To make an image darker by a certain percentage:
1. Calculate a **multiplier** less than 1.0
   - For 30% darker: multiplier = `1 - (30/100)` = `0.70`
   - For 50% darker: multiplier = `1 - (50/100)` = `0.50`
2. Multiply each color value by the multiplier
3. Convert to integer using `int()`

### Example Calculation

Make a pixel 30% darker:
```python
original_pixel = (100, 150, 200)
percentage = 30
multiplier = 1 - (percentage / 100)  # 0.70

new_r = int(100 * 0.70)  # int(70.0) = 70
new_g = int(150 * 0.70)  # int(105.0) = 105
new_b = int(200 * 0.70)  # int(140.0) = 140

new_pixel = (70, 105, 140)
```

### Task

Write a function `darken_image(img, percentage)` that takes an image and a percentage, and returns a new image that is darker by that percentage.

### Guided Steps

#### Step 1: Define Your Function

Create a file called `pa5_lab.py` and write the function definition:

```python
from CSE8AImage import *

def darken_image(img, percentage):
    
    # Your code here
```

#### Step 2: Implement the Logic

Follow this structure:

```python
def darken_image(img, percentage):
    # Get image dimensions using height(img) and width(img)
    
    # Calculate the multiplier: 1 - (percentage / 100)
    
    # Create a new blank image using create_img()
    
    # Use nested loops to process each pixel
    #   - Outer loop: for row in range(img_height)
    #   - Inner loop: for col in range(img_width)

    #   - Get the pixel at img[row][col]
    #   - Unpack it into (r, g, b)

    #   - Calculate new_r = int(r * multiplier)
    #   - Calculate new_g = int(g * multiplier)
    #   - Calculate new_b = int(b * multiplier)

    #   - Store the new pixel in new_img[row][col]
    
    # TODO: Return the new image
```

#### Step 3: Test Your Function

Test with these examples:

```python
# Test 1: Single pixel, 30% darker
test1 = [[(100, 150, 200)]]
result1 = darken_image(test1, 30)
print("Test 1: " + str(result1))
# Expected: [[(70, 105, 140)]]

# Test 2: Single pixel, 50% darker
test2 = [[(200, 100, 50)]]
result2 = darken_image(test2, 50)
print("Test 2: " + str(result2))
# Expected: [[(100, 50, 25)]]

# Test 3: 2x2 image, 20% darker
test3 = [[(100, 100, 100), (200, 150, 100)],
         [(150, 200, 250), (50, 75, 25)]]
result3 = darken_image(test3, 20)
print("Test 3: " + str(result3))
# Expected: [[(80, 80, 80), (160, 120, 80)], 
#            [(120, 160, 200), (40, 60, 20)]]

# Test 4: Black pixel stays black
test4 = [[(0, 0, 0)]]
result4 = darken_image(test4, 50)
print("Test 4: " + str(result4))
# Expected: [[(0, 0, 0)]]
```

**Expected Results:**
- Test 1: `[[(70, 105, 140)]]`
- Test 2: `[[(100, 50, 25)]]`
- Test 3: `[[(80, 80, 80), (160, 120, 80)], [(120, 160, 200), (40, 60, 20)]]`
- Test 4: `[[(0, 0, 0)]]`

#### Step 4: Test with a Real Image (Optional)

If you have time, try it with a real image inside a test file (e.g. `test_lab_5.py`). Download `beach.jpg` and `cat_sand_dune.jpg` from the GitHub link.

```python
from pa5_lab import darken_image
from CSE8AImage import *

# Load an image
my_img = load_img("beach.jpg")  # Use any image file

# Make it 40% darker
darker = darken_image(my_img, 40)

# Save the result
save_img(darker, "beach_darker.jpg")
print("Darkened image saved!")
```

---

## Part 6: Lab Quiz (~15 minutes)

Make sure to review the lab activity today! The lab quiz will test material based on what you learned in this lab, including:
- Understanding RGB color values
- Working with nested loops
- Accessing pixels in a 2D list with `image[row][col]`
- Unpacking tuples
- Using CSE8AImage library functions
- Creating new images vs. modifying originals

---

## Challenge Problem: Invert Colors

**Note:** This challenge problem will NOT be tested on the lab quiz today, but it gives you more code writing practice for your PA!

In this problem, let's practice a different kind of image transformation!

### Task

Write a function `invert_colors(img)` that takes an image and returns a new image where all the colors are inverted (like a photo negative).

To invert a color value, subtract it from 255:
- If red = 100, inverted red = 255 - 100 = 155
- If green = 200, inverted green = 255 - 200 = 55
- Black (0, 0, 0) becomes white (255, 255, 255)
- White (255, 255, 255) becomes black (0, 0, 0)

### Example

```python
original_pixel = (100, 150, 200)
# After inversion:
new_r = 255 - 100  # 155
new_g = 255 - 150  # 105
new_b = 255 - 200  # 55
new_pixel = (155, 105, 55)
```

### Guided Steps

#### Step 1: Define Your Function

Create a file called `pa5_lab_challenge.py` and write the function definition:

```python
from CSE8AImage import *

def invert_colors(img):

    # Your code here
```

#### Step 2: Implement the Logic

This is similar to `darken_image`, but instead of multiplying:
- For each color value, calculate: 255 - value

```python
def invert_colors(img):
    # Get image dimensions
    
    # Create a new blank image
    
    # Use nested loops to process each pixel
    #   - Get (r, g, b) from img[row][col]
    #   - Calculate new_r = 255 - r
    #   - Calculate new_g = 255 - g
    #   - Calculate new_b = 255 - b
    #   - Store new pixel (new_r, new_g, new_b) in new_img[row][col]
    
    # Return the new image
```

#### Step 3: Test Your Function

```python
# Test 1: Single pixel
test1 = [[(100, 150, 200)]]
result1 = invert_colors(test1)
print("Test 1: " + str(result1))
# Expected: [[(155, 105, 55)]]

# Test 2: Black becomes white
test2 = [[(0, 0, 0)]]
result2 = invert_colors(test2)
print("Test 2: " + str(result2))
# Expected: [[(255, 255, 255)]]

# Test 3: White becomes black
test3 = [[(255, 255, 255)]]
result3 = invert_colors(test3)
print("Test 3: " + str(result3))
# Expected: [[(0, 0, 0)]]

# Test 4: 2x2 image
test4 = [[(100, 50, 200), (200, 100, 50)],
         [(150, 75, 25), (50, 200, 100)]]
result4 = invert_colors(test4)
print("Test 4: " + str(result4))
# Expected: [[(155, 205, 55), (55, 155, 205)], 
#            [(105, 180, 230), (205, 55, 155)]]
```

**Expected Results:**
- Test 1: `[[(155, 105, 55)]]`
- Test 2: `[[(255, 255, 255)]]`
- Test 3: `[[(0, 0, 0)]]`
- Test 4: `[[(155, 205, 55), (55, 155, 205)], [(105, 180, 230), (205, 55, 155)]]`
