# Digital_Terrain_Analyses
## Codes relevant to geomorphometric calculations in ArcGIS, GRASS GIS, and SAGA GIS.
### This script calculates relative elevation looping through a list of analyses scales for 2 different DEMs using arcpy spatial analyst
#### requires 1) script saved to workspace; 2) coarse-scale and fine-scale DEMs; 3) Output path
```
list1 = [j for j in range(3,42,1) if j %2 != 0]
list2 = [k for k in range(47,138,6) if k %2 != 0]
list3 = [l for l in range (43,204,10) if l %2 != 0]

## import modules
import os
from arcpy import env
from arcpy.sa import *
arcpy.CheckOutExtension("Spatial")

# Set environment settings
path = os.getcwd()
env.workspace = path

# Script arguments
#Inras = finescale DEM.tif'
#Inras2 = coarsescale DEM.tif'
#source = 'outputpath'

for i in list1:
    outMax = FocalStatistics(Inras, NbrCircle(i, "MAP"), "MAXIMUM", "DATA")
    outMin = FocalStatistics(Inras, NbrCircle(i, "MAP"), "MINIMUM", "DATA")
    OutRel = Inras - ((outMax + outMin) - Inras)
    oname= "rel_" + str(i*3).zfill(3)+"m"
    outname = oname +".tif"
    OutRel.save(source + '\\' + outname)

for j in list2:
    outMax = FocalStatistics(Inras, NbrCircle(j, "MAP"), "MAXIMUM", "DATA")
    outMin = FocalStatistics(Inras, NbrCircle(j, "MAP"), "MINIMUM", "DATA")
    OutRel = Inras - ((outMax + outMin) - Inras)
    oname= "rel_" + str(j*3).zfill(3)+"m"
    outname = oname +".tif"
    OutRel.save(source + '\\' + outname)

for k in list3:
    outMax = FocalStatistics(Inras2, NbrCircle(k, "MAP"), "MAXIMUM", "DATA")
    outMin = FocalStatistics(Inras2, NbrCircle(k, "MAP"), "MINIMUM", "DATA")
    OutRel = Inras - ((outMax + outMin) - Inras)
    oname= "rel_" + str(k*10).zfill(3)+"m"
    outname = oname +".tif"
    OutRel.save(source + '\\' + outname)    

