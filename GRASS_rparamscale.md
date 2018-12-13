# Digital_Terrain_Analyses
## Codes relevant to geomorphometric calculations in ArcGIS, GRASS GIS, and SAGA GIS.
#### This script is generalized for the r.param.scale function in GRASS GIS 7.2.2
#### It can only be used in the GRASS python interface
#### It loops through analyses scales based on the list comprehension input; all other inputs are generic
##### requires 1) DEM from Grass Environment name; 2) List of analyses scales; 3) script must be saved in the output workspace
```
## import modules
import os

## list comprehension for analysis scales
list1 = [j for j in range(3,42,1) if j %2 != 0]
list2 = [k for k in range(47,138,6) if k %2 != 0]
list3 = [l for l in range (43,204,10) if l %2 != 0]

## this script only works in the interactive python window for GRASS GIS 7.2.2
import grass.script as gscript

## DEFINE GEOSPATIAL ENVIRONMENT
def main():

    gscript.run_command('g.region', raster=InputDEM)


## Input method (elev, slope, aspect, profc, planc, longc, crosc, maxic, minic, feature: peaks, ridges, passes, channels, pits, and planes
method_n = ["aspect"]
## Name the variable (example is asp)
naming = ["asp_"]

## workspace output
path = os.getcwd()

### loop through lists
for i in list1:

    oname= "asp_" + str(i*3).zfill(3)+"m"

    gscript.run_command("r.param.scale", overwrite="T", input = 'DEM@PERMANENT', output = oname, size = i, method = method_n)

    outname = oname+".tif"

    AddLayer(oname)

    gscript.run_command("r.out.gdal", overwrite="T", input= oname, format = "GTiff", output=path + outname)

for j in list2:

    oname= "asp_" + str(j*3).zfill(3)+"m"

    gscript.run_command("r.param.scale", overwrite="T", input = 'DEM@PERMANENT', output = oname, size = j, method = method_n)

    outname = oname+".tif"

    AddLayer(oname)

    gscript.run_command("r.out.gdal", overwrite="T", input= oname, format = "GTiff", output = path + outname)

for k in list3:

    oname= "asp_" + str(k*10).zfill(3)+"m"

    gscript.run_command("r.param.scale", overwrite="T", input = 'DEM@PERMANENT', output = oname, size = k, method = method_n)

    outname = oname+".tif"

    AddLayer(oname)

    gscript.run_command("r.out.gdal", overwrite="T", input= oname, format = "GTiff", output = path + outname)
