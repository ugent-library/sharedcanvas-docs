sharedcanvas.be
===============

## What does it do?

sharedcanvas.be is an administration website that allows

you to create [IIIF v2](https://iiif.io/api/presentation/2.1/) manifests, that can be viewed by any

viewer that supports [IIIF v2](https://iiif.io/api/presentation/2.1/).

e.g. Go to http://universalviewer.io/#view, and enter this manifest URL

    https://sharedcanvas.be/IIIF/manifests/B_OB_MS354

sharedcanvas.be is also the domain where these manifests are hosted,

as you can see in the URL above.

## What do I need (to do) to create one manifest (at minimum)?

* a list of <b>images</b> that should be viewed together in a carousel/book viewer <b>[required]</b>

  * supported file types are `jpg`, `tif`, `png` and `jp2`.

  * the alphabetic order of the image names should reflect the intended order in the viewer. e.g. `0001.jpg, 0002.jpg ..` and NOT `1.jpg, 2.jpg .. 10.jpg` as alphabetically `9.jpg` is bigger than `10.jpg`. For simplicity we just sort the images alphabetically.

  * each image becomes visible as a `canvas` in the manifest

* a <b>label</b> that should be shown in the viewer <b>[required]</b>

## But .. I have audio and video files?

Unfortunately audio and video are not supported (yet)

[IIIF v2](https://iiif.io/api/presentation/2.1/) only supports images.

Audio and video will have to wait until we support [IIIF v3](https://iiif.io/api/presentation/3.0/)

## How can I influence the metadata that is shown in the viewer?

[IIIF v2](https://iiif.io/api/presentation/2.1/) stores a **ordered** list of metadata pairs (given as `label` and `value` in a list)

```
[
  { "label": "Title", "value": "Café" },
  { "label": "Subject", "value": "Drinking" }
]
```

viewers typically show it like this

```
Title
  Café

Subject
  Drinking

```

The standard does not however enforce any vocabulary, as it focuses on presentation.

Every compatible viewer will show this list in its own way.

e.g. the universalviewer adds an extra right tab "more info"

e.g. the mirador adds a tab on the left

One can change this metadata by either..

  * uploading a file called `bag-info.txt` in this record (go to "edit record" in your record)

  ```
  Title: Café
  Subject: Drinking
  ```

  Each line should be formatted as `$label: $value` and represents a pair.

  The order is retained in the manifest metadata

  * uploading a file called `dc.xml` in this record. This is an XML file that conforms to the [OAI Dublin Core XML](http://www.openarchives.org/OAI/openarchivesprotocol.html#dublincore) standard. See also related [XSD file](http://www.openarchives.org/OAI/2.0/oai_dc.xsd) to validate this file

  ```
  <oai_dc:dc xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/oai_dc/ http://www.openarchives.org/OAI/2.0/oai_dc.xsd">
    <dc:title>Café</dc:title>
    <dc:language>fre</dc:language>
  </oai_dc:dc>
  ```

  The order of the fields is retained in the manifest metadata

  * uploading a file called `marc.xml`. This is an XML file that conforms to the [MARC 21 XML](https://www.loc.gov/standards/marcxml/) standard

  This may look like the easiest solution for some institutions,

  but beware that we use a general mapping from [MARC to Dublin Core](https://www.loc.gov/marc/marc2dc.html)

  in order to generate a list of metadata pairs

  * uploading a file called `iiif.json`. This is a JSON file that should look this

  ```
  {
    "metadata": [
      { "label": "Title", "value": "Café" },
      { "label": "Subject", "value": "Drinking" }
    ]
  }
  ```
  This way you can influence the metadata more directly, as there is no conversion involved here. This `metadata` is inserted "as is" in the manifest.

## Can we search within our manifest?

Yes if you provide the necessary OCR files. We do not generate OCR ourselves.

For each image, sharedcanvas looks for an `html` or `xml` file with the same name but with different extension.

e.g. the OCR for 0001.jpg should be stored in 0001.xml

Such an XML file must be an ALTO XML file, and should only contain data for that page.

Such an HTML file must be a hOCR file, and should only contain data for that page.
