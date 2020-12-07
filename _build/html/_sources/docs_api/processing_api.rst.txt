Mapflow API
===========


Authorization
-------------

The API uses ``Basic Auth`` authorization method. Valid login and password are to be used for every API call. The client should provide `Authorization` header.

API Methods
-----------

Projects
--------

Get project
"""""""""""

``GET https://mapflow.ai/api/{projectId}`` 

Returns the project with the specified ID.  

Response example:

.. code:: json
    
    {
        "id": "546d148f-19a1-40d8-8f16-d1e6dabfd204",
        "name": "test",
        "description": "test",
        "progress": {
            "status": "UNPROCESSED",
            "percentCompleted": 0,
            "details": []
        },
        "aoiCount": 0,
        "aoiArea": 0,
        "user": {
            "id": "61cd6899-19e8-44a0-97db-b86f1a9b7af4",
            "login": "user@user.com",
            "email": "user@user.com",
            "role": "USER",
            "created": "2019-12-16T16:10:29.492358Z"
        },
        "isDefault": false,
        "created": "2020-05-13T13:00:31.978Z",
        "updated": "2020-05-13T13:00:31.978Z",
        "workflowDefs": [
            {
                "id": "084474b5-e001-456f-a486-f62f5ee1ffe1",
                "name": "Buildings Detection",
                "created": "2020-08-11T19:57:40.974170Z",
                "updated": "2020-08-11T19:57:40.974172Z"
            }
        ]
    }


Get default (demo) project
""""""""""""""""""""""""""

Default (demo) project is created for each authorized user after the first login.

``GET https://mapflow.ai/api/projects/default`` 

Returns the name and ID of the user's default project.  

Get all projects
""""""""""""""""

``GET https://platform.geoalert.io/rest/projects`` 

Returns the list of all user's projects.  


Post project
""""""""""""

``POST https://mapflow.ai/api/projects/``

Creates a new project, and returns its immediate state.  

Request body example:

.. code:: json

    {
        "name": "test",          
        "description": "test"
    }



//Name of the project
//Arbitrary description of this project. Optional

Response: the newly created project

Delete project
""""""""""""""

``DELETE https://mapflow.ai/api/projects/{projectId}`` 

Deletes the project. Cascade deletes any child entities.

Processings
-----------

Get processing
""""""""""""""

``GET https://mapflow.ai/api/processings/{processingId}``

Returns the processing with the specified id.  
Response example:

.. code:: json
    {
        "id": "b86127bb-38bc-43e7-9fa9-54b37a0e17af",
        "name": "Buildings Detection4",
        "projectId": "b041da8c-3af3-4269-b4b2-6e3cfe26520c",
        "vectorLayer": {
            "id": "098ff0e4-ac3e-45f9-a049-cf84ac45e5c1",
            "name": "Buildings Detection4",
            "tileJsonUrl": "http://localhost:8600/api/layers/7448c462-6078-49d6-b64a-289c4320508c.json",
            "tileUrl": "http://localhost:8600/api/layers/7448c462-6078-49d6-b64a-289c4320508c/tiles/{z}/{x}/{y}.vector.pbf"
        },
        "rasterLayer": {
            "id": "f56ba4c8-30cb-4a54-9aca-cb66214ea2f8",
            "tileJsonUrl": "http://localhost:8500/api/v0/cogs/tiles.json?uri=s3://mapflow-rasters/4f64797d-bfb2-4433-bf56-3bcfd790ee20",
            "tileUrl": "http://localhost:8500/api/v0/cogs/tiles/{z}/{x}/{y}.png?uri=s3://mapflow-rasters/4f64797d-bfb2-4433-bf56-3bcfd790ee20"
        },
        "workflowDef": {
            "id": "9b70a8fc-6e63-4929-b287-c2307d06e678",
            "name": "Buildings Detection",
            "created": "2020-05-06T23:08:50.412Z",
            "updated": "2020-05-06T23:08:50.412Z"
        },
        "externalWfIds": [
            146923
        ],
        "aoiCount": 1,
        "aoiArea": 265197,
        "status": "OK",
        "percentCompleted": 100,
        "params": {
            "source_type": "tif",
            "url": "s3://mapflow-rasters/7689666a-a707-4307-8c76-bf8c2ee3e0e4/raster.tif",
            "zoom": "18"
        },
        "meta": {
            "test": "test"
        },
        "created": "2020-05-06T23:13:57.239Z",
        "updated": "2020-05-06T23:13:57.239Z"
    }


Get all processings
"""""""""""""""""""

``GET https://mapflow.ai/api/processings``

Returns the list of this user's processings.  

Post processing
"""""""""""""""

``POST https://platform.geoalert.io/rest/processings``

Creates and runs a processing, and returns its immediate state  
Request body example:

.. code:: json

    {
        "name": "Test",                                      #Name of this processing. Optional.
        "description": "A simple test",                      #Arbitrary description of this processing. Optional.
        "projectId": "20f05e39-ccea-4e26-a7f3-55b620bf4e31", #Project id. Optional. If not set, this user's default project will be used.
        "wdName": "Buildings Detection",                     #The name of a workflow definition.
                                                             #Could be "Buildings Detection", or "Forest Detection", etc. See ref. below
        "wdId": "009a89fc-bdf9-408b-ad04-e33bb1cdedda",      #Workflow definition id. Either wdName or wdId may be specified.
        "geometry": {                                        #A geojson geometry of the area of interest.
            "type": "Polygon",
            "coordinates": [
              [
                [
                  37.29836940765381,
                  55.63619642594767
                ],
                [
                  37.307724952697754,
                  55.63619642594767
                ],
                [
                  37.307724952697754,
                  55.64024152130109
                ],
                [
                  37.29836940765381,
                  55.64024152130109
                ],
                [
                  37.29836940765381,
                  55.63619642594767
                ]
              ]
            ]
        },
        "params": {                           #Arbitrary string parameters of this processing. Optional.
            "source_type": "wms",
            "url": "https://catalog.data.gov/dataset/usgs-naip-imagery-overlay-map-service-from-the-national-map/resource/776e4050-213c-4203-91b8-657d8fa4b009",
            "partition_size": "0.1"           #Max partition size in degrees (both dimensions). Defaults to DEFAULT_PARTITION_SIZE=0.1.
        },
        "meta": {                             #Arbitrary string key-value pairs for this processing (metadata). Optional.
            "test": "test"
        }
    }


To process a user-provided raster (see `Upload GeoTIFF for processing` section), set parameters as follows:  

 .. code:: json

        "params": {
            "source_type": "tif",
            "url": "s3://mapflow-rasters/9764750d-6047-407e-a972-5ebd6844be8a/raster.tif"
        }

Response: the newly created processing

Restart processing
^^^^^^^^^^^^^^^^^^

``POST https://platform.geoalert.io/rest/processings/{processingId}/restart``  

Restarts failed partitions of this processing. Doesn't restart non-failed partitions. Each workflow is restarted from the first failed stage. Thus, the least possible amount of work is performed to try and bring the processing into successful state.

Delete processing
^^^^^^^^^^^^^^^^^

``DELETE https://platform.geoalert.io/rest/processings/{processingId}``

Deletes this processing. Cascade deletes any child entities.

Get processing AOIs
^^^^^^^^^^^^^^^^^^^

``GET https://platform.geoalert.io/rest/processings/{processingId}/aois``  

Returns a list of the defined geographical areas for processing in GeoJSON.  

Response example:


.. code:: json

    [
        {
            "id": "b86127bb-38bc-43e7-9fa9-54b37a0e17af",
            "status": "IN_PROGRESS",
            "percentCompleted": 0,
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                    [
                        [
                            37.29836940765381,
                            55.63619642594767
                        ],
                        [
                            37.29836940765381,
                            55.64024152130109
                        ],
                        [
                            37.307724952697754,
                            55.64024152130109
                        ],
                        [
                            37.307724952697754,
                            55.63619642594767
                        ],
                        [
                            37.29836940765381,
                            55.63619642594767
                        ]
                    ]
                ]
            },
            "area": 265197,
            "externalWfIds": [
                "146923"
            ]
        }
    ]


Downloading processing results
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``GET https://platform.geoalert.io/rest/processings/{processingId}/result``

Returns geojson results of this processing as an octet stream. Should only be called on a successfully completed processing.


Upload GeoTIFF for processing
-----------------------------

``POST https://platform.geoalert.io/rest/rasters``

Can be used to upload a raster for further processing. Returns URI to the uploaded raster. This URI can be referenced when starting a processing.  
The request is a multipart request whith the only part "file" - which contains the raster.
Request example with ``cURL``:  

    .. code:: bash

          curl -X POST \
          https://mapflow.ai/api/rasters \
          -H 'authorization: <Insert auth header value>' \
          -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
          -F file=@custom_raster.tif



Response example:  

``{"uri": "s3://mapflow-rasters/9764750d-6047-407e-a972-5ebd6844be8a/raster.tif"}``


API reference
-------------

wdName
""""""

   .. tabularcolumns:: |p{5cm}|p{7cm}|p{7cm}|

   .. csv-table::
      :file: _static/api_ref_wdname.csv 
      :header-rows: 1 
      :class: longtable
      :widths: 1 1 1


source_type
"""""""""""

   .. tabularcolumns:: |p{5cm}|p{7cm}|p{7cm}|

   .. csv-table::
      :file: _static/api_ref_source.csv 
      :header-rows: 1 
      :class: longtable
      :widths: 1 1 1



status
""""""

   .. tabularcolumns:: |p{5cm}|p{7cm}|p{7cm}|

   .. csv-table::
      :file: _static/api_ref_status.csv 
      :header-rows: 1 
      :class: longtable
      :widths: 1 1 1