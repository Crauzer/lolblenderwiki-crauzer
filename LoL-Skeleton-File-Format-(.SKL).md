There are 3 known versions of the .SKL format. Versions 1, 2, and 0, in that order.

Version 1 and 2 are alike, while version 4 is far different. All versions start with a header containing an 8-byte string, then an integer with the version. It is at this point that the versions diverge. Version 4 description coming soon

# Format (Versions 1 and 2)
Field | Type | Size (bytes) | Condition | Description
------|------|--------------|-----------|-------------
header | [SKL v1-2 File Header](https://github.com/lispascal/lolblender/wiki/LoL-Skeleton-File-Format-(.SKL)#skl-file-header-versions-1-and-2)  | 20	  | | Holds numBones
bones | [SKL v1-2 Bone Data](https://github.com/lispascal/lolblender/wiki/LoL-Skeleton-File-Format-(.SKL)#single-bone-data-versions-1-and-2) * \[numBones\]  | 88 * numBones | | 
numBoneIDs | int | 4 | version == 2 | Holds number of bone IDs referred to by the Reordered Bone List
Reordering | int * \[numBoneIDs\] | 4 * numBoneIDs  | version == 2 | A reordered list of the bones (by ID) above. It seems in version 2, this was put in so that order of bones in file didn't have to correspond to ID #


## SKL File Header (Versions 1 and 2)
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
magic / fileType| char[8]  | 8	    |  id string? unknown
version | int | 4 | version. If not in [0,1,2], not supported yet (Submit an issue!)
skeletonHash | int | 4 | unique ID number?
numBones | int | 4 | number of bones
 |  | 20 | total

## Single Bone Data (Versions 1 and 2)
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
name | char[32] | 32	    | name of bone
parentID | int | 4 | ID # of parent bone. -1 means no parent
scale | float | 4 | Should be 1?
matrix | float[3][4] | 48 | affine bone matrix \[\[x1, x2, x3, xt\], \[y1, y2, y3, yt\], \[z1, z2, z3, zt\]\]. After reading in, z should be inverted (whole bottom row set to itself * -1)
           |     | 88                   | total

## SKL File Header (Version 0)
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
fileType | char[8]        |   8   |  id string
version  | int            |   4   |  version # (0)
zero     |      short     |   2   |  ?
numBones |      short     |   2   |   
numBoneIDs       | int    |   4   |
offsetVertexData | short  |   2   |   usually 64. Starting position in file of Bone Data structures
unknown          | short  |   2   |   if this is always 0, perhaps this and above are one int
offset1  | int            |   4   |   Start of bone ID map. Not sure what it's used for? Comments left by previous coders point to being used for version 4 animation
offsetToAnimationIndices | int | 4 |  Starting position in file of bone ID remapping. Name is probably quite inaccurate
offset2  | int            |   4   |   ?
offset3  | int            |   4   |   ?
offsetToStrings  | int    |   4   |   Starting position in file of bone name strings.
empty            |        |  20   |
                 |        |  64   | total

## Bone ID remapping & bone name strings (Version 0)
TODO: Explain further in this section about the above

## Single Bone Data (Version 0)
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
zero | short | 2 | 
id   | short | 2 | 
parent   | short | 2 | 
unknown   | short | 2 |  Might be combined with parent above, as an int
"namehash" | int | 4 | Might be chars or hash of the name
2.1        | float | 4 | the value 2.1 as a float
position | float[3] | 12 | xyz position. Z is opposite(negative) of actual value
scaling? | float[3] | 12 | values of very close to 1. probably x-y-z scaling
orientation | float[4] | 16 | Quaternion as [x,y,-z,-w], in that order
"ct"        | float[3] | 12 | Probably a translation vector or something to indicate the difference between the position calculated by position vectors, and the base "pose" used for animations. z is probably opposite of actual value.
extra       | float[8] | 32 | Unknown. These values are probably floats.
