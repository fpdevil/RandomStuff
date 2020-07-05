# Installation of specific version of `OpenCV 4.x.x`

Recently I ran into some issues with the homebrew installed `opencv` as the
latest version from brew is set to use `python 3.8` as it's dependency. Since `3.8`
version is still not the mainstream version of `python3` and also the fact
that I was not prepared to make the move to this version of `python` I was
looking for ways to switch back to the previous version with `python 3.7.7`
dependency.

One way I could do is using the `virtualenv` of `python`, but that will be out
of the default homebrew way. Also, I didn't wanted to spend that much time in
setting up my new `virtualenv` and build `cv` over it.

I got some pretty good information about how to switch to the previous
versions smoothly from an old link posted by [Zoltan Altfatter].

(_thanks to Zoltan_)

## Installation of the previous version.

For installing the previous version of `OpenCV`, I followed the below steps using the link as a reference.

* Browse to the homebrew core as below.

```bash
cd "$(brew --repo homebrew/core)"
```

which on my machine took me to the below path:

`/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core|master`

* Now check the history of the commits to pick the one we are interested in.

```bash
git log master -- Formula/opencv.rb

# or
# git log Formula/opencv.rb

...
commit 9193059621d2af66599074df90524b0ea202b5d8
Author: BrewTestBot <homebrew-test-bot@lists.sfconservancy.org>
Date:   Sun Apr 5 09:20:43 2020 +0000

    opencv: update 4.2.0_4 bottle.

commit 9950e80c1ea4e3e5a3dda296b5900d251535acb1
Author: Alexander Bayandin <a.bayandin@gmail.com>
Date:   Mon Dec 2 19:26:00 2019 +0000

    opencv: revision bump for python@3.8

commit 54deb66bf88b9987e5243c6f8580ff6b75c29607
Author: Mike McQuaid <mike@mikemcquaid.com>
Date:   Thu Mar 12 20:54:57 2020 +0000

    opencv: autofix rubocop Style/IfUnlessModifier.

commit 38a5da9f70ee39258180ce925ce336ed5f448a59
Author: BrewTestBot <homebrew-test-bot@lists.sfconservancy.org>
Date:   Fri Mar 6 03:08:25 2020 +0000

    opencv: update 4.2.0_3 bottle.
...
```

The commit I was interested in is `54deb66bf88b9987e5243c6f8580ff6b75c29607`, which can be checked out as below:

* Do a git checkout of the selected commit branch.

```bash
git checkout -b opencv-4.2.0 54deb66bf88b9987e5243c6f8580ff6b75c29607

Switched to a new branch 'opencv-4.2.0'
```

* Unlink the previous version of `OpenCV`

If we have any previous version(s) of opencv already installed, they need to
be unlinked using `brew unlink opencv`. In my case I already uninstalled it,
so it gave me a warning when I ran the command. Here is the command which can
do the unlinking.

```bash
brew unlink opencv
```

* Now install `opencv` by running the below:

```bash
HOMEBREW_NO_AUTO_UPDATE=1 brew install opencv
```

The option `HOMEBREW_NO_AUTO_UPDATE` will force `homebrew` not to update itself.

The installation should continue normally with this by fetching any required dependencies as needed.

```bash
apple:/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core|opencv-4.2.0
⇒  brew -v unlink opencv
Error: No such keg: /usr/local/Cellar/opencv
apple:/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core|opencv-4.2.0
⇒  HOMEBREW_NO_AUTO_UPDATE=1 brew -v install opencv
==> Downloading https://homebrew.bintray.com/bottles/cmake-3.16.5.catalina.bottle.tar.gz
```

* Below are all the dependencies installed as per the requirement of `opencv-4.2.0`. Some of the dependencies already installed may have to be downgraded as a part of this process. For my setup, below are the dependencies which were downgraded.

```bash
https://homebrew.bintray.com/bottles/gcc-9.2.0_3.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/openblas-0.3.9.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/suite-sparse-5.7.1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/freetype-2.10.1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/libffi-3.2.1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/p11-kit-0.23.20.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/openssl%401.1-1.1.1d.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/sqlite-3.31.1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/python-3.7.7.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/icu4c-64.2.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/harfbuzz-2.6.4.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/little-cms2-2.9.catalina.bottle.1.tar.gz
https://homebrew.bintray.com/bottles/x264-r2917_1.catalina.bottle.1.tar.gz
https://homebrew.bintray.com/bottles/ffmpeg-4.2.2_2.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/numpy-1.18.1_1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/ilmbase-2.4.1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/openexr-2.4.1.catalina.bottle.tar.gz
https://homebrew.bintray.com/bottles/opencv-4.2.0_3.catalina.bottle.tar.gz

==> Installing dependencies for opencv: gcc, openblas, suite-sparse, freetype, libffi, p11-kit, openssl@1.1, sqlite, python, icu4c, harfbuzz, little-cms2, x264, ffmpeg, numpy, ilmbase and openexr
```

* Stop updates using `pin`

Once the installation is completed, we can set the `pin` option to prevent
homebrew from further updating self and also to stop certain formulae from
being updated. This may be done as under.

```bash
brew pin opencv
```

Finally we can check the installed component status which should show the
appropriate version.

```bash
apple:~|⇒  brew info opencv
opencv: stable 4.2.0 (bottled) [pinned at 4.2.0_3]
Open source computer vision library
https://opencv.org/
/usr/local/Cellar/opencv/4.2.0_3 (756 files, 223.2MB) *
  Poured from bottle on 2020-05-18 at 21:54:19
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/opencv.rb
==> Dependencies
Build: cmake ✔, pkg-config ✘
Required: ceres-solver ✔, eigen ✔, ffmpeg ✔, glog ✔, harfbuzz ✔,
jpeg ✔,libpng ✔, libtiff ✔, numpy ✔, openblas ✔, openexr ✔,
protobuf ✔, python ✔, tbb ✔, webp ✔
==> Analytics
install: 16,302 (30 days), 50,281 (90 days), 185,396 (365 days)
install-on-request: 14,943 (30 days), 45,447 (90 days), 167,202 (365 days)
build-error: 0 (30 days)
```

## Check the installed `OpenCV` version from the `python` repl.

```python
Python 3.7.7 (default, Mar 10 2020, 15:43:33)
Type 'copyright', 'credits' or 'license' for more information
IPython 7.14.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import cv2

In [2]: cv2.__version__
Out[2]: '4.2.0'
```

* checking the `pkg-config`

```bash
$ pkg-config --modversion opencv4
4.2.0

$ pkg-config --libs --cflags opencv4 | tr ' ' '\n'
-I/usr/local/Cellar/opencv/4.2.0_3/include/opencv4/opencv
-I/usr/local/Cellar/opencv/4.2.0_3/include/opencv4
-L/usr/local/Cellar/opencv/4.2.0_3/lib
-lopencv_gapi
-lopencv_stitching
-lopencv_aruco
-lopencv_bgsegm
-lopencv_bioinspired
-lopencv_ccalib
-lopencv_dnn_objdetect
-lopencv_dnn_superres
-lopencv_dpm
-lopencv_highgui
-lopencv_face
-lopencv_freetype
-lopencv_fuzzy
-lopencv_hfs
-lopencv_img_hash
-lopencv_line_descriptor
-lopencv_quality
-lopencv_reg
-lopencv_rgbd
-lopencv_saliency
-lopencv_sfm
-lopencv_stereo
-lopencv_structured_light
-lopencv_phase_unwrapping
-lopencv_superres
-lopencv_optflow
-lopencv_surface_matching
-lopencv_tracking
-lopencv_datasets
-lopencv_text
-lopencv_dnn
-lopencv_plot
-lopencv_videostab
-lopencv_videoio
-lopencv_xfeatures2d
-lopencv_shape
-lopencv_ml
-lopencv_ximgproc
-lopencv_video
-lopencv_xobjdetect
-lopencv_objdetect
-lopencv_calib3d
-lopencv_imgcodecs
-lopencv_features2d
-lopencv_flann
-lopencv_xphoto
-lopencv_photo
-lopencv_imgproc
-lopencv_core
```

## Cleanups and further updates

Later, if we really want to clean this up and go for the regular update, we can perform the necessary cleanup process as under.

```bash
git checkout master
git branch -d opencv-4.2.0
brew cleanup && brew prune
```


[Zoltan Altfatter]: https://zoltanaltfatter.com/2017/09/07/Install-a-specific-version-of-formula-with-homebrew/
