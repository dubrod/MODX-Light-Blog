MODX-Light-Blog
===============

Light Blog Structure for MODX Revo using the new Tagger and Collections Extras


> Have your Category and Single Page Templates Ready

> Install GetResources, Collections and Tagger via Package Management

> Change "Content Types" .html => /

##Tagger Setup

1. Create a New Tagger Group (*Components -> Tagger*) - our example is "**Category**"

    **Options:**  

    - Field Type = Combo box
    - Show for Templates = 1, (*your single page template*)

![Tagger Groups](https://dl.dropboxusercontent.com/u/4277345/MODX/Light-Blog/tagger-groups.jpg)

2. Create a New Tag in "Category" our example is "**Category One**"

![Tagger Tags](https://dl.dropboxusercontent.com/u/4277345/MODX/Light-Blog/tagger-tags.jpg)

3. Create a Collection in the root (*right click web context*) - our example is "**Blog**"

![Create Collection](https://dl.dropboxusercontent.com/u/4277345/MODX/Light-Blog/create-collection.jpg)

  - Set your collection to your "Category Template"
  - Add a child page - set the template to your "Single Page Template"
  - Select the Category for the child page via the Tagger Tab

![Add Category](https://dl.dropboxusercontent.com/u/4277345/MODX/Light-Blog/tagger-resource-tab.jpg)

--- 

##Code

###Sidebar Category List

> **Note:** Calling the Group 1 (Category) and Linking to resource 3 (Category Placeholder)

We are using a Category Placeholder resource and not linking back to our blog collections resource simply so we can have the URL structure of *category.html?tags=Category+One* so Google will see the index without variables. You can set the target to the collection and it will work. 

![Resource Tree](https://dl.dropboxusercontent.com/u/4277345/MODX/Light-Blog/resource-tree.jpg)

```
[[TaggerGetTags? &groups=`1` &rowTpl=`tag_categories_tpl` &target=`3`]]
```
 
**tag\_categories\_tpl** = `<li><a href="[[+uri]]">[[+tag]]</a> ([[+cnt]])</li>`


###Category Page Template

####Snippet to grab Tag from URL

Put `[[cateHeading]]` on line 1 to set our "tagName" placeholder

```
$get = modX::sanitize($_GET, $modx->sanitizePatterns);
$modx->setPlaceholder('tagName', urldecode($get['tags']));
```
  
####Dynamic Heading

> Set a Property Type for the Default Heading in the chunk - our example is **ph.heading**

![Set Property Type](https://dl.dropboxusercontent.com/u/4277345/MODX/Light-Blog/chunk-banner-properties.jpg)

```
[[$banner?[[+tagName:notempty=`&ph.heading=`[[+tagName]]``]]]]
```

> The chunk "banner" should holder your additional title markup and the placeholder

`<h1>[[+ph.heading]]</h1>`

####Dynamic Page Meta Title

> Repeat steps from banner, assuming you have a chunk for **&lt;meta&gt;**

```
[[$meta?[[+tagName:notempty=`&ph.title=`[[+tagName]]``]]]]
```

Your title is now: `<title>[[+ph.title]]</title>`


####Content Area (Post Listing)

> **Note:** Calling the Resource 2 (Our Collection) and sorting by publish date

```
[[getResources? &where=`[[TaggerGetResourcesWhere]]` &parents=`2` &tpl=`gr_posts_tpl` &sortby=`publishedon` &sortdir=`DESC`]]
```

**gr\_posts\_tpl** = your typical getResources listing chunk
