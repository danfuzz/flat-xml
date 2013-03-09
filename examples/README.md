Flat-XML Examples
=================

```xml
<?xml version="1.0"?>
<cat>
<variety>Nebelung</variety>
<name><first>Francis</first><middle>Greyscale</middle><last>Lux</last></name>
<origin type="informal">San Francisco SPCA</origin>
</cat>
```

```shell
$ flat-xml greyscale.xml
/?xml {
/?xml/@version {
/?xml/@version - 1.0
/?xml/@version }
/?xml }
/cat {
/cat +
/cat -
/cat/variety {
/cat/variety - Nebelung
/cat/variety }
/cat +
/cat -
/cat/name {
/cat/name/first {
/cat/name/first - Francis
/cat/name/first }
/cat/name/middle {
/cat/name/middle - Greyscale
/cat/name/middle }
/cat/name/last {
/cat/name/last - Lux
/cat/name/last }
/cat/name }
/cat +
/cat -
/cat/origin {
/cat/origin/@type {
/cat/origin/@type - informal
/cat/origin/@type }
/cat/origin - San Francisco SPCA
/cat/origin }
/cat +
/cat -
/cat }
```

Note that `flat-xml` does not ignore any whitespace, which is what the
multiple `+`-then-`-` lines (above) are all about.

```xml
<!-- One of my favorites. -->
<poem>A peanut sat upon a track.
Its heart was all a-flutter.
A train came speeding down that track.
Toot! Toot! Peanut butter.

    -- Ogden Nash</poem>
```

```shell
$ flat-xml peanut.xml
/poem {
/poem + A peanut sat upon a track.
/poem + Its heart was all a-flutter.
/poem + A train came speeding down that track.
/poem + Toot! Toot! Peanut butter.
/poem +
/poem -     -- Ogden Nash
/poem }
```

```shell
$ flat-xml --quote peanut.xml
/poem {
/poem - $'A\x20peanut\x20sat\x20upon\x20a\x20track.\nIts\x20heart\x20was\x20all\x20a-flutter.\nA\x20train\x20came\x20speeding\x20down\x20that\x20track.\nToot!\x20Toot!\x20Peanut\x20butter.\n\n\x20\x20\x20\x20--\x20Ogden\x20Nash'
/poem }
```

The `--quote` version isn't the prettiest thing in the world; that's
not the point. The point is that we can do stuff like this. (Imagine
that `/poem` is a deeper-nested path within some more complicated
document.):

```shell
$ eval echo $(flat-xml --quote peanut.xml | awk '$1 == "/poem" && $2 == '-' { print $3 }')
A peanut sat upon a track.
Its heart was all a-flutter.
A train came speeding down that track.
Toot! Toot! Peanut butter.

    -- Ogden Nash
```
