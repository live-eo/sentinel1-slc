# earth-on-aws
Sentinel1-SLC IW imagery in an unzipped form 





## Accessing the unzipped dataset on AWS

Currently, we ingest [Sentinel-1A/B Level-1 Interferometric Wideswath(IW) SLC](https://sentinel.esa.int/web/sentinel/technical-guides/sentinel-1-sar/products-algorithms/level-1/single-look-complex/interferometric-wide-swath) over [Germany](https://github.com/live-eo/earth-on-aws/blob/main/imagery/germany.geojson) only:
<p align="center">
<img src="https://github.com/live-eo/earth-on-aws/blob/main/imagery/germany.png" width="650">
</p>


### AWS S3 bucket and object structure

The dataset in the S3 bucket is organized in a directory structure based on the **start date of the acquisition** for ease of retrieval.
The table below provides information on product details, useful for querying the dataset.

---

s3://sentinel1-slc/**`YYYY`/`MM`/`XXX`\_`BB`\_SLC\_\_1S`PP`\_`YYYYMMDD`T`HHMMSS`\_`yyyymmdd`T`hhmmss`\_`OOOOOO`\_`DDDDDD`\_`CCCC`**.SAFE

---

Example S3 URI of an imagery: 

```
s3://sentinel1-slc/2014/11/S1A_IW_SLC__1SDV_20141101T055027_20141101T055057_003084_003886_5D35.SAFE/
``` 

or if you only want to retrieve the PDF of that imagery

```
https://sentinel1-slc.s3.eu-west-1.amazonaws.com/2014/11/S1A_IW_SLC__1SDV_20141101T055027_20141101T055057_003084_003886_5D35.SAFE/S1A_IW_SLC__1SDV_20141101T055027_20141101T055057_003084_003886_5D35.SAFE-report-20141101T103541.pdf
```

### [Sentinel1-SLC IW product naming convention](https://sentinel.esa.int/web/sentinel/technical-guides/sentinel-1-sar/products-algorithms/level-1-product-formatting)

|Variable      |Description                |Details (code: code details)|
|--------------|---------------------------|----------------------------|
|**`XXX`**      |Denotes the satellite       |S1A: Sentinel-1A <br>S1B: Sentinel-1B|
|**`BB`**       |Acquisition Mode            |IW: Interferometric Wide-Swath       |
|**`PP`**       |Polarisation                |SH:single HH polarisation <br>SV:	single VV polarisation<br>DH:	dual HH+HV polarisation <br>DV:	dual VV+VH polarisation <br>HH:	Partial Dual polarisation, HH only <br>HV:	Partial Dual polarisation, HV only <br>VV:	Partial Dual polarisation, VV only <br>VH:	Partial Dual polarisation, VV only|
|**`YYYYMMDD`** |Acquisition Start Date (UTC)|Four-digit year, two-digit month, two-digit day|
|**`HHMMSS`**   |Acquisition Start Time (UTC)|Two-digit hour, two-digit minutes, two-digit seconds|
|**`yyyymmdd`** |Acquisition End Date (UTC)  |Four-digit year, two-digit month, two-digit day|
|**`hhmmss`**   |Acquisition End Time (UTC)  |Two-digit hour, two-digit minutes, two-digit seconds|
|**`OOOOOO`**   |Absolute orbit number at product start time |In the range of 000001-999999|
|**`DDDDDD`**   |Mission data take ID        |In the range 000001-FFFFFF|
|**`CCCC`**      |Hexadecimal string generated from CRC-16 of the manifest file |CRC-16 algorithm used to compute the unique identifier is CRC-CCITT (0xFFFF)|





### Querying for available imagery in the bucket

List of all available imagery in the bucket for a given year/month can be queried with awscli s3 command:
Example: Retrieve list of all available imagery for the month of October (10) and the year 2014:
```
aws s3 ls s3://sentinel1-slc/2014/10/
```

This dataset is updated in the interval of 6 days, after they are made available by [Alaska Satellite Facility](https://search.asf.alaska.edu/#/).


### Product Directory

Each Sentinel-1 SAR product folder (object in the bucket) includes:
  - a 'manifest.safe' file which holds the general product information in XML
  - a measurement folder with complex measurement data set in GeoTIFF format per sub-swath per polarisation
  - a preview folder containing 'quicklooks' in PNG format, Google Earth overlays in KML format and HTML preview files
  - an annotation folder containing the product metadata in XML as well as calibration data
  - a support folder containing the XML schemes describing the product XML.

 
 ### Downloading objects

The objects in the bucket can be accessed and retrieved using the s3api or boto3. 


## Acknowledgements

We would like to thank Amazon Web Services for providing the storage resources for this program. We would also like to acknowledge the team at ASF for their collaboration. 