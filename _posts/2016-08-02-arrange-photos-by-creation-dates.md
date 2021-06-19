---
title: 'Arrange photos by their creation dates'
date: 2016-08-02
permalink: /posts/2016/08/arrange-photos-by-creation-dates/
tags:
  - Photo
  - bash
published: true
excerpt: "The goal is that photos would be arranged programmatically into different directories, preferably starting with `Year` as root directories and a list of `YYYY-MM-DD` directories. That would make life easier"
---
I have to confess that I do not like taking photo of my own or others. However, I do posses a big amount of photo in my computer. After every trip that I traveled, it had taken months before I managed to pull out all of photo taken from my IPhone, my wife's, our Canon camera and labeled (re-arranged) them into corresponding folders. The folders name must ring a bell when we look back later. The goal is that those photo would be arranged programmatically into different directories, preferably starting with `Year` as root directories and a list of `YYYY-MM-DD` directories. That would make life easier.

Example:
```
2015
  2015-01-31
    Img1.jpeg
    Img2.jpeg
  2015-02-16
    Img1.jpeg
    Img2.jpeg
2016
  2016-01-31
    Img1.jpeg
    Img2.jpeg
  2016-02-16
    Img1.jpeg
    Img2.jpeg
```

These dates are computed from photo's creation dates. Therefore, this approach heavily relies on the assumption that all device's dates are correctly set.

The first I had to do is finding the creation date of a given file. That is achieved by utility `stat`.


[sourcecode language="bash"]
stat -f %Sm -t %F Img1.jpeg
[/sourcecode]


According to the manual of 'stat', option 'm' means the modified date of the file while 'B' means the birth date of the file. However, when I tested 'B' did not deliver what it was supposed to. I had to reply on an assumption that the file was untouched since it was copied from memory card ( of my camera).

The next challenge is looping through all jpeg files in the current directory and possibly looking into subdirectories. It was achieved by combination of 'for' loop and 'find'.

{% highlight shell %}
for thisfile in $(find . -maxdepth 1 -name &amp;quot;$1&amp;quot; \! \( -name "*._* \) -type f | xargs -0 ); do
    echo "Moving..." $thisfile;
    moving $thisfile;
    done
{% endhighlight %}

However, as expected, when filenames containing space, the result of `find` will return broken filenames. For example, if there are 3 files `a.jpeg`, `b.jpeg`, and `c d.jpeg`. The `find` utility will return the following strings `./a.jpeg`, `./b.jpeg`, `./c` , and lastly `d.jpeg`.
The reason is that the special variable $IFS determines how string should be treated. By default, $IFS is or something like that. Other utilities that deal with strings containing sensitive characters '\0', ' ', 'b' (break), etc, will reply on the value of $IFS to manipulate the string. Therefore, I needed to make sure $IFS not containing something I cannot digest. In this case, it is the `space`. The strategy is switching on/off the value of $IFS.


{% highlight shell %}
SAVEIFS=$IFS
IFS=$(echo -en "\n\b")
--&gt; do some good thing with names
IFS=$SAVEIFS
{% endhighlight %}


Altogether, here is the final script.

{% highlight shell %}
#!/bin/bash
#Author: Thanh Truong, Sweden, July 2016
#Reference: https://gist.github.com/pietrop/880a58088c630c960166
rootdir="/Users/thanhtruong/repos/projects/github/temp"

function moving() {
    img="$1"
    # B means birth of the file
    #creation_date="$(stat -f %SB -t %F $img)"
    # m means modified time of the file
    creation_date="$(stat -f %Sm -t %F $img)"

    imgYearDir=${creation_date:0:4}

    pushd $rootdir
    if [ ! -d "$imgYearDir" ]; then
        mkdir "$imgYearDir"
    fi

    pushd $imgYearDir
    if [ ! -d "$creation_date" ]; then
        mkdir "$creation_date"
    fi
    popd # from imgYearDir
    popd # from rootdir

    imgNameOnly=${img##*/}
    #echo "ImageNameOnly $imgNameOnly"
    $(mv $img $rootdir/$imgYearDir/$creation_date/$imgNameOnly)

    echo "...to $rootdir/$imgYearDir/$creation_date/$imgNameOnly"
 }

function mover()  {
    #The value of IFS are used as token delimiters or separator for each line.
    #By default, IFS is space so processing filenames containing spaces will not correct
    # because the any utility will break a line with spaces into several lines
    SAVEIFS=$IFS
    IFS=$(echo -en "\n\b")

    # Note that, here -print0 is not used. Otherwise, it treats '\0' as 'space'
    for thisfile in $(find . -maxdepth 1 -name "$1" \! \( -name "*._*" \) -type f | xargs -0 ); do
    echo "Moving..." $thisfile;
    moving $thisfile;
    done
    #Restore IFS
    IFS=$SAVEIFS
}

abort()
{
    echo &gt;&amp;2 '
***************
*** ABORTED ***
***************
'
    echo "An error occurred. Exiting..." &gt;&amp;2
    exit 1
}

#----------------------------------------
trap 'abort' 0
set -e
## ===&gt; Guarded script goes here
mover "$1"
# Done!
trap : 0
#----------------------------------------
{% endhighlight %}


# References
- [https://packaging.python.org/tutorials/packaging-projects/](https://packaging.python.org/tutorials/packaging-projects/)
- [https://packaging.python.org/specifications/pypirc/](https://packaging.python.org/specifications/pypirc/)