# README
This is a package to create DSSAT input files

## ```Way2DSSAT.SOL()``` : Creates DSSAT Soil File (SG.SOL)
 Use `Way2DSSAT.SOL("path/to/you/gssurgo.csv")` to create a soil file first. The funciton will save a SG.SOL file. It also returns a dictionary of soil depth:SDUL which will be used as intial conditions in the Xfile. a string of MUKEYS of all the soil profiles created. 
 If you have multiple soil profiles in the csv exported from GSSURGO data, different soil profiles will be created with name convention of soil ID as "SGG"+"MUKEY".eg. GSS2733103.
 
Since the funciton is designed to handle the csv generated from gSSURGO soil database. It is aware of the column names which the gSSURGO arcmap toolbox generates. Thus no need to remove any columns containing 'pct' string in the column name. 

**Columns required**:
 
        * WC15Bar_DCP_0to5
        * WC3rdbar_DCP_0to5
        * Clay_DCP_0to5
        * Silt_DCP_0to5
        * pHwater_DCP_0to5
        * CEC_DCP_0to5
        * Db3rdbar_DCP_0to5
        * Ksat_DCP_0to5
        * OrgMatter_DCP_0to5
        * MUKEY_DCP_0to5 
 Similar columns for other depths as well
 
**Important** Save the returned dicitonary to a variable
For eg.
returned dictionary is: 

{'GSS2733103': {5: 0.2, 15: 0.2, 30: 0.2, 60: 0.2, 100: 0.338, 200: 0.308},
 'GSS2733104': {5: 0.2, 15: 0.2, 30: 0.2, 60: 0.2, 100: 0.338, 200: 0.308}}
 
- 'GSS2733103' is soil ID (individual soil profile)
- {5: 0.2, 15: 0.2, 30: 0.2, 60: 0.2, 100: 0.338, 200: 0.308} is a dictinary of key=depth, value=SDUL.

Incase of multiple profiles (as here), there is a need to further save the Individual sub-dictionaries(nested dictionary) to different variables. Individual sub-dictionary has to be provided as the Input argument Inside the `Way2DSSAT.Xfile()` funciton.

### Example

```
path = 'Username/pathtothecsv'.
A,B = Way2DSSAT.SOL(path)
```
A is the returned dictionary
B is the list of (MUKEYS) Soil profiles
***

## ```Way2DSSAT.WTH()``` : Creates .WTH file

This function creates a weather file of DSSAT format .WTH by taking input as paath to the .csv file contaitng weather data. The funciton is writes single year data thus csv should contain data from once year. Although it will create .WTH file even if the data is not from a single year but the name of the outputfile will be saved using the year of first row of the .csv.

The funciton also returns the name of the file created. 

**Following columns are required**
        - 'SRAD','Tmax','Tmin','Rain','timedate' 
        - 'timedate' in format = '%m/%d/%Y'


### Example
```
path = "path/to/your/weatherdata.csv"
Way2DSSAT.WTH(path,
        stname = 'ABCd',
        latitude= 34.583,
        longitude= -103.200,
        elevation= 1348)
```        

***

## ```Way2DSSAT.Xfile()``` : Creates .SNX file

It creates DSSAT seasonal file (.SNX) from a reference template. Click [here](https://github.com/ManavjotSingh97/Way2DSSAT/blob/main/Seasonl/KSMR6301.SNX) to download 

The [dictionary](#```Way2DSSAT.SOL()```-:-Creates-DSSAT-Soil-File-(SG.SOL)) created above using ```.SOL``` funciton is used as an argument along with other arguments as shown here

```

A,B = Way2DSSAT("path/to/csvfromssurgo.csv")
print(A)

#this will return
```

{'GSS2733103': {5: 0.2, 15: 0.2, 30: 0.2, 60: 0.2, 100: 0.338, 200: 0.308},
 'GSS2733104': {5: 0.2, 15: 0.2, 30: 0.2, 60: 0.2, 100: 0.338, 200: 0.308}}

We need **{5: 0.2, 15: 0.2, 30: 0.2, 60: 0.2, 100: 0.338, 200: 0.308}**

```
path = 'path/to/template.SNX'
init_cond = A['GSS2733103'] #Relace with the soil id you get from print(A)

Xfile(path,init_cond,site_name = '-99',station = 'ABCD',soil = 'GSS2733103',
          crop = 'SB',cultivar = 'NE0006 NECPHA4 2014',planting_date = '24145')

#replace the default information 
```
> **All the output files are stored in CWD**

