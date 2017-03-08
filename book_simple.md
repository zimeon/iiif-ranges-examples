# Simple book use-case for [#1070](https://github.com/IIIF/iiif.io/issues/1070)

We want to describe a book and to to provide the following navigation:

  * A Short Book
    * Chapter 1
    * Chapter 2
      * Picture of House
    * Chapter 3

Over a sequence of canvases (one for each side of each page) that includes cover pages, and where some chapters start/end in the middle of a page:

![Book canvases](book_simple.png)

Where the desired behaviours are:

  * Selecting "A Short Book" in navigation will set viewer to front cover image.
  * Selecting "Capter 1", "Chapter 2", "Chapter 3" or "Picture of House" will set viewer to page containing that start of that chapter or the picture.

## Extents derived from hierarchy of parts (with Canvas in Range)

  * `members` is used to indicate the list of parts within a struture.
  * `viewingHint` of `no-nav` is used to indicate portions that should not appear in the navigation hierarchy (re-use of `none` is tempting be has potential conflict with use to control rendering of content)
  * Here we allow direct inclusion of whole canvases which has a problem with the context-specific application of `label` and `viewingHint` to an existing object

```json
{
    "type": "Manifest",
    "label": "A Short Book -- extents derived from hierarchy of parts",
    "sequences": [ "/*...define http://example.org/iiif/canvas/1 in here...*/" ],
    "structures": [
        {
            "id": "http://example.org/iiif/range/0",
            "type": "Range",
            "viewingHint": "top",
            "label": "A Short Book",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/1", 
                    "type": "Canvas",
                    "label": "Front cover",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/canvas/2", 
                    "type": "Canvas",
                    "label": "Inside front cover",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/range/1", 
                    "type": "Range",
                    "label": "Chapter 1"
                },
                {
                    "id": "http://example.org/iiif/range/2", 
                    "type": "Range",
                    "label": "Chapter 2"
                },
                {
                    "id": "http://example.org/iiif/canvas/6", 
                    "type": "Canvas",
                    "label": "Chapter 3"
                },
                {
                    "id": "http://example.org/iiif/canvas/7",
                    "type": "Canvas",
                    "label": "Inside back cover",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/canvas/8",
                    "type": "Canvas",
                    "label": "Back cover",
                    "viewingHint": "no-nav"
                }
            ]
        },
        {
            "id": "http://example.org/iiif/range/1",
            "type": "Range",
            "label": "Chapter 1",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/3", 
                    "type": "Canvas",
                    "label": "Chapter 1, p1",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/canvas/4#xywh=0,0,600,400", 
                    "type": "Canvas",
                    "label": "Chapter 1, p2",
                    "viewingHint": "no-nav"
                }
            ]
        },
        {
            "id": "http://example.org/iiif/range/2",
            "type": "Range",
            "label": "Chapter 1",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/4#xywh=0,401,600,800", 
                    "type": "Canvas",
                    "label": "Chapter 2, p1",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/canvas/5#xywh=0,0,600,200, 
                    "type": "Canvas",
                    "label": "Chapter 2, p2 above picture",
                    "viewingHint": "no-nav"
                }
                {
                    "id": "http://example.org/iiif/canvas/5#xywh=0,201,600,600, 
                    "type": "Canvas",
                    "label": "Picture of House",
                }
                {
                    "id": "http://example.org/iiif/canvas/5#xywh=0,601,600,800, 
                    "type": "Canvas",
                    "label": "Chapter 2, p2 below picture",
                    "viewingHint": "no-nav"
                }
            ]
        }
    ]   
}
```

## Extents derived from hierarchy of parts (with Canvas in Range)

  * `members` is used to indicate the list of parts within a struture.
  * `viewingHint` of `no-nav` is used to indicate portions that should not appear in the navigation hierarchy (re-use of `none` is tempting be has potential conflict with use to control rendering of content)
  * Addition `Range` objects used to avoid direct inclusion of whole canvases and context-specific application of `label` and `viewingHint` to an existing object. We relax the [current requirement for every `member` object to have a `label`](http://iiif.io/api/presentation/2.1/#members)
    * Should `no-nav` be implied for a Range with one Canvas as a member?

```json
{
    "type": "Manifest",
    "label": "A Short Book -- extents derived from hierarchy of parts, extra ranges",
    "sequences": [ "/*...define http://example.org/iiif/canvas/1 in here...*/" ],
    "structures": [
        {
            "id": "http://example.org/iiif/range/0",
            "type": "Range",
            "viewingHint": "top",
            "label": "A Short Book",
            "members": [
                {
                    "id": "http://example.org/iiif/range/1", 
                    "type": "Range",
                    "label": "Front cover",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/range/2", 
                    "type": "Range",
                    "label": "Chapter 1"
                },
                {
                    "id": "http://example.org/iiif/range/3", 
                    "type": "Range",
                    "label": "Chapter 2"
                },
                {
                    "id": "http://example.org/iiif/range/4", 
                    "type": "Range",
                    "label": "Chapter 3"
                },
                {
                    "id": "http://example.org/iiif/range/5",
                    "type": "Range",
                    "label": "Back cover",
                    "viewingHint": "no-nav"
                }
            ]
        },
        {
            "id": "http://example.org/iiif/range/1",
            "type": "Range",
            "label": "Front cover",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/1", 
                    "type": "Canvas"
                },
                {
                    "id": "http://example.org/iiif/canvas/2", 
                    "type": "Canvas"
                },
            ]
        },
        {
            "id": "http://example.org/iiif/range/2",
            "type": "Range",
            "label": "Chapter 1",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/3", 
                    "type": "Canvas",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/canvas/4#xywh=0,0,600,400", 
                    "type": "Canvas",
                    "viewingHint": "no-nav"
                }
            ]
        },
        {
            "id": "http://example.org/iiif/range/3",
            "type": "Range",
            "label": "Chapter 2",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/4#xywh=0,401,600,800", 
                    "type": "Canvas",
                    "viewingHint": "no-nav"
                },
                {
                    "id": "http://example.org/iiif/canvas/5#xywh=0,0,600,200", 
                    "type": "Canvas",
                    "viewingHint": "no-nav"
                }
                {
                    "id": "http://example.org/iiif/canvas/5#xywh=0,201,600,600", 
                    "type": "Canvas",
                    "label": "Picture of House",
                }
                {
                    "id": "http://example.org/iiif/canvas/5#xywh=0,601,600,800", 
                    "type": "Canvas",
                    "viewingHint": "no-nav"
                }
            ]
        },
        {
            "id": "http://example.org/iiif/range/4",
            "type": "Range",
            "label": "Chapter 3",
            "members": [
                {
                    "id": "http://example.org/iiif/canvas/6", 
                    "type": "Canvas"
                }
            ]
        },
        }
    ]   
}
```

## Extents specified separately from hierarchy

  * `extents` is used to provide the list of parts within a struture.
  * `members` is used to provide the list of navigation components within a structure. The content of the `members` _SHOULD_ be included with the `extents`. Would need to define expected behavior if this is not the case (error or whether it is OK for viewer to play part that does not include the sub-parts).

```json
{
    ...   
}
```