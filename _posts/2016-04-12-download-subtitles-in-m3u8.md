---
title: 'Download subtitles/captions from sites using .m3u8'
date: 2016-12-12
permalink: /posts/2016/04/download-subtitles-in-m3u8/
tags:
  - C++
  - Code
published: true
excerpt: "Recently, I have not been able to download subtitle for some of my favourite sites. Then I discovered that they had changed to .m3u8 format in order to support different platforms."
---
Recently, I have not been able to download subtitle for some of my favourite sites. Then I discovered that they had changed to [.m3u8](https://en.wikipedia.org/wiki/M3U) format in order to support different platforms.

You can save the following script in a bash file and run it with the following syntax:
{% highlight shell %}
download_captions.sh <link to captions> <caption file>
{% endhighlight %}


That's all

Here is the source code:

{% highlight shell %}
#!/bin/bash
#Intialization variables
mfile="master.m3u8"
ext=".vtt"
extension=".vtt";

link="$1"
mlen=${#link}
printf '%s mlen=%s\n' "$link" "$mlen"
mlink=${link:0:(mlen - 10)}
printf '%s\n' "$mlink"

#download master file
curl $1 -o "$mfile"

if [ "$extension" == "$ext" ]; then
printf '%s extension=%s\n' "$line" "$extension"
fi

while IFS= read -r line
do
  # download temporary files and join them together
  len=${#line}
  extension=${line:(len-4)} #.vtt
if [ "$extension" == "$ext" ];
then
  curl $mlink$line -o "$line"
  tail -q -n +3 "$line" &gt;&gt; "$2"
  printf '%s extension=%s\n' "$line" "$extension"
  rm $line
fi

done &lt;"$mfile"

printf 'Done\n'
rm $mfile
{% endhighlight %}