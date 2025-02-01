# 🚀 WordPress Version Detector Bookmarklet  

A **one-click** bookmarklet that detects the **WordPress version** of any website using multiple methods. Perfect for **security researchers, bug bounty hunters, and penetration testers**.  

## 🔹 Features  
✅ **Multiple detection techniques** for accuracy:  
- Meta tag scanning (`<meta name="generator">`).  
- Query string extraction (`ver=X.X.X` in script URLs).  
- WordPress embed script (`wp-embed.min.js?ver=X.X.X`).  
- Direct version retrieval from `/wp-includes/version.php`.  
- RSS Feed analysis (`/feed/`).  
- REST API lookup (`?rest_route=/`).  

✅ **Automatic fallback** → Tries multiple methods until it finds a version.  
✅ **Source indication** → Shows where it found the version.  
✅ **Fast & lightweight** → No installation, just a bookmark!  

## 🚀 How to Use  
1. **Create a new bookmark** in your browser.  
2. **Copy the bookmarklet code** (see below).  
3. **Paste it into the "URL" field** of the bookmark.  
4. **Visit a WordPress-powered website**.  
5. **Click the bookmark** → See the detected version in an alert!  

## 🔗 Bookmarklet Code  
Copy and paste this into the **URL field** of a new bookmark:  

```javascript
javascript:(function(){function callback(response,source){if(response.ok){response.text().then(function(text){var version=null,match;if(match=text.match(/<meta name="generator" content="WordPress ([\d.]+)"/i)){version=match[1];source="Meta tag";}else if(match=text.match(/ver=([\d.]+)/i)){version=match[1];source="Query string in scripts";}else if(match=text.match(/wp-embed\.min\.js\?ver=([\d.]+)/i)){version=match[1];source="WP embed script";}if(version){alert("WordPress version: "+version+"\n(Source: "+source+")");}else{nextMethod();}});}else{nextMethod();}}function nextMethod(){if(methods.length>0){var next=methods.shift();fetch(next.url).then(response=>callback(response,next.source)).catch(nextMethod);}else{alert("No WordPress version found.");}}var methods=[{url:window.location.origin+"/wp-includes/version.php",source:"Version.php"},{url:window.location.origin+"/feed/",source:"RSS Feed"},{url:window.location.origin+"/?rest_route=/",source:"REST API"}];fetch(window.location.origin).then(response=>callback(response,"Homepage")).catch(nextMethod);})();
