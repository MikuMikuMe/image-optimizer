# Image-Optimizer

Creating an image optimizer in Python involves several steps including loading the image, compressing it, and saving the optimized version. Popular libraries for this purpose are `PIL` (Python Imaging Library) via the `Pillow` fork and `io` for handling streams. We will also make use of `os` and `sys` for file handling and error management. Here’s a complete Python program that achieves this task:

```python
from PIL import Image, ImageOps
import os
import sys

def optimize_image(input_path, output_path, quality=85):
    """
    Optimize the given image and save it to the output path.
    
    :param input_path: Path to the input image file
    :param output_path: Path where the optimized image will be saved
    :param quality: Quality setting for the saved optimized image (1-100)
    """
    try:
        # Open the image file using PIL's Image
        with Image.open(input_path) as img:
            print(f"Original Image Size: {img.size} - Format: {img.format}")
            
            # Automatically convert all images to RGB, to ensure compatibility
            if img.mode in ("RGBA", "P"):
                img = img.convert("RGB")
                print(f"Converted image mode to RGB")

            # Use ImageOps to automatically adjust the image
            img = ImageOps.exif_transpose(img)

            # Save the image in the given output path with optimization
            img.save(output_path, optimize=True, quality=quality)
            print(f"Image optimized and saved to {output_path} with quality {quality}.")

    except IOError as e:
        print(f"IOError: {e}. Could not read/write image file.")
    except Exception as e:
        print(f"Unexpected error: {e}")

def batch_optimize_images(input_directory, output_directory, quality=85):
    """
    Optimize all the images in the input directory and save them to the output directory.
    
    :param input_directory: Directory where the input images are stored
    :param output_directory: Directory where the optimized images will be stored
    :param quality: Quality setting for the saved optimized images (1-100)
    """
    try:
        # Check if output directory exists; if not, create it
        if not os.path.exists(output_directory):
            os.makedirs(output_directory)
            print(f"Created output directory: {output_directory}")

        # Loop over all files in the input directory
        for filename in os.listdir(input_directory):
            if filename.lower().endswith(('.png', '.jpg', '.jpeg', '.gif')):
                input_path = os.path.join(input_directory, filename)
                output_path = os.path.join(output_directory, filename)

                # Call optimize_image for each file
                optimize_image(input_path, output_path, quality)

    except OSError as e:
        print(f"OSError: {e}. Could not create directory or read file.")
    except Exception as e:
        print(f"Unexpected error: {e}")

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: image_optimizer.py <input_directory> <output_directory> [<quality>]")
    else:
        input_dir = sys.argv[1]
        output_dir = sys.argv[2]
        quality = int(sys.argv[3]) if len(sys.argv) > 3 else 85

        batch_optimize_images(input_dir, output_dir, quality)
```

### Key Features of This Program

1. **Image Loading and Processing**: The `Pillow` library is used to open and read image files. We address images with alpha channels or palettes by converting them to RGB.

2. **Optimization Handling**: Images are saved with the `optimize=True` option, and a quality level is specified to balance file size and quality.

3. **Batch Processing**: The script can handle multiple image files at once, optimizing all images in a specified input directory and saving the optimized versions to an output directory.

4. **Error Handling**: The script includes comprehensive error handling to catch and explicitly print any issues encountered during image processing or file handling.

5. **Ease of Use**: The command-line interface allows users to easily specify input and output directories, as well as the quality parameter.

This tool is effective for preparing images for the web, aiming to reduce loading times and improve overall page speed, positively impacting SEO.