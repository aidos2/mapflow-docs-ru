API обработки данных
====================

.. attention::
    Проекты и обработки, которые вы создаете в Mapflow, не будут доступны через API, и наоборот. Также ваши кредиты Mapflow нельзя использовать для выполнения обработки через API. Чтобы начать пользоваться API, используйте логин Mapflow и API token, который нужно сгенерировать в `настройках профиля <https://app.mapflow.ai/account>`_ (см. :doc:`авторизация для работы с Mapflow API <../docs_userguides/mapflow_auth>`). 

.. note::
    .. figure:: _static/postman_logo.png
       :alt: Preview results
       :align: left
       :width: 1cm
   Запустите у себя `Коллекцию запросов API для Postman <https://documenter.getpostman.com/view/5400715/TzmCiu5h>`_.

.. important::
  При загрузке собственных изображений для обработки через API платформы Mapflow необходимо следовать требованиям, указанным на странице с :doc:`описанием моделей <../docs_userguides/pipelines>`. Запрос с помощью по предобработке данных отправляйте на help@geoalert.io.

Авторизация
-----------

API использует метод авторизации Basic Auth, как получить логин и API token описано :doc:`здесь <../docs_userguides/mapflow_auth>`.

Методы API
-----------

Проекты
--------

Получить проект
"""""""""""""""

``GET https://api.mapflow.ai/rest/projects/{projectId}`` 

Возвращает проект с указанным ID.   

Пример ответа:

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


Поучить демо проект
"""""""""""""""""""

При регистрации для каждого пользователя создается демонстрационный проект.

``GET https://api.mapflow.ai/rest/projects/default`` 

Возвращает имя и ID пользователя демонстрационного проекта.

Получить все проекты
""""""""""""""""""""

``GET https://api.mapflow.ai/rest/projects`` 

Возвращает список всех проектов пользователя.  


Создать проект
"""""""""""""""""""

``POST https://api.mapflow.ai/rest/projects``

Создает новый проект и возвращает его непосредственный статус.

Пример тела запроса:

.. code:: json

    {
        "name": "test",          
        "description": "test",
        "addDefaultWds": true
    }



//Указаны название проекта и его произвольное описание

//Добавить дефолтные :doc:`сценарии обработки <../docs_userguides/pipelines>` в проект

Ответ: вновь созданный проект.

Удалить проект
""""""""""""""

``DELETE https://api.mapflow.ai/rest/projects/{projectId}`` 

Удаляет проект. Каскад удаляет все дочерние объекты.

Обработки
-----------

Получить обработку
""""""""""""""""""

``GET https://api.mapflow.ai/rest/processings/{processingId}``

Возвращает обработку с определенным ID.

Пример ответа:

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
            "tileJsonUrl": "http://localhost:8500/api/v0/cogs/tiles.json?url=s3://mapflow-rasters/4f64797d-bfb2-4433-bf56-3bcfd790ee20",
            "tileUrl": "http://localhost:8500/api/v0/cogs/tiles/{z}/{x}/{y}.png?url=s3://mapflow-rasters/4f64797d-bfb2-4433-bf56-3bcfd790ee20"
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


Получить все обработки
""""""""""""""""""""""

``GET https://api.mapflow.ai/rest/processings``

Возвращает список всех обработок пользователя.

Создать обработку
"""""""""""""""""

``POST https://api.mapflow.ai/rest/processings``

Создает и запускает обработку, а также возвращает ее непосредственное состояние.

Пример тела запроса:

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


Чтобы обработать растр, предоставленный пользователем (см. Раздел «Загрузка GeoTIFF» для обработки), установите следующие параметры: 

 .. code:: json

        "params": {
            "source_type": "tif",
            "url": "s3://mapflow-rasters/9764750d-6047-407e-a972-5ebd6844be8a/raster.tif"
        }

Ответ: вновь созданная обработка.

Перезапустить обработку
^^^^^^^^^^^^^^^^^^^^^^^

``POST https://api.mapflow.ai/rest/processings/{processingId}/restart``  

Перезапускает неудачные части обработки (не запускает удавшиеся части обработки). Каждый рабочая обработка перезапускается с первого неудачного этапа. Таким образом, выполняется минимально возможный объем работы, чтобы попытаться привести обработку к лучшему результату.

Удалить обработку
^^^^^^^^^^^^^^^^^

``DELETE https://api.mapflow.ai/rest/processings/{processingId}``

Удаляет обработку. Каскад удаляет все дочерние объекты.

Получить обработку определенной области
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``GET https://api.mapflow.ai/rest/processings/{processingId}/aois``  

Возвращает список определенных географических областей для обработки в GeoJSON.  

Пример ответа:


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


Загрузить результаты обработки
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``GET https://api.mapflow.ai/rest/processings/{processingId}/result``

Возвращает результаты обработки в .geojson в виде потока октетов. Следует вызывать только при успешно завершенной обработке.


Загрузить GeoTIFF для обработки
-------------------------------

``POST https://api.mapflow.ai/rest/rasters``

Может использоваться для загрузки растра для дальнейшей обработки. Возвращает url загруженного растра. На этот url можно ссылаться при запуске обработки. Запрос представляет собой составной запрос, единственная часть которого «file» - содержит растр. 

Пример запроса с cURL:
  

    .. code:: bash

          curl -X POST \
          https://api.mapflow.ai/rasters \
          -H 'authorization: <Insert auth header value>' \
          -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
          -F file=@custom_raster.tif


Пример ответа:  

``{"url": "s3://mapflow-rasters/9764750d-6047-407e-a972-5ebd6844be8a/raster.tif"}``


API справочник
--------------

wdName
""""""

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - ЗНАЧЕНИЕ
     - ОПИСАНИЕ
   * - Buildings Detection
     - Распознавание зданий и строений
   * - Forest Detection
     - Распознавание древесной растительности
   * - Roads Detection
     - Распознавание дорог



source_type
"""""""""""
.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - ЗНАЧЕНИЕ
     - ОПИСАНИЕ
   * - XYZ
     - URL сервиса изображений в формате «XYZ», например `https://tile.openstreetmap.org/{z}/{x}/{y}.png <https://tile.openstreetmap.org/{z}/{x}/{y}.png>`_
   * - TMS
     - Аналогично XYZ с обратной координатой Y
   * - WMS
     - URL сервиса изображений в формате «WMS», например `https://services.nationalmap.gov/arcgis/services/ USGSNAIPImagery/ImageServer/WMSServer <https://services.nationalmap.gov/arcgis/services/USGSNAIPImagery/ImageServer/WMSServer>`_
   * - Quadkey
     - Индексированный ключ, обозначающий привязку тайлов в XY координатах (например, Bing Maps)
   * - TIF/TIFF
     - Одно изображение в формате GeoTIFF



status
""""""

.. list-table::
   :widths: 10 30
   :header-rows: 1

   * - ЗНАЧЕНИЕ
     - ОПИСАНИЕ
   * - UNPROCESSED
     - Обработка еще не началась
   * - IN_PROGRESS
     - Обработка идет (или находится в очереди)
   * - FAILED
     - Обработка закончилась неудачно - измените неверные параметры или попробуйте перезапустить
   * - OK
     - Обработка завершена на 100 процентов

