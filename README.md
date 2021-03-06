This projects contains a set of high-res images of poker cards, for use in iOS applications. It also contains instructions to regenerate them at any resolution.

![king of spades](https://raw.githubusercontent.com/Xadeck/xCards/master/demo.png)

Initial card design obtained from https://sourceforge.net/projects/vector-cards/?source=typ_redirect
They were split into single files, and exported as SVG using [Sketch](https://www.sketchapp.com/).
They were then converted to fix the maximum sized indicated by https://www.paintcodeapp.com/news/ultimate-guide-to-iphone-resolutions, that is:

```
320x480
750x1334
1242x2208
```

Poker cards have dimensions of 63.5cmx88.9cm according to [wikipedia](https://en.wikipedia.org/wiki/Standard_52-card_deck), which means an aspect ratio of 0.714. Applying that to above dimensions with fixed width yields the following sizes:
  
```
320x448
750x1050
1242x1739
```

Using [ImageMagick](http://www.imagemagick.org/), conversion is done via:

```bash
for svg in $(find svg/face -name *.svg); do
  basename=$(basename $svg .svg)
  options="-background none -density 1200"
  convert $options -resize   320x448\! $svg png/face/${basename}@1x.png
  convert $options -resize  750x1050\! $svg png/face/${basename}@2x.png
  convert $options -resize 1242x1739\! $svg png/face/${basename}@3x.png
done
```

Joker image was found on the web. I could not get licensing information so contact me if you think it's not usable. The image was manually edited with [Pixelmator](www.pixelmator.com) to have the same round corners than the other cards, in all three resolutions.

## Rank and suits

The size below have hights matching that of cards:

```bash
for svg in $(find svg/rank -name *.svg); do
  basename=$(basename $svg .svg)
  options="-background none -density 1200"
  convert $options -resize   261x320 $svg png/rank/${basename}@1x.png
  convert $options -resize   612x750 $svg png/rank/${basename}@2x.png
  convert $options -resize 1014x1242 $svg png/rank/${basename}@3x.png
done
```

```bash
for svg in $(find svg/suit -name *.svg); do
  basename=$(basename $svg .svg)
  options="-background none -density 1200"
  convert $options -resize   198x320 $svg png/suit/${basename}@1x.png
  convert $options -resize   464x750 $svg png/suit/${basename}@2x.png
  convert $options -resize  768x1242 $svg png/suit/${basename}@3x.png
done
```

## Adding shadows

When using the images in an iOS app, use builtin shadowing support. If you want to generate an image with shadow burnt in, you can use imagemagick and the `shadow.sh` script. For example to duplicate all images under `png` directory to a `shadowed` directory (beware that it will take time).

```
mkdir -p shadowed/face
for png in $(find png/face -name ??.png); do
  ./shadow.sh $png 10 ${png/#png/shadowed}
done
```

Each size is enlarged by ~10% of its width (that is the `10` part of the command lines above).
