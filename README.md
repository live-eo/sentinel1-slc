# Sentinel1- SLC Europe dataset in unzipped form

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Sentinel1-SLC Product description](#sentinel1-slc-product-description)
- [Dataset in S3 at glance](#dataset-in-s3-at-glance)
  - [Expanding data scope to EU region (work in progress)](#expanding-data-scope-to-eu-region-work-in-progress)
  - [AWS S3 bucket and object structure](#aws-s3-bucket-and-object-structure)
  - [Contents inside each imagery](#contents-inside-each-imagery)
  - [Sentinel1-SLC IW product naming convention](#sentinel1-slc-iw-product-naming-convention)
- [Querying for available imagery in the bucket](#querying-for-available-imagery-in-the-bucket)
  - [Some examples with AWS CLI.](#some-examples-with-aws-cli)
  - [Examples with boto3 library](#examples-with-boto3-library)
- [Acknowledgements](#acknowledgements)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Sentinel1-SLC Product description
Synthetic Aperture Radar (SAR) is an active remote sensing technique, where electromagnetic pulses are emitted and received by an antenna. 
The Sentinel-1 satellite mission consists of two identical satellites, Sentinel-1A and Sentinel-1B, launched by the European Space Agency (ESA) in 03 April 2014 and 25 April 2016 respectively (ESA, 2021a). These satellites orbit the Earth while acquiring SAR images at a wave frequency of 5.405GHz (C-band). This mission exploits the usage of 2 satellites in order to have a fast revisit time (12 days and down to 6 days in some areas).The main applications of the Sentinel-1 SAR images are the monitoring of land use changes and surface deformation along with support for emergency management. Further applications include but are not limited to monitoring of sea ice, icebergs, land ice, inland waters, oil spills, ships, and others (ESA, 2021a).

ESA. (2021a). About [copernicus sentinel-1](https://sentinel.esa.int/documents/247904/4603794/Sentinel-1-infographic.pdf) 
## Dataset in S3 at glance

The Sentinel-1 SLC IW image collection provided by ESA comes in an archive format, which must be entirely downloaded and unzipped first, in order to be used. Often times users only need selective data inside these archived folders for their work.
- The Earth On AWS dataset is stored in the unzipped form in the S3 bucket, offering users the option to selectively retrieve either the full imagery or only the parts of the data that is needed for a given study area.
- Since the dataset resides on S3, depending upon the application, users can also directly read the object into memory and carry out their work without having to download, unzip and store them in on-premise or cloud storages.
- The S3 bucket and objects in it are public. Anonymous access is also enabled. So users can access the data without aws account/credentials as well.

We have fully ingested [Sentinel-1A/B Level-1 SLC](https://sentinel.esa.int/web/sentinel/user-guides/sentinel-1-sar/product-types-processing-levels/level-1) over [Germany](https://github.com/live-eo/earth-on-aws/blob/main/imagery/germany.geojson), which is updated in the interval of 6 days, after they are made available by [Alaska Satellite Facility](https://search.asf.alaska.edu/#/) (ASF).


<p align="center">
<img src="https://github.com/live-eo/earth-on-aws/blob/main/imagery/germany.png">
</p>

### Expanding data scope to EU region (work in progress)

Following up, as a next step, we aim to expand the service by increasing data coverage across the [Europe region](https://github.com/live-eo/earth-on-aws/blob/main/imagery/europe.geojson). The map below clearly describes the data coverage expansion plan.

<p align="center">
<img src="https://github.com/live-eo/earth-on-aws/blob/main/imagery/europe.png">
</p>

The following figure provides an insight of the current percentage of EU dataset uploaded to the S3 bucket. As mentioned above, in addition to [Germany](https://github.com/live-eo/earth-on-aws/blob/main/imagery/germany.geojson), other regions are also being updated simultaneously, however, the process is still in progress with 34.31% of the total EU dataset already available in [sentinel1-slc](https://s3.console.aws.amazon.com/s3/buckets/sentinel1-slc/?region=eu-west-1&tab=objects) S3 bucket.


<table align = "center">
<tr><th>Year</th><th>Percentage of EU dataset in S3</th></tr>
<tr><td>2014</td><td align = "center">94.55%</td></tr>
<tr><td>2015</td><td align = "center">66.48%</td></tr>
<tr><td>2016</td><td align = "center">24.08%</td></tr>
<tr><td>2017</td><td align = "center">28.33%</td></tr>
<tr><td>2018</td><td align = "center">29.53%</td></tr>
<tr><td>2019</td><td align = "center">68.47%</td></tr>
<tr><td>2020</td><td align = "center">19.80%</td></tr>
<tr><td>2021</td><td align = "center">17.30%</td></tr>
<tr><td>2022</td><td align = "center">43.21%</td></tr>
<tr><td><b>Total</b></td><td align = "center"><b>34.31%</b></td></tr>

</table>

### AWS S3 bucket and object structure

The dataset in the S3 bucket is organized in a directory structure based on the **start date of the acquisition** for ease of retrieval.
The table below provides information on product details, useful for querying the dataset.

---

s3://sentinel1-slc/**`YYYY`/`MM`/`DD`/`XXX`\_`BB`\_SLC\_\_1S`PP`\_`YYYYMMDD`T`HHMMSS`\_`yyyymmdd`T`hhmmss`\_`OOOOOO`\_`DDDDDD`\_`CCCC`**.SAFE

---

Example S3 URI of an imagery: 

```
s3://sentinel1-slc/2022/01/01/S1A_IW_SLC__1SDV_20220101T053305_20220101T053332_041263_04E777_0EE8.SAFE/
``` 

### Contents inside each imagery

Each Sentinel-1 SAR product folder (object in the bucket) includes:
  - a 'manifest.safe' file which holds the general product information in XML
  - a measurement folder with complex measurement data set in GeoTIFF format per sub-swath per polarisation
  - a preview folder containing 'quicklooks' in PNG format, Google Earth overlays in KML format and HTML preview files
  - an annotation folder containing the product metadata in XML as well as calibration data
  - a support folder containing the XML schemes describing the product XML.




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





## Querying for available imagery in the bucket

The data in the S3 bucket can be queried with AWS CLI or with boto3 library.

### Some examples with AWS CLI. 

For access without using the aws credentials, simply pass the `--no-sign-request` to the same command. 

- To retrieve list of all years for which the imagery are available:
  ```cmd
  aws s3 ls sentinel1-slc
  aws s3 ls sentinel-slc --no-sign-request
  ```
- To retrieve list of all imagery available for a given year, month and date:
  ```cmd
  aws s3 ls sentinel1-slc/2022/01/01/
  aws s3 ls sentinel1-slc/2022/01/01/ --no-sign-request
  ```
- To retrieve list of all files and folders inside a given imagery:
  ```cmd
  aws s3 ls sentinel1-slc/2022/01/01/S1A_IW_SLC__1SDV_20220101T053305_20220101T053332_041263_04E777_0EE8.SAFE/
  aws s3 ls sentinel1-slc/2022/01/01/S1A_IW_SLC__1SDV_20220101T053305_20220101T053332_041263_04E777_0EE8.SAFE/ --no-sign-request
  ```
- To retrieve list of all imagery with a given polarization type and mission type:
  ```cmd
  aws s3 ls sentinel1-slc/2022/01/01/S1A_IW_SLC__1SDV_
  aws s3 ls sentinel1-slc/2022/01/01/S1A_IW_SLC__1SDV_ --no-sign-request
  ```
   
### Examples with boto3 library
- Anonymously read objects without downloading and without passing AWS credentials.

  ```python
  
  import boto3
  
  from botocore import UNSIGNED
  from botocore.config import Config
  
  s3_client = boto3.client('s3', config=Config(signature_version=UNSIGNED))
  my_bucket = 'sentinel1-slc'
  file_to_read = '2022/01/01/S1A_IW_SLC__1SDV_20220101T053305_20220101T053332_041263_04E777_0EE8.SAFE/annotation/s1a-iw1-slc-vh-20220101t053305-20220101t053330-041263-04e777-001.xml'
  
  s3_response_object = s3_client.get_object(Bucket=my_bucket, Key=file_to_read)
  object_content = s3_response_object['Body'].read()
  print(object_content)
  
  ```

- Retrieve list of all imagery for a given year and month with AWS credentials.
  ```python
  import boto3
  
  client = boto3.client('s3', region_name='eu-west-1')
  my_bucket = 'sentinel1-slc'
  prefix_to_query = '2022/01/01/'
  results = client.list_objects(Bucket=my_bucket,
                                Prefix=prefix_to_query,
                                Delimiter='/'
                                )
  for result in results.get('CommonPrefixes'):
      print(result.get('Prefix'))
  ```

Tools and ready to use scripts will be added to the repository in future updates.

## Acknowledgements

We would like to thank Amazon Web Services for providing the storage resources for this program. We would also like to acknowledge the team at ASF for their collaboration. 


