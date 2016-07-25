This folder contains the calibration pattern used for [Kalibr](https://github.com/ethz-asl/kalibr) camera calibration toolbox. 

## Generation

The AprilTag is generated using `kalibr_create_target_pdf --type apriltag --nx 4 --ny 6 --tsize 0.15 --tspace 0.176`. We printed it on an A0 foam board. 

## Calculation

Suppose we want to find out the largest number of columns on the A0 paper, whose size is 841 x 1189 mm, and suppose max. column number = x, tag size = 15cm, tag spacing = 2.64cm, then 15x + 2.64(x+1) = 84cm, so floor(x) = 4, similarly, for row, 15y + 2.64(y+1) = 119cm, so floor(y) = 6. If we use larger numbers than 4, 6, then the size exceeds that of A0 paper. 

Hence we can set nx = 4, ny = 6, tsize = 0.15, and tspace = 2.64/15 = 0.176. 

## YAML file

To use the pattern, print it out on an A0 paper, measure the actual size of the tag size and spacing for the yaml. 

To use apriltag, the yaml file should be 

```
target_type: 'aprilgrid' #gridtype
tagCols: 6               #number of apriltags
tagRows: 6               #number of apriltags
tagSize: 0.088           #size of apriltag, edge to edge [m]
tagSpacing: 0.3          #ratio of space between tags to tagSize
                         #example: tagSize=2m, spacing=0.5m --> tagSpacing=0.25[-]
```

The `target_type` specifies that we are using aprilgrid. 

Similarly, to use checkerboard, the yaml file should be 

```
target_type: 'checkerboard' #gridtype
targetCols: 6               #number of internal chessboard corners
targetRows: 7               #number of internal chessboard corners
rowSpacingMeters: 0.06      #size of one chessboard square [m]
colSpacingMeters: 0.06      #size of one chessboard square [m]
```

Note that we need to modify the yaml file provided in the [example](https://github.com/ethz-asl/kalibr/wiki/downloads) accordingly. 




