# 18_PWA_Online-Offline_Budget_Trackers

-Compress using jscompress.com

# Solution Scope

Reading the article on [Progress-Web-Apps](https://medium.com/james-johnson/a-simple-progressive-web-app-tutorial-f9708e5f2605)

There are 3 key elements we needed to do to make it a PWA:

### 1) Add to Index Header
First the header has to be setup on the index.html page:
```
  <link rel="manifest" href="/manifest.json">
  <link rel="icon" href="favicon.ico" type="image/x-icon" />  
  <link rel="apple-touch-icon" href="assets/pwa-icon.png">   
  <meta name="theme-color" content="white"/>  
  <meta name="apple-mobile-web-app-capable" content="yes">  
  <meta name="apple-mobile-web-app-status-bar-style" content="black"> 
  <meta name="apple-mobile-web-app-title" content="Picturegram"> 
  <meta name="msapplication-TileImage" content="assets/pwa-icon.png">  
  <meta name="msapplication-TileColor" content="#FFFFFF"> 
```

### 2) Add a Manifest
A *manifest.json* file that itemizes the files the browser must to cache must be setup.

### 3) Add a Service Worker
Third, a serviceworker.js needs to be registered and have some boiler-plate code to cache the various dependency files, and also the key api calls (GET requests). This code we can copy, and adjust the files to cache.

Once that is done, it will load ok and work offline.

### 4) Disable Features Unusable in Offline Mode
There is one slight problem we still should address -- any functionality that does not work in offline mode should be disabled.

First, we add a stylesheet class that when added to an element disables clicking on it and also slightly dims the element:
```
.appear-offline {
    pointer-events: none;
    opacity: 0.7;
}
```

We want this class to be added to any DOM elements when the device is offline. To achieve this, we mark each of those senstive DOM elements with another class 'online-only'.

Then we add this:
```
function updateOnlineStatus(event){
    const isOnline = navigator.onLine==true || navigator.onLine==='online'
    const el = document.querySelectorAll('.online-only');
    for( let i=0; i<el.length; i++ ){
        if( isOnline ){
            el[i].classList.remove("appear-offline");
        } else {
            el[i].classList.add("appear-offline");
        }
    }
}

window.onload = function(){
    window.addEventListener('online',  updateOnlineStatus);
    window.addEventListener('offline', updateOnlineStatus);
    updateOnlineStatus();
}
```

### 5) Payload/Media Optimization
For mobile sites, special attention to the payload size should be checked. Any media that is not optimized in size should be shrunk down. Javascript should be minified.

Any media uploaded/stored by the site should be shrunk and optimized to deliver the right size. (we do this with the imageTool.resize).

Finally images should be lazy-loaded so that possibly many images that are never looked at, should not be loaded. Nowadays we do this by adding a *loading="lazy"* attribute to images. We do this on scripts.js line 243. [Lazy Loading / Eager Loading](https://www.inmotionhosting.com/blog/what-is-lazy-loading-lazy-loading-vs-eager-loading/)
