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
  * Selecting "Chapter 1", "Chapter 2", "Chapter 3" or "Picture of House" will set viewer to page containing that start of that chapter or the picture.

## Extents derived from hierarchy of parts (with Canvas in Range)

  * `members` is used to indicate the list of parts within a struture.
  * `viewingHint` of `no-nav` is used to indicate portions that should not appear in the navigation hierarchy (re-use of `none` is tempting be has potential conflict with use to control rendering of content)
  * Here we allow direct inclusion of whole canvases which has a problem with the context-specific application of `label` and `viewingHint` to an existing object

[Example Manifest](book_simple_ext1.json)

## Extents derived from hierarchy of parts (extras Ranges to wrap whole Canvas)

  * `members` is used to indicate the list of parts within a struture.
  * `viewingHint` of `no-nav` is used to indicate portions that should not appear in the navigation hierarchy (re-use of `none` is tempting be has potential conflict with use to control rendering of content)
  * Addition `Range` objects used to avoid direct inclusion of whole canvases and context-specific application of `label` and `viewingHint` to an existing object. We relax the [current requirement for every `member` object to have a `label`](http://iiif.io/api/presentation/2.1/#members)
    * Should `no-nav` be implied for a Range with one Canvas as a member?

[Example Manifest](book_simple_ext2.json)

## Extents derived from embedded hierarchy of parts (extras Ranges to wrap whole Canvas)

  * `members` is used to indicate the list of parts within a struture.
  * Canvases are always treated as no-nav, Ranges as nav.
  * We get rid of the flat list and deeply nest the structure to avoid repetition
  * `viewingHint` of `no-nav` could be used to indicate Ranges that should not appear in the navigation hierarchy
  * Addition `Range` objects used to avoid context-specific application of `label` and `viewingHint` to an existing object. We relax the [current requirement for every `member` object to have a `label`](http://iiif.io/api/presentation/2.1/#members)

[Example Manifest](book_simple_ext3.json)


## Extents specified separately from hierarchy

  * `extents` is used to provide the list of parts within a struture.
  * `members` is used to provide the list of navigation components within a structure. The content of the `members` _SHOULD_ be included with the `extents`. Would need to define expected behavior if this is not the case (error or whether it is OK for viewer to play part that does not include the sub-parts).

[Example Manifest](book_simple_extents_and_members.json)
