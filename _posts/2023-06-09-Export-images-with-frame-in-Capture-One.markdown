---
layout: post
title: Automatically export images with a frame in Capture One
date: 2023-06-09
excerpt: "A handy trick if you want to avoid Instagram cropping your images to a square." 
---

# Introduction

Adding a white or black frame to a picture valorises the picture's composition and appearance. 

A white frame can also be a convenient trick to post photos in a 2:3 ratio on Instagram without it cropping them to fulfil its square format, like [in this example][my-insta]. More on that [later](#how-to-avoid-the-instagram-crop).

# Software presequisites

You need [Capture One][capture-one]. 
You need to install the [ImageMagick][imagemagick] software suite.
I recommend to install the latter using [Homebrew][homebrew], with this simple command:
```
brew install imagemagick
```
More on this [here][homebrew-imagemagick].
If you are on a Mac, you can use the powerful [Automator][automator]. If you are on Windows or Linux, you will need to search for a way to render the script that we will write [later](#imagemagic-script) executable. Herewith, I will refer to Mac instructions only, but it should be straightforward to adapt to the other OS systems.

# ImageMagick script

[mogrify][mog] is the software within the [ImageMagick][imagemagick] software suite that we will use to create your frame. It will modify the input image file and automatically replace it with the modified version. 

Should you want to keep the original file, use [convert][convert] instead. The commands and the logic of the automation are substantially the same. 

[mogrify][mog] can do much more than add a frame. Unleash your creativity!

We will use [mogrify][mog] to create a continuous white frame that is 4% of the image's x-side and a white frame to fill the y-axis of landscape images to make them appear square to Instagram. 

First, you need to know where is installed [mogrify][mog] with the command 
```
which mogrify
```
This command should output something like `/opt/homebrew/bin/mogrify` if you installed mogrify with [Homebrew][homebrew]. If you installed the [ImageMagick][imagemagick] otherwise, still find the path to its installation.
You will need this path to call mogrify in the [Automator][automator], otherwise, it might not find it. 

## Continous frame 4% wide

You will need the command:
```
/opt/homebrew/bin/mogrify -bordercolor white -border 5x% $1
```
If you try this script in your shell, replace `$1` with your filename. In Windows, you shall use `%1`.

## Fill Instagram square format
```
/opt/homebrew/bin/mogrify -bordercolor white -border x180 $1
```
The border size rationale will be evident in the set-up of the Export Recipes in [Capture One][caputer-one].

# Automator app

In [Automator][automator], select `File>New` and then choose _Application_.
In the `Library`, under `Files...olders`, choose (double click or drag) `Get Selected Finder Items`.
Then, again in the `Library`, under `Utilities`, choose `Run Shell Script`.
Replace `cat` with one of the [mogrify][mog] scripts above. 
In _Pass input_ in the top right of the `Run Shell Script` window, choose _as arguments_.
Your app should look like this:

![The Automator App](/assets/images/automator-app.png)

Save the app by editing its name (`Untiteled`) in the image and choose its destination folder.
You can now test the app by dragging and dropping an image in it.

# Capture One Export Recipes

You can now create your Export Recipes for each border type and image format you wish. Each recipe will use the specific app you created with the Automator to create a border with [mogrify][mog].
Choose your app in the tab `Open with` under `Format & Size`.

# How to avoid the Instagram crop 

We can now create an Export Recipe that transforms your landscape format pictures into square pictures.

In Capture One, create an Export Recipe that:
- Uses as ICC Profile `sRGB Color Space Profile`.
- Has its width set to 1080px. A 2:3 ratio landscape picture will have a 720px height. You need 360px of the white border to make it a 1080px-1080px image. That is why in the [mogrify][mog] recipe [above](# fill-instagram-square-format), you create 180px heigh horizontal borders.
- Open the output file with your [Automator app](#automator-app).

Your Export Recipe should look like that:

![The Capture One Export Recipe](/assets/images/caputre-one-recipe.png)

The output image should look like that:

<img src="/assets/images/grandmas-fight.jpg" alt="Grandmas' fight. Sardinia, Italy" style="border: 2px solid  gray;">


[my-insta]: https://www.instagram.com/myfisheye.pictures/
[capture-one]: https://www.captureone.com/en
[imagemagick]: https://imagemagick.org 
[homebrew]: https://brew.sh
[homebrew-imagemagick]: https://formulae.brew.sh/formula/imagemagick#default
[mog]: https://imagemagick.org/script/mogrify.php
[convert]: https://imagemagick.org/script/convert.php
[automator]: https://macosxautomation.com/automator/
[mog-border]: https://imagemagick.org/script/command-line-options.php#blur
