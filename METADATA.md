# IIIFv2 metadata section

Every IIIFv2 manifest has a [metadata](https://iiif.io/api/presentation/2.1/#metadata) section (only part shown):

```
{
  ..
  "metadata": [
    {"label": "Author", "value": "Anne Author"},
    {"label": "Notes", "value": ["Text of note 1", "Text of note 2"]}
  ]
  ..
}
```

This attribute `metadata` contains an ordered list of pairs with a `label` and a `value`.

The presentation api does not pose any restriction on the used vocabulary,
as you would expect from metadata standards like MARC-21 or Dublin Core.

IIIF viewers however tend to pose restrictions on the contents of the attributes,
both label and value; i.e. only a limited set of html tags are allowed (`<br>`, `<p>`, `<b>` ..).
You can for example add html links (`<a>`), but the `target` attribute will
always be set to `_blank` (opens in a different tab or window).

IIIF viewers add this restriction both for security reasons (e.g. javascript that generates "adds"),
and because random html could possibly mess up the entire layout of your
viewer, which is, of course, also written in html.

# Generate IIIFv2 metadata in sharedcanvas

In SharedCanvas this section is generated using one of the following files in your record:

## dc.xml

If you upload a file with name `dc.xml`, it is considered to be an XML file
that conforms to the Dublin Core standard, more specifically the [OAI DC standard](https://guidelines.readthedocs.io/en/latest/literature/use_of_oai_dc.html)
from OpenArchives.org.

All fields can be repeated

Example:

```
<oai_dc:dc xmlns:oai_dc="http://www.openarchives.org/OAI/2.0/oai_dc/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.openarchives.org/OAI/2.0/oai_dc/ http://www.openarchives.org/OAI/2.0/oai_dc.xsd" xmlns:dc="http://purl.org/dc/elements/1.1/">
  <dc:title>Acta ornithologica.</dc:title>
  <dc:creator>Polska akademia nauk. Instytut zoologiczny</dc:creator>
  <dc:type>text</dc:type>
  <dc:publisher>Warszawa : Polska akademia nauk. Instytut zoologiczny,</dc:publisher>
  <dc:date>n.d.</dc:date>
</oai_dc:dc>
```

All fields are converted in order to the IIIF metadata equivalent:

```
"metadata": [
  { "label" : "title", "value": "Acta ornithologica." },
  { "label" : "creator", "value": "Polska akademia nauk. Instytut zoologiczny" },
  { "label" : "type", "value": "text" },
  { "label" : "publisher", "value": "Warszawa : Polska akademia nauk. Instytut zoologiczny," },
  { "label" : "date", "value": "n.d." }
]
```

## marc.xml

If you upload a file with name `marc.xml`, it is considered to be [MARC-21 XML](https://www.loc.gov/standards/marcxml/) file

Due to the nature of MARC, it is not possible to provide a widely accepted
conversion to a list of pairs in IIIF.

By default the MARC is converted to Dublin Core using an [XSLT file from the Library of Congress](https://www.loc.gov/standards/marcxml/xslt/MARC21slim2OAIDC.xsl).

Example record:

```
<marc:record xmlns:marc="http://www.loc.gov/MARC21/slim">
  <marc:leader>00000nam a22 a 4500</marc:leader>
  <marc:controlfield tag="001">000967677</marc:controlfield>
  <marc:controlfield tag="003">BE-GnUNI</marc:controlfield>
  <marc:controlfield tag="005">20180604145405.0</marc:controlfield>
  <marc:controlfield tag="008">060503q12011250be |r |000 ||lat|d</marc:controlfield>
  <marc:datafield ind1="7" ind2=" " tag="024">
    <marc:subfield code="9">001</marc:subfield>
    <marc:subfield code="a">archive.ugent.be:0E183B7C-B33B-11E6-85BF-00F9D43445F2</marc:subfield>
    <marc:subfield code="d">910000094021</marc:subfield>
    <marc:subfield code="c">online-open</marc:subfield>
    <marc:subfield code="2">info</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="040">
    <marc:subfield code="a">BE-GnUNI</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1="1" ind2=" " tag="100">
    <marc:subfield code="a">Papias grammaticus</marc:subfield>
    <marc:subfield code="0">(viaf)72189246</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1="1" ind2=" " tag="245">
    <marc:subfield code="k">[manuscript]</marc:subfield>
    <marc:subfield code="a">Elementarium doctrinae rudimentum. De finalibus ...</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1="2" ind2=" " tag="246">
    <marc:subfield code="a">Grammatical treatises</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="260">
    <marc:subfield code="a">Zuidelijke Nederlanden,</marc:subfield>
    <marc:subfield code="c">1e helft 13e eeuw.</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="300">
    <marc:subfield code="a">206 f. :</marc:subfield>
    <marc:subfield code="b">initialen ;</marc:subfield>
    <marc:subfield code="c">365 x 255 mm.</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="340">
    <marc:subfield code="a">Perkament</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="505">
    <marc:subfield code="a">Inhoudstafel en nota's (schutblad, f. 1v); Papias, Elementarium doctrinae rudimentum, A-N (ff. 2r-192r); Servius, De finalibus (ff. 192v-194r); De nominibus mensium (f. 194r-v); Etymologia (ff. 194v-201r); Versus de praeteritis et supinis (ff. 201r-205r); Algorismus (ff. 205r-206v).</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="505">
    <marc:subfield code="a">De finalibus ... / Servius</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="581">
    <marc:subfield code="a">De Saint-Genois, J. Catalogue ... des manuscrits de la bibliothèque ... de l'université de Gand; 293</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="581">
    <marc:subfield code="a">Derolez, A. Medieval manuscripts Ghent University library p. 49, 51</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="561">
    <marc:subfield code="a">Herkomst: Cambron (13e eeuw)</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="540">
    <marc:subfield code="a">CC-BY-SA-4.0</marc:subfield>
    <marc:subfield code="u">https://creativecommons.org/licenses/by-sa/4.0/</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="591">
    <marc:subfield code="a">Bijzondere collecties</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2=" " tag="591">
    <marc:subfield code="a">Medieval manuscripts</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1=" " ind2="7" tag="655">
    <marc:subfield code="a">Manuscripts.</marc:subfield>
    <marc:subfield code="2">UGENT</marc:subfield>
  </marc:datafield>
  <marc:datafield ind1="1" ind2=" " tag="700">
    <marc:subfield code="a">Servius,</marc:subfield>
    <marc:subfield code="d">active 4th century</marc:subfield>
    <marc:subfield code="0">(viaf)10970700</marc:subfield>
  </marc:datafield>
</marc:record>
```

## bag-info.txt

If you upload a file with name `bag-info.txt`, a regular text file is expected.

Each line represents a pair.

Each pair must be formatted as such: `$label: $value`

Labels can be repeated

New lines are best generated using the html tag `<br>`

```
Title: my very long title
Author: first author
Author: second author
```

## iiif.json (upcoming)

If you upload a file with name `iiif.json`, it is considered a JSON file.
This json file is merged into the final IIIFv2 manifest.

For now only attribute `metadata` is allowed here:

```
{

  "metadata": [
    {"label": "Author", "value": "Anne Author"},
    {"label": "Notes", "value": ["Text of note 1", "Text of note 2"]}
  ]

}
```

# Provide updates for your metadata

If you already created a lot of records in sharedcanvas,
and you want regular updates to your IIIFv2 metadata,
it is best to provide us with a JSON:

Each line must be json object, that contains attributes:
 
* `metadata`
  * required: true
  * description: (see explanation about iiif.json above)
* `_id`
  * required: true
  * description: record identifier of your

Example:

```
{ "_id": "my-id-1", "metadata": [{"label": "Author", "value": "Anne Author"}, {"label": "Notes", "value": ["Text of note 1", "Text of note 2"]} ] }
{ "_id": "my-id-2", "metadata": [{"label": "Author", "value": "Anne Author"}, {"label": "Notes", "value": ["Text of note 2", "Text of note 3"]} ] }
```  
