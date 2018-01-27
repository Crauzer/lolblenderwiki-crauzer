There are 3 known versions of the .SKN format. Versions 1, 2, and 4

Version 1 is the simplest. It has a header, a materials block, a meta-data block, an Indices data block, and a Vertices data block. Version 2 and 4 are similar, but have some extra data, the purpose of which is mostly unknown.

# Format (Versions 1, 2, and 4)
Field | Type | Size (bytes) | Condition | Description
------|------|--------------|-----------|-------------
header | [SKN File Header](https://github.com/lispascal/lolblender/wiki/LoL-Mesh-File-Format-(.SKN)#skn-file-header)  | 8	  | | contains matsExist
numMats| int	 | 4  | matsExist > 0 | number of Material Header blocks
MaterialHeaders * \[numMats\] | [Material Header Data](https://github.com/lispascal/lolblender/wiki/LoL-Mesh-File-Format-(.SKN)#material-headers) | 80*numMaterials | matsExist > 0 |
metaData | [Meta-Data Block](https://github.com/lispascal/lolblender/wiki/LoL-Mesh-File-Format-(.SKN)#meta-data-block) | 12-60 |  | Contains numIndices & numVertices
indices | [Index Data](https://github.com/lispascal/lolblender/wiki/LoL-Mesh-File-Format-(.SKN)#single-index-data) * \[numIndices\]  | 4 * numIndices	  | |
vertices | [Vertex Data](https://github.com/lispascal/lolblender/wiki/LoL-Mesh-File-Format-(.SKN)#single-vertex-data) * \[numVertices\]  | 52 * numVertices | | 
endTab | int[3] | 12 | version >= 2 | Unknown data
       |        | variable  |       | total   


## SKN File Header
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
magic | int  | 4	    | 
version | short | 2			| 
matsExist | short | 2 		| 1 if material headers exist, 0 otherwise
 |  | 8 		| total

## Material Headers
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
name | char[64] | 64	    | name of the material
startVertex | int | 4			| Start vertex of material. With 1 mat, should be 0
numVertices | int | 4			| Number of vertices for this material.
startIndex | int | 4			| Start index of material. With 1 mat, should be 0
numIndices | int | 4			| Number of indices for this material. Is equal to 3*number of triangles (faces)
           |     | 80                   | total


## Meta-Data block
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
unknown(v4) | int | 4  | Version 4 only. Should be zero.
numIndices  | int | 4  | number of Indices in the file
numVertices | int | 4  | number of vertices in the file
Vertex Size | uint   | 4 | (v4) Size of the Vertex structure
Contains Tangent | bool | 4 | (v4) Whether the Vertex structure contains a Tangent
Bounding Box | float[3], float[3] | 24 | (v4) Bounding Box of the whole model. Structure is `Min` `Max`
Bounding Sphere | float[3], float | 16 | (v4) Bounding Sphere of the whole model. Structure is `Position`, `Radius`
            |     | 12, 60(v4) | total

## Single Index Data
Sets of three of these give the data for one face, or triangle.

Field | Type | Size (bytes) | Description
------|------|--------------|-------------
buffer 		| short | 2  | unused
Index  		| short | 2  | 
                |       | 4  | total


## Single Vertex Data
Field | Type | Size (bytes) | Description
------|------|--------------|-------------
position                | float[3] | 12  | Position of vertex (x, y, z)
bone Indices  		| byte[4]  |  4  | Bones that affect this vertex
bone weights 		| float[4] | 16  | Amount that the bones above affect this vertex
normal vector 		| float[3] | 12  | Normal vector of this vertex
texture coordinates     | float[2] |  8  | This vertex's UV texture coordinates
Tangent | byte[4] | 4 | The Tangent of this Vertex
                        |          | 52, 56(v4 and Contains Tangent)  | total

