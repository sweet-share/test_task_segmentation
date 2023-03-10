This is a solution for the test task #2.

This algorithm is based solely on human parsing segments, pose estimation was not used. 

You can find code in **test_task.ipynb**, segmented pictures are stored in **dataset/masked_images/** folder.

## Stages of mask creation process:
1. Find pants/skirt segment.
2. Find 4 corners of pants segment (upper left, upper right, downwards left, downwards right - UL, UR, DL, DR).
3. Find upper corners of left and right boot (or downward corners of legs, if there are no boots).
4. Find left and right lines which cross UL, UR and DL, DR respectively. 
5. Draw a visible part of that line from DL/DR to the height of boot/leg corners, remember that point.
6. Create a filled polygon with following points: DL, left line end, left boot corner, right boot corner, right line end, DR. 
7. Combine polygon with segmentation masks of pants, legs, boots and socks. Subtract segments of arms, bags, rackets etc. Create mask.
8. Apply mask to original picture.

## Performance 
Algorithm creates masks for 704 pictures for 45-50 seconds. Most of the masks are good, but there is a corner case that I couldn't manage to solve: 
if original human parsing segmentation has errors, the output mask will be wrong (namely pictures 176012_00, 176951_00, 176739_00, 176228_00). Other corner cases are treated with 
heuristics and crunches, so the algorithm works well in most of the cases.
