# MacOS Photo Scripts

A growing collection of scripts meant to improve photo workflows on the mac and shine dim light on the mysteries of *Photos.app.* 

Further explanation, discussion, and excuses offered in the blog posts associated with each tool and linked below.

### photos2spotlight.py

Recent versions of *Photos.app* classify pictures into 1,000+ categories (4000+ if you include synonyms). photos2spotlight.py duplicates these category tags in filesystem metadata, where *Spotlight* can index them.

  * [Photo Metadata and Search on MacOS](https://28mm.github.io/notes/osx-photo-search)

### alternative-classifiers.py

Builds a 2nd photos database with tags from 3rd-party image classifiers as well as the *Photos.app* database. Intended to facilitate easy comparisons of image classification services, and expanded xattr tagging (for spotlight search).

  * [Better Photo Search for MacOS with Clarifai](https://28mm.github.io/notes/better-photo-search-for-macos)

### NOTE: Stopping Photolibraryd

In a perfect world, you could reliably stop `photolibraryd` with `launchctl`, but this is hit or miss in my experience.

````bash
[...]$ launchctl stop photolibraryd
[...]$ echo $?
3
````

And you'd be able to unload the photolibraryd `service` without disabling `SIP`.

````bash
[...]# launchctl unload  /System/Library/LaunchAgents/com.apple.photolibraryd.plist
/System/Library/LaunchAgents/com.apple.photolibraryd.plist: Operation not permitted while System Integrity Protection is engage
````

But you cant't. Here is a dubious method for stopping `photolibraryd`, which is at least effective. Assumes `$photodb` is set to your `photos.db`

````
[...]$ chmod -w "$photodb"
[...]$ lsof "$photodb" | 
  awk '{ print $2}'    | 
  egrep -v PID         | 
  xargs kill
````

(Don't forget to restore write permissions when you're done!)




