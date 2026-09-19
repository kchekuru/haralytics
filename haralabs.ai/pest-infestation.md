                    Most relevant examples

YOLOv8-SAHI-Inference-Video: the best starting point for drone imagery. SAHI slices high-resolution frames into overlapping tiles, detects small objects, and merges results. Adapt it from video frames to georeferenced orthomosaic tiles or individual drone images.
YOLOv8-Segmentation-ONNXRuntime-Python: useful if infestation appears as irregular canopy damage, discolored patches, leaf loss, or affected plant regions rather than discrete insects.
YOLOv8-Region-Counter: useful after detection for summarizing infestation counts by field blocks, but it is not the diagnostic model.
object_tracking.ipynb: useful for repeated video passes or tracking insects/markers across frames. It is less important for a stitched aerial survey.
heatmaps.ipynb: useful for visualizing spatial concentration, but a heatmap alone should not be treated as infestation detection.
tutorial.ipynb: the closest general training starting point.
The repository’s split_dota.py logic and SAHI tiled inference guide are relevant for creating high-resolution training tiles.

                    Recommended model design

“Pest infestation” is usually too broad as one label. First decide what the imagery can actually show:

Visible insects or nests: object detection.
Damaged leaves, canopy gaps, lesions, or affected plants: instance or semantic segmentation.
A plant or tile rated as healthy, stressed, or infested: classification.
A practical system may use detection or segmentation for evidence, followed by field-level severity estimation.

For large(100+) acres, I would use this data pipeline:

Capture RGB imagery with consistent altitude, overlap, time of day, and ground sampling distance.
Create an orthomosaic or preserve image GPS/EXIF metadata.
Tile the imagery with overlap so small pests are not lost during resizing.
Label individual pests, affected plants, or damage masks, depending on the visible evidence.
Train a custom YOLO model from pretrained weights.
Run tiled inference and merge detections.
Convert detections to map coordinates and aggregate them into field blocks or management zones.
Produce severity maps and have an agronomist review a sample of alerts.

For large-area processing, avoid randomly splitting adjacent tiles. Tiles from the same flight or neighboring locations are highly correlated and can make validation look much better than real deployment. Use spatial and temporal holdouts, for example:

Training: selected blocks and historical flights.
Evaluation: different blocks from the same farm, unseen blocks, a later flight, and some neighboring acreage.
Including neighboring farm acreage is useful for reducing overfitting to one farm, camera setup, soil color, or crop variety. Keep some neighboring fields completely held out for evaluation rather than mixing all of them into training.

                    Hara Labs Frontier Fine Tunning Services

No
Custom Model Implementation Services
Model: YOLO segmentation if damage regions are visible; otherwise detection.
Inference: SAHI-style tiled inference at the native resolution.
Aggregation: region/field-block summaries, not raw frame counts.
Training: standard transfer learning first, with spatial-temporal holdouts.
Proprietary Data: include historic, current, and neighboring farm imagery, but reserve unseen acreage and a later flight for the final signoff. Farmers painlessly (hands free) walking capturing farm imagery with mobile devices. multispectral and thermal images using mobile, propreitary drone
Operational output: infestation probability plus severity and map location, with human review for high-impact decisions.
