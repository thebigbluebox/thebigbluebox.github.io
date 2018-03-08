---
title:  "Blob to the download"
date:   2018-03-07 9:00:00
description: At work trying to download a file with a post request, but I was stumped and here is my solution. 
---
# How do you [post] a download
## Understanding the fine line between lazy and working

#### The problem
At work I'm deciding between submitting a form to download a file with minimum interaction and allowing extensibility through keeping as much of the needed requirements for submission the same. The problem here is that we have a filter form where by users filter a certain selection of records, and this filter form is already backed by a value object on the server side. Even when writing the end point I was wondering <insert thinking emoji> if using a post request is the correct end point for this especially when I need to have the filter's data in the body of the request. As Wikipedia has generously provided the difference between POST and GET requests below:

> GET Requests a representation of the specified resource. Requests using GET should only retrieve data and should have no other effect. (This is also true of some other HTTP methods.)[1] The W3C has published guidance principles on this distinction, saying, "Web application design should be informed by the above principles, but also by the relevant limitations."[11] See safe methods below.
POST Requests that the server accept the entity enclosed in the request as a new subordinate of the web resource identified by the URI. The data POSTed might be, as examples, an annotation for existing resources; a message for a bulletin board, newsgroup, mailing list, or comment thread; a block of data that is the result of submitting a web form to a data-handling process; or an item to add to a database.[12]

Since I require the data to remain essentially hidden in the body of the request due to the sensitive nature of the filter itself, I sided with the POST request. But how do I download a document from a post request in an async way without interfering with the user's experience?

The form in question:
```html
<form method="POST" action="/searchFile.xhtml" id="fileFilter" id="searchForm">
    <div>
        <input type="datetime" value="Start Date" name="startDate" />
        <input type="datetime" value="End Date" name="endDate" />
        <input type="submit" value="Search" />
    </div>
</form>
```

### First try, using a blob
My first iteration of this was more in line of using a blob to asynchronously download the file like below.

```javascript
function exportList(fileType){
    var xhr = new XMLHttpRequest();
    xhr.open('POST', "/download?fileType=" + fileType);
    xhr.responseType = 'blob';
    var formData = $("#searchForm").serialize();
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xhr.send(formData);
    xhr.onload = function(e) {
        if (this.status == 200) {
            var blob = {};
            var a = document.createElement("a");
            a.style = "display: none";
            document.body.appendChild(a);
            var url = "";
            var dateTimeNow = new Date();
            var fileName = 'theFile_' + dateTimeNow;
            switch(fileType){
                case "CSV":
                    blob = new Blob([this.response], {type: 'text/csv'});
                    fileName = fileName + ".csv";
                    break;
                case "XLSX":
                    fileName = fileName + ".xlsx";
                    blob = new Blob([this.response], {type: 'application/vnd.ms-excel'});
                    break;
            }
            if(window.navigator && window.navigator.msSaveOrOpenBlob) {
                window.navigator.msSaveOrOpenBlob(blob, fileName);
            } else {
                url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = fileName;
                a.click();}
            window.URL.revokeObjectURL(url);
        }
    }
```

It works, real great, but then I discover on iOS and Android systems the blob file downloaded does not execute the file automatically, and also the file type does not show up correctly. Which this problem can be seen here in this [Github issue](https://github.com/readium/readium-js/issues/72) but the fix as seen here in [StackOverFlow](https://stackoverflow.com/questions/39375076/how-to-display-pdf-blob-on-ios-sent-from-my-angularjs-app). This was way more JavaScript than I am committed on maintaining for just a simple web application. So instead I just resolved to the following snippet of code to replace what I had initially wrote.

```javascript
function exportList(fileType) {
    if($("#fileDownload").length == 0){
        var newForm = $("form#searchForm").clone().prop("id","fileDownload").attr("style","display:none").insertAfter("#searchForm");
    }
    $("#fileDownload").prop("action","/download?fileType=" + fileType);
    $("#fileDownload").prop("target","_blank");
    $("#fileDownload").submit();
    $("#fileDownload").remove();
}
```

This is the something where I realized I could clone the filter form and then change the directed url, and then submit it directly via JQuery. This reduces the lines of code to get this done by 80%. But I'm sure I could've done a lot more to make an easier way to download this file, but this probably the simplest solution I could have had for this simple procedure. This also allows the server to set the content deposition for the device to read instead of doing it via the async method above, and added bonus this fixed the issue with iOS and Android browsers not being able to load the files.
