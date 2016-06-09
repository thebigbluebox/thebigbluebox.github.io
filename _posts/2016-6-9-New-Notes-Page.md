---
title:  "New Notes Page!"
date:   2016-1-28 9:00:00
description: A place for all my notes that aren't professional
---
# New Notes Page

For the people who look at my blog even, there is now a notes page that feature and document all the different things that I've learnt.
Since nowadays technology seems to be a never ending trail of developers churning out new frameworks and cool gizmos everyday, its hard to keep track of what you've already learnt and what you haven't.
So opening up my notes page is both a place for all the strangers and me to see.

And it was somewhat of a headache making a new notes page, so here is how you do it in Jekyll

* Create the page you want by placing it in the root directory, for me...note.md or note.html they all get rendered back to Html
* Within note.md or note.html, you'll again place that lovel YAML Font Matter header that will describe what the page is about

```
---
title: Notes
permalink: notes/
profile: false
---
```

* After that we go into your _config.yml to actually setup a few things, which are your collections (the new collection of things other than posts)


```
# create a new collection by adding the parameter collections
collections:
    notes:
    # output gives you the same feel as a post like page where each article is rerendered as a individual page
        output: true
        
# now we also add to our defaults options to theme and get the pages have a type covered
defaults:
     - 
        scope:
            path: ""
            type: notes
        values:
            layout: post
```

* After that create a folder with an underscore infront of the name directory '_' and the name should be like "/_notes" using the same name as your collection
* Then just start to create new posts in the folders with HTML or MD, all gets compiled to their own pages because you set the "output" parameter to true