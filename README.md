Large file uploads with progress reporting in Silverlight
=========================================================

Are you a web developer? Are you trying to support uploading huge files on all browsers? Are you trying to support displaying files locally?

Then you should use the [HTML5 File API](http://www.w3.org/TR/FileAPI/). That's the future.

You already know that? You've already used it? Great. Now, you've surely uncovered a problem from the past. And that's where this code comes in.

Old Internet Explorer
---------------------

Old Internet Explorer versions (IE9 and below) don't support the HTML5 File API. You'd like a backup system, but there are problems:

* Regular form submission (including iframe hacks): you can't track upload progress; you can't read files locally.
* Flash: you can't handle huge files. (Flash seeks by reading the entire file into memory; its memory limit is about 200MB.)
* Java: because of past security problems, not enough of your users have enabled it.

Silverlight is the obvious choice: a good portion of your IE9 users have it installed, and it doesn't have Flash's limitation.

This library
------------

You can use this library as a shim. You'll need very few modifications to your actual code.

This library supports File, Blob, FileReader and a made-up one, XMLHttpUploadRequest. (Sorry, it doesn't modify the existing XMLHttpRequest.) FileReader can decode UTF-8, UTF-16 and Windows-1252.

Where are the docs?
-------------------

Unfortunately, I'm releasing this library because I'm in the process of deleting it from its old home, [The Overview Project](https://www.overviewproject.org). We stopped supporting IE9, so we aren't maintaining it any more.

And that means no docs.

But let me very quickly give the broad strokes of how we used this library:

* We let our form's JavaScript replace jQuery's `xhr_factory` so [we can use our Silverlight object instead](https://github.com/overview/overview-server/blob/ded4ce098f1c537f936f4e4ed139541a9b7700a4/app/assets/javascripts/util/net/upload.coffee#L118)
* In our form's JavaScript, we [load file-upload.xap and specify our xhr_factory callback](https://github.com/overview/overview-server/blob/ded4ce098f1c537f936f4e4ed139541a9b7700a4/app/assets/javascripts/for-view/CsvUpload/new.coffee#L335)
* In our form's JavaScript, we [replace the file input with a Silverlight control](https://github.com/overview/overview-server/blob/ded4ce098f1c537f936f4e4ed139541a9b7700a4/app/assets/javascripts/for-view/CsvUpload/new.coffee#L358)

You'll have to learn a lot of context to get this code working. But at least you won't have to re-code everything.

Where is the code?
------------------

* silverlight-upload-request.js: the JavaScript side of the shim
* cs-project/FileUpload.xap: the compiled C# code. (use build.bat to compile it. You need .NET but not Visual Studio.)
* vendor/Silverlight.debug.js: Microsoft's Silverlight loader code. This is what loads FileUpload.xap.

License
-------

This software is Copyright 2011-2014 The Associated Press, and distributed under the terms of the GNU Affero General Public License. See the LICENSE file for details.
