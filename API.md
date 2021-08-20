#  Common information

*  Every route described below must be prefixed with this base url: [https://sharedcanvas.be/IIIF](https://sharedcanvas.be/IIIF)

# Get IIIF v2 manifest

See also [IIIF v2 manifests](https://iiif.io/api/presentation/2.1/#manifest)

## GET /manifests/:_id

> e.g. [https://sharedcanvas.be/IIIF/manifests/A0CE18A8-1FD8-11EA-B34A-B59DF03B8D53](https://sharedcanvas.be/IIIF/manifests/A0CE18A8-1FD8-11EA-B34A-B59DF03B8D53)

Returns: IIIF v2 manifest (JSON). This is the canonical URL for this resource.

Parameters are enclosed within the url path:

* **_id** : record identifier

## GET /:department_id/manifests/:alternative_id

> e.g. [https://sharedcanvas.be/IIIF/GUM/manifests/AMUG_00056](https://sharedcanvas.be/IIIF/GUM/manifests/AMUG_00056)

Returns: HTTP redirect to the canonical URL of the manifest.

Parameters are enclosed within the url path:

* **department_id** : department identifier

* **alternative_id** : alternative identifier

# Search within IIIF v2 manifest

See also [IIIF Content Search API 1.0](https://iiif.io/api/search/1.0/)

Note: we use version 0.9 because the viewer [universalviewer](http://universalviewer.io/) expects that version. But there are no functional differences between version 0.9 and 1.0

## GET /manifests/:_id/search

> e.g. [https://sharedcanvas.be/IIIF/manifests/A0CE18A8-1FD8-11EA-B34A-B59DF03B8D53/search](https://sharedcanvas.be/IIIF/manifests/A0CE18A8-1FD8-11EA-B34A-B59DF03B8D53/search)

Returns: IIIF v2 AnnotationList (JSON)

Parameters are enclosed within the url path:

* **_id** : record identifier

# Get IIIF v2 collection

See also [IIIF v2 collections](https://iiif.io/api/presentation/2.1/#collection)

## GET /collections/:_id

> e.g. [https://sharedcanvas.be/IIIF/collections/ad558740-3073-11ea-9f84-eb0ea02fbe65](https://sharedcanvas.be/IIIF/collections/ad558740-3073-11ea-9f84-eb0ea02fbe65)

Returns: IIIF v2 collection (JSON). This is the canonical URL for this resource.

Parameters are enclosed within the url path:

* **_id** : record identifier

## GET /:department_id/collections/:alternative_id

> e.g. [https://sharedcanvas.be/IIIF/GUM/collections/dragendorff](https://sharedcanvas.be/IIIF/GUM/collections/dragendorff)

Returns: HTTP redirect to the canonical URL of the collection.

Parameters are enclosed within the url path:

* **department_id** : department identifier

* **alternative_id** : alternative identifier

## GET /:department_id/discover

> e.g. [https://sharedcanvas.be/IIIF/GUM/discover](https://sharedcanvas.be/IIIF/GUM/discover)

Returns: IIIF v2 Collection (JSON) of manifests belonging to department `:department_id`

Notes:

* when the collection contains more than 50 manifests, then the results are split into [pages](https://iiif.io/api/presentation/2.1/#paging).
* Each page is represented by a collection
with its own url. The main collection contains the total number of manifests, and refers to the first page, but does NOT include any links to manifests.

> e.g. compare [main collection](https://sharedcanvas.be/IIIF/GUM/discover) to the [first page](https://sharedcanvas.be/IIIF/GUM/discover?limit=50&q=&start=0)

Query parameters:

* **q**: search query. No special syntax. Search behaviour is determined by the api internally.
* **start**: return list of manifests starting from index `start`. Must be natural number, starting at `0`
* **limit**: limit of list of manifests returned. Must be natural number
