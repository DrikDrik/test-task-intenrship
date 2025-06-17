# test-task-intenrship
A test task for an internship in a company.
I do not provide a company name here due to a risk of cribbing my solution by other candidates.
The task was to detect timeframes on different episodes in various movie series, both test and train datasets are available.
the files in this repository are: a JSON file with predicted timeframes and a Jupyter Notebook file with completed task. It might not be seen correctly in GitHub so you will probably need to run the file locally.
So here goes the desctiption of my solution:

**Approach**:
1. **Feature Extraction**: Use a pretrained ResNet18 (without final FC) to extract deep visual features from video frames, sampled at 1 fps.
2. **Template Building**: From the labeled train videos, slice features within the provided start/end segments, resize each segment’s feature sequence to a common length (median of all lengths), and average to form a canonical intro template.
3. **Detection**: Slide a window of the template’s length over features of each test video (truncated to the max train-end time), compute cosine similarity between window and template, and pick the time with highest similarity as the intro segment.
4. **Optimizations**:
   - Frame sampling at 1 fps instead of full frame rate.
   - Frame size reduced to 256×256 in preprocessing.
   - Leveraging GPU with batched model inference.
   - Progress logging with `tqdm` for visibility.

Output is saved as `predicted_labels.json` with `start`/`end` times in `0:00:SS` format.

