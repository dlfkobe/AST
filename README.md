# Image Sampling and Analysis Script

## Overview
This Python script is designed for generating, analyzing, and sampling images with multiple categories of objects. It creates a synthetic image with specified categories, calculates category frequencies and weights, generates a probability map for sampling, and then samples patches from the image based on the probability map. The script also provides functions for visualizing the image, masks, and sampling results.

## Requirements
- **Python Libraries**:
  - `numpy`: For numerical operations and array manipulation.
  - `matplotlib`: For plotting and visualizing the results.
  - `skimage`: For image processing tasks such as resizing and generating masks.
  - `tqdm`: For displaying progress bars during loops.

## Installation
To install the required libraries, you can use `pip`:
```bash
pip install numpy matplotlib scikit-image tqdm
```

## Usage
1. **Configuration**:
   - Modify the parameters at the beginning of the script according to your needs. For example, you can change the image size (`H`, `W`), patch size (`h`, `w`), tile size (`Ht`, `Wt`), number of samples (`N`), etc.
   - Define the categories (`cats`) and the background ratio (`bg_ratio_base`).

2. **Run the Script**:
   - Save the script with a `.py` extension (e.g., `image_sampling.py`).
   - Open a terminal and navigate to the directory where the script is saved.
   - Run the script using the following command:
   ```bash
   python image_sampling.py
   ```

## Code Structure

### 1. Problem Setup
- **Image Size**: Defines the height (`H`) and width (`W`) of the image.
- **Background Ratio**: Sets the base background ratio (`bg_ratio_base`).
- **Categories**: Specifies the list of categories (`cats`) and calculates the number of categories (`C`).

### 2. Program Setup
- **Patch and Tile Sizes**: Defines the patch size (`h`, `w`) and tile size (`Ht`, `Wt`).
- **Exclusion and Power Parameters**: Sets the parameters for excluding background (`exclude_bg`), power (`power`), and temperature (`temperature`).
- **Data Types**: Specifies the data types for indices, counts, scores, values, and plots.

### 3. Sampling Program Setup
- **Number of Samples**: Defines the number of samples (`N`).
- **Size and Rotation Variation**: Sets the size variation range (`size_variation_range`) and rotation range (`rotation_range`).

### 4. Generating Masks and Image
- **Category Ratios**: Generates random category ratios (`cat_ratios`) based on the Dirichlet distribution.
- **Image Generation**: Generates a synthetic image by resizing a cat image from `skimage.data`.
- **Mask Generation**: Generates masks for each category by drawing ellipses randomly on the image.

### 5. Mask API
- **Tile Mask Retrieval**: Provides three functions (`get_tile_mask_real`, `get_tile_mask_gen`, `get_tile_mask_abs`) for retrieving tile masks from different data structures.
- **Default Mask Retrieval Function**: Sets the default mask retrieval function (`get_tile_mask`).

### 6. Category Frequency and Weights
- **Global Category Count**: Calculates the global category count (`global_category_count`) by iterating over all tiles.
- **Global Category Frequency**: Calculates the global category frequency (`global_category_frequency`) based on the global category count.
- **Category Weights**: Calculates the category weights (`category_weights`) by taking the reciprocal of the global category frequency and normalizing them.

### 7. Tile Processing
- **Tile Mask Extension**: Defines a function (`get_tile_maskx`) for retrieving extended tile masks.
- **Cumulative Difference Image (CDI)**: Calculates the CDI (`get_tile_cdix`) for each tile.
- **Tile Count**: Calculates the tile count (`get_tile_count`) based on the CDI.
- **Tile Score**: Calculates the tile score (`get_tile_score`) based on the tile count and category weights.

### 8. Probability Map Generation
- **Score Map**: Calculates the score map (`score_map`) by summing the tile scores over all tiles.
- **Probability Map**: Calculates the probability map (`probability_map`) by normalizing the score map.

### 9. Sampling
- **Batch Sampling**: Samples `N` patches from the image based on the probability map.
- **Single-Shot Sampling**: Samples a single patch from the image based on the probability map.

### 10. Visualization
- **Mask and Probability Map Visualization**: Visualizes the mask and probability map using `matplotlib`.
- **Sampling Results Visualization**: Visualizes the sampling results by overlaying the sampled patches on the mask.

## Key Functions

### `ecoimshow(image, ax=None, dtype=None, Hp=Hp, Wp=Wp, axis_off=True, aspect_equal=True, **kwargs)`
- **Purpose**: Resizes and displays an image using `matplotlib.imshow`.
- **Parameters**:
  - `image`: The input image.
  - `ax`: The `matplotlib` axes object.
  - `dtype`: The data type of the image.
  - `Hp`, `Wp`: The height and width of the resized image.
  - `axis_off`: Whether to turn off the axes.
  - `aspect_equal`: Whether to set the aspect ratio to equal.
  - `**kwargs`: Additional keyword arguments for `matplotlib.imshow`.

### `multiple_bar_chart(series, ax=None, margin=0.1, text_format="{:}", ticks=None)`
- **Purpose**: Plots a multiple bar chart using `matplotlib`.
- **Parameters**:
  - `series`: A dictionary containing the data series.
  - `ax`: The `matplotlib` axes object.
  - `margin`: The margin between the bars.
  - `text_format`: The format string for the bar labels.
  - `ticks`: The tick labels for the x-axis.

### `sample_tile(row, col)`
- **Purpose**: Samples patches from a tile based on the tile score and probability map.
- **Parameters**:
  - `row`, `col`: The row and column indices of the tile.
- **Returns**: An array of sampled patch coordinates.

### `sample_patch(y, x, hs, ws, rot, order=1)`
- **Purpose**: Samples a patch from the image at the specified coordinates with the given size and rotation.
- **Parameters**:
  - `y`, `x`: The center coordinates of the patch.
  - `hs`, `ws`: The height and width of the patch.
  - `rot`: The rotation angle of the patch.
  - `order`: The interpolation order for resampling.
- **Returns**: The sampled patch.

## Notes
- The script uses the `tqdm` library to display progress bars during loops, which can be useful for monitoring the progress of long-running operations.
- Some parts of the script are commented out, such as the evaluation functions for comparing with golden results. You can uncomment these parts if you have the corresponding golden data files.
- The script uses the `skimage` library for image processing tasks, which provides efficient and flexible functions for resizing, drawing shapes, and sampling patches.

## License
This script is released under the [MIT License](https://opensource.org/licenses/MIT). You are free to use, modify, and distribute it for any purpose.
