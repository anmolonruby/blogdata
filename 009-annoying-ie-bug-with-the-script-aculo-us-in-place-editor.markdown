--- 
:format: :markdown
:title: Annoying IE bug with the script.aculo.us in-place editor
:published_on: Mon Jul 03 13:34:00 UTC 2006
---
I came across a rather annoying bug with the [script.aculo.us](http://script.aculou.us) effects library the other day. If you use pass in a loadTextUrl parameter to make the inline editor grab the value from the server, Internet Explorer would throw up the error *'Can't move focus to the control because it's invisible not enabled or of a type that does not accept focus.'*.

After digging around in the inplace-editor code, it seems that it was disabling the inplace form field while it sent off an AJAX request to the server to grab the value, then re-enabling it once it the data was returned. The problem seemed to be down to the difference in the way IE and other browsers place focus on the field once it was inserted. Safari and Firefox were both placing focus on the field before it was disabled, whilst IE was trying to focus it after it had been disabled. This is what was causing IE to barf.

The solution? Instead of simply calling the function that retrieves the remote text from the server (which does both the disabling/reenabling and AJAX request) directly, attach it to the field's onfocus event handler. This ensures that it will always get executed *after* the field has focus.

I've submitted a [patch](http://dev.rubyonrails.org/ticket/5557) for this which fixes this problem which will hopefully be accepted, so if you've ever had problems with this in IE, this should help you out.