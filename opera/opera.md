# Exploration of an opera soundtrack use-case for [#1070](https://github.com/IIIF/iiif.io/issues/1070)

We want to describe an opera recording and to provide the following navigation:

  * A Very Short Opera
    * Act 1
      * Famous Aria
    * Act 2

Over a soundtrack that is divided into sections:

  * t=0,100 - Overture
  * t=100,1000 - Act 1
  * t=500,600 - Famous Aria
  * t=1000,2000 - Act 2

Where the desired behaviours are:

  * Selecting "A Very Short Opera" in navigation will play the whole opera including the overture (sub-sections may highlighted at appropriate points).
  * Selecting "Act 1" or "Act 2" in navigation will play the entirity of that act.
  * Selecting "Famous Aria" will play just that aria within act 1.

For the sake of simplicity, the entire soundtrack for this opera is one canvas with a duration of 2000s. There would be no significant changes in structure if it were split over more than one canvas.

## Extents derived from hierarchy of parts

  * `members` is used to indicate the list of parts within a struture. The extent is the concatenation of the members. It is expected that a viewer would be smart enough to stitch together contiguous members, but out-of-order arrangements (e.g. `[ { #t=1,2 }, { #t=0,1 }, { #t=2,3} ]`) are allowed.
  * `viewingHint` of `no-nav` is used to indicate portions that should not appear in the navigation hierarchy (re-use of `none` is tempting be has potential conflict with use to control rendering of content)

[Example Manifest](opera_ext1.json)

## Extents embedded and nested, Ranges nav, Canvases no-nav

  * `members` indicates list of parts.
  * Ranges are always in the navigation
  * Canvases can be members along with Ranges, and provide extent
  * `viewingHint` possible, but so far unnecessary

[Example Manifest](opera_ext3.json)

## Extents specified separately from hierarchy

  * `extents` is used to provide the list of parts within a struture.
  * `members` is used to provide the list of navigation components within a structure. The content of the `members` _SHOULD_ be included with the `extents`. Would need to define expected behavior if this is not the case (error or whether it is OK for viewer to play part that does not include the sub-parts).

[Example Manifest](opera_extents_members.json)

