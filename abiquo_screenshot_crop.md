# Automating Screenshot Cropping for Abiquo Documentation

## Situation

Producing major release documentation for the Abiquo product requires capturing and cropping around 150 dialog box screenshots per release cycle. The dialog boxes appear bright against the darker background of the web application, so each screenshot must be cropped to show only the dialog box before it can be published to the Abiquo wiki.

In 2015, screenshots were taken using the Firefox browser or Shutter on Linux. By 2020, the workflow had shifted to Snagit on a Mac with a ZSH script to run the cropping commands. In both cases, a screenshot might contain a dialog box, or it might be a main screen with no dialog box at all — meaning the cropping process also needed to detect which type of screenshot it was dealing with.

## Obstacle

Manually cropping hundreds of screenshots for every major release was slow and tedious, consuming many hours of editing time. The process also had to handle several complicating factors:

- Screenshots could contain a dialog box **or** just a main screen, requiring detection logic before cropping  
- Some dialog types (e.g. VDC and vapp screens) did not respond correctly to the standard crop command  
- High-contrast objects in the background — such as a mouse pointer — could cause the imagemagick `convert` commands to fail  
- The original workflow depended on Shutter, which was dropped from Ubuntu 18.04 and was no longer maintained

## Action

The team built a shell script using [Imagemagick](http://www.imagemagick.org/) to automate the cropping. The script works in two stages: first detecting whether a dialog box is present, then applying the appropriate crop command.

**Step 1 — Detect whether a dialog box exists**

A `convert` command adds contrast borders to the image and trims the background. If no dialog box is present, the trim removes a large portion of the image:

```
convert "${directory}${ifile}" \\

 \-bordercolor gray75 \-compose copy \-border 3 \\

 \-bordercolor gray58 \-compose copy \-border 3 \\

 \-fuzz 50% \-trim \+repage "${output\_directory}$ifileout"
```
The `identify` command then measures how much the width changed:
```
new\_width=$(magick identify "${output\_directory}$ifileout" | awk \-F "\[ \]\\|x" '{print $3}')

new\_height=$(magick identify "${output\_directory}$ifileout" | awk \-F "\[ \]\\|x" '{print $4}')

change\_width=$(( width \- new\_width ))

print "changewidth: $change\_width"
```

**Step 2 — Apply the correct crop**

If the width shrank by more than 20 pixels, there is no dialog box and the original file is copied straight to the output directory. Otherwise, one of two `convert` commands is applied depending on how much the width changed:

```
\# For files with no dialogue box

if \[\[ change\_width \-ge 20 \]\]; then

    cp ${directory}$ifile ${output\_directory}$ifileout

else

    \# for weird files that it doesn't work for (vdc and vapp)

    if \[\[ change\_width \-le 1 \]\]; then

        convert "$directory${ifile}" \-crop "${new\_width}x${new\_height}+0+0" \+repage \\

        \-bordercolor gray75 \-compose copy \-border 3 \\

        \-bordercolor gray58 \-compose copy \-border 3 \\

        \-fuzz 50% \-trim \+repage \\

        \-fuzz 50% \-trim \+repage "$output\_directory${ifileout}"

    else

        convert "${directory}${ifile}" \\

        \-bordercolor gray75 \-compose copy \-border 3 \\

        \-bordercolor gray58 \-compose copy \-border 3 \\

        \-fuzz 50% \-trim \+repage \\

        \-fuzz 50% \-trim \+repage "${output\_directory}${ifileout}"

    fi

fi
```

The `trim` command works by sampling the corner pixels of the image, blurring any pixels within 50% of that colour (via `-fuzz`), and trimming the blurred area — run twice for a cleaner result.

The earlier Shutter-based workflow used a simpler three-step command that cropped the browser chrome, then trimmed the dark background twice. It required a grey browser border to be present and broke when the browser was in full-screen mode:

```
convert "${FILE}" \-crop "${WIDTH}"x"${BCROP}"+0+62 \+repage \\

\-fuzz 50% \-trim \+repage \\

\-fuzz 50% \-trim \+repage "${FILE}"
```

When Shutter was still available, the Autocrop plugin made batch processing straightforward — selecting images in the Session tab and running the plugin across all of them at once. Its base command was simply:

```convert "${FILE}" \-trim \+repage "${FILE}"```

The new script was modelled on that plugin. Creating a custom Shutter plugin was also simple: copy the existing plugin, modify it, and add an SVG logo created in Inkscape to identify it in the UI. See [this ask Ubuntu post](http://askubuntu.com/questions/329696/screenshot-tool-with-integrated-optipng-or-pngcrush) for a worked example and tips on optimising screenshots.

## Result

The shell script successfully automated the cropping of the majority of Abiquo 5.0 UI screenshots, handling both dialog box and non-dialog-box screens in a single pass. The process saved many hours of manual screenshot editing per release, making major release documentation significantly faster and easier to produce.  
