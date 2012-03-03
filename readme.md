Twilicious
=========

Here are the steps I took to grab my tweets, parse them for the URLS embedded, and build a Bookmarks standard compliant HTML file so I could import the links into delicious.

##Step 1

* Use Harold's  [Twitter Archive](https://github.com/hrldcpr/twitter-archive). This puts all your tweets as individual JSON files on your machine.

##Step 2

* I used Unix tools like SED GREP and CURL to grab the "text:" parse out just the http and put it into a file. CURL helped find the original expanded URL bypassing the URL shorteners.

```
sed 's/.*"text"://' * | sed 's/"created_at":.*//' | sed 's/.*http/http/' | grep http | sed 's/ .*//' | sed 's/",//' >> mytweets.txt

```

##Step 3

* Saved the urls in a .js file and made it an array by just added var urls = [];

##Step 4

* Using a javascript For Loop I started preparing the urls to have the right DOM element structure. I added the strict Bookmarks document standard as a string to the url elements and manually saved this out as an HTML. I them imported this into Delicious.

```
<script src="urls.js"></script>
<script>
    //create a div
    var div = document.createElement('div');
    var i;
    //loop through the length of urls and attach dom elements <dt> <a href>
    for (i = 0; i <= urls.length; i++)
        {
            var link = document.createElement('a');
            link.setAttribute('href', urls[i]);
            var wrapper = document.createElement('dt'); 
            wrapper.appendChild(link);
            div.appendChild(wrapper);
        }
    //attach the Delicious Doctype as a string to div.innerHTML
    var string = '<!DOCTYPE NETSCAPE-Bookmark-file-1>\n<!--This is an automatically generated file.\nIt will be read and overwritten.\nDo Not Edit! -->\n<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">\n<Title>Bookmarks</Title>\n<H1>Bookmarks</H1>'

    console.log(string + div.innerHTML);
    //copy what we get from Console log, stick it into a new HTML file and Upload to delicious.
```