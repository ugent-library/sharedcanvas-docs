How to collect manifests and their "metadata" for the MMMONK project
--------------------------------------------------------------------

## Retrieve IIIF v2 collection for manifests from adore.ugent.be

[https://adore.ugent.be/IIIF/sets/mmmonk](https://adore.ugent.be/IIIF/sets/mmmonk) returns a [IIIF v2 collection](https://iiif.io/api/presentation/2.1/#collection) (HTTP headers included below):

```
HTTP/1.1 200 OK
Server: openresty/1.15.8.2
Date: Fri, 17 Dec 2021 09:27:01 GMT
Content-Type: application/ld+json;charset=utf-8
Content-Length: 203
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: accept, origin, x-requested-with, content-type, x-transmission-session-id, authorization, cookie
Access-Control-Expose-Headers: X-Transmission-Session-Id
Access-Control-Allow-Methods: GET, POST, OPTIONS, X-Transmission-Session-Id
Access-Control-Allow-Credentials: true

{
  "first":"https://adore.ugent.be/IIIF/sets/mmmonk?start=0",
  "total":106,
  "@type":"sc:Collection",
  "@context":"http://iiif.io/api/presentation/2/context.json",
  "@id":"https://adore.ugent.be/IIIF/sets/mmmonk"
}
```

This collection only returns a summary of what is available, and does not include the manifests themselves, but acts as a starting point. The IIIF specification expects collections to contain either all manifests or split the collection into multiple when too many. In that last case, each of those separate collections act as a "page". See [paging in a iiif collection](https://iiif.io/api/presentation/2.1/#paging)

Available attribute in this "summary" collection are:

* **first**: link to the first page of manifests (50 per page)
* **total**: total number of manifests in this collection

If you retrieve the link from attribute **first** you'll see a collection that works in a IIIF viewer:

```
{
   "total" : 106,
   "@context" : "http://iiif.io/api/presentation/2/context.json",
   "@id" : "https://adore.ugent.be/IIIF/sets/mmmonk?start=0",
   "startIndex" : 0,
   "manifests" : [
      {
         "label" : "Secreta secretorum en andere werken[manuscript]",
         "@type" : "sc:Manifest",
         "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A010C9ED6-94DB-11E3-AFBA-2845D43445F2"
      },
      {
         "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A018970A2-B1E8-11DF-A2E0-A70579F64438",
         "label" : "Liber Floridus [manuscript]",
         "@type" : "sc:Manifest"
      }
   ],
   "@type" : "sc:Collection",
   "next" : "https://adore.ugent.be/IIIF/sets/mmmonk?start=50",
   "within" : "https://adore.ugent.be/IIIF/sets/mmmonk"
}
```

(Important: the list of manifests in this example is truncated)

This is a collection that shows the first page of manifests, not all of them.

Now see extra attributes:

* **next**: link to next page of manifests (if applicable)
* **prev**: link to previous page of manifests (if applicable)
* **manifests**: list of manifests. Manifests are never embedded in a collection, but only referred to by `@id`. Attributes like `label` are included, but only so that viewers may present a list without having to retrieve all manifests.

With attributes **prev** and **next** one can scroll to all manifests.

## Retrieve IIIF v2 manifest from adore.ugent.be

Every IIIF v2 manifest from adore.ugent.be will look like this (simplified version):

```
{
   "@context" : "http://iiif.io/api/presentation/2/context.json",
   "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0",
   "@type" : "sc:Manifest",
   "seeAlso": {
      "@id": "https://lib.ugent.be/catalog/rug01%3A000781999.marcxml",
      "dcterms:format": "application/marcxml+xml"
   },
   "attribution" : "Provided by Ghent University Library",
   "label" : "Antiphonarium van de Sint-Baafsabdij te Gent",
   "metadata" : [
      {
         "value" : "[manuscript] Antiphonarium van de Sint-Baafsabdij te Gent.",
         "label" : "Title"
      },
      {
         "value" : "2 bd. (360 ; 337 f.) : miniaturen, initialen, randversiering, muziek; 545 x 385 mm ; 540 x 380 mm.",
         "label" : "Description"
      }
   ],
   "sequences" : [
      {
         "@type" : "sc:Sequence",
         "canvases" : [
            {
               "height" : 6032,
               "images" : [
                  {
                     "motivation" : "sc:painting",
                     "resource" : {
                        "service" : {
                           "@context" : "http://iiif.io/api/image/2/context.json",
                           "profile" : "http://iiif.io/api/image/2/level1.json",
                           "@id" : "https://adore.ugent.be/IIIF/images/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0%3ADS.3"
                        },
                        "height" : 6032,
                        "label" : "BHSL-HS-0015-02_2010_0001_AC.jp2",
                        "format" : "image/jpeg",
                        "@id" : "https://adore.ugent.be/IIIF/images/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0%3ADS.3/full/full/0/native.jpg",
                        "@type" : "dctypes:Image",
                        "width" : 9246
                     },
                     "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/canvases/DS.3/image-annotations/0",
                     "on" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/canvases/DS.3",
                     "@type" : "oa:Annotation"
                  }
               ],
               "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/canvases/DS.3",
               "@type" : "sc:Canvas",
               "label" : "-",
               "width" : 9246,
            }
         ],
         "viewingDirection" : "left-to-right",
         "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/sequences/0"
      }
   ]
}
```

Metadata can be retrieved as follows:

* [MARC XML](http://www.loc.gov/standards/marcxml/) metadata can be retrieved using the url in `seeAlso.@id`.
* The [IIIF metadata fields](https://iiif.io/api/presentation/2.1/#metadata) can be retrieved from `metadata`, but these are meant for presentation (in a viewer)

## Retrieve IIIF v2 collection for manifests from sharedcanvas.be

[https://sharedcanvas.be/IIIF/mmmonk/discover](https://sharedcanvas.be/IIIF/mmmonk/discover) returns a [IIIF v2 collection](https://iiif.io/api/presentation/2.1/#collection), containing links to manifests

Same rules apply to this collection as for the one from adore.ugent.be

Were are the images?
--------------------

All images are served by adore.ugent.be and sharedcanvas.be.

Every [Canvas](https://iiif.io/api/presentation/3.0/#53-canvas) in the manifest has a link to its image api service url in `images.0.resource.service.@id` which is compliant with the [IIIF v2.0 Image API](https://iiif.io/api/image/2.0/):

Example:

```
{
   "height" : 6032,
   "images" : [
      {
         "motivation" : "sc:painting",
         "resource" : {
            "service" : {
               "@context" : "http://iiif.io/api/image/2/context.json",
               "profile" : "http://iiif.io/api/image/2/level1.json",
               "@id" : "https://adore.ugent.be/IIIF/images/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0%3ADS.3"
            },
            "height" : 6032,
            "format" : "image/jpeg",
            "@id" : "https://adore.ugent.be/IIIF/images/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0%3ADS.3/full/full/0/native.jpg",
            "@type" : "dctypes:Image",
            "width" : 9246
         },
         "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/canvases/DS.3/image-annotations/0",
         "on" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/canvases/DS.3",
         "@type" : "oa:Annotation"
      }
   ],
   "@id" : "https://adore.ugent.be/IIIF/manifests/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0/canvases/DS.3",
   "@type" : "sc:Canvas",
   "width" : 9246
}
```

In this example the service url is https://adore.ugent.be/IIIF/images/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0%3ADS.3.

This is the [base url](https://adore.ugent.be/IIIF/images/archive.ugent.be%3A8197A220-F705-11DF-8137-96239BE017E0%3ADS.3) of the image api of that specific resource.

In order to retrieve the [info.json](https://iiif.io/api/image/2.0/#information-request), append `/info.json` to the url, and visit it:

```
{
   "@id" : "https://adore.ugent.be/IIIF/images/archive.ugent.be:8197A220-F705-11DF-8137-96239BE017E0:DS.3",
   "@context" : "http://iiif.io/api/image/2/context.json",
   "protocol" : "http://iiif.io/api/image",
   "width" : 9246,
   "tiles" : [
      {
         "height" : 256,
         "width" : 256,
         "scaleFactors" : [
            1,
            2,
            4,
            8,
            16,
            32,
            64
         ]
      }
   ],
   "height" : 6032,
   "profile" : [
      "http://iiif.io/api/image/2/level1.json",
      {
         "supports" : [
            "regionByPct",
            "regionSquare",
            "sizeByForcedWh",
            "sizeByWh",
            "sizeAboveFull",
            "rotationBy90s",
            "mirroring"
         ],
         "maxWidth" : 4000,
         "maxHeight" : 4000,
         "formats" : [
            "jpg"
         ],
         "qualities" : [
            "native",
            "color",
            "gray",
            "bitonal"
         ]
      }
   ],

   "sizes" : [
      {
         "height" : 94,
         "width" : 144
      },
      {
         "width" : 288,
         "height" : 188
      },
      {
         "width" : 577,
         "height" : 377
      },
      {
         "height" : 754,
         "width" : 1155
      },
      {
         "width" : 2311,
         "height" : 1508
      }
   ]
}
```

With this information a viewer that is compliant with the IIIF v2 Image API knows
how it can retrieve slices of the image, in what size and in what format.

Note that our image api always redirects to the info.json url if the base url of an image is requested.

If the viewer supports the specified Image API version, no further development should be done.
The `info.json` is retrieved, and the viewer uses that to build a "zoomer application"
by requesting "tiles" at different resolution levels.
