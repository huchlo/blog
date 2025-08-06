渐进式Web应用（Progressive Web App，_PWA_）是一个使用web平台技术构建的应用程序，但它提供的用户体验就像一个特定平台的应用程序。


## manifest.json
主要：head中添加manifest.json，网页就可以安装到桌面，一般是chrome内核浏览器，但应用还是需要有网络请求

```html
<head>
  <link rel="manifest" href="manifest.json">
</head>
```

```json
{
  "short_name": "Super",
  "name": "Super",
  "description": "Super",
  "start_url": "/h5/index.html",
  "display": "standalone",
  "background_color": "#000000",
  "theme_color": "#181818",
  "orientation": "portrait",
  "icons": [
    {
      "src": "icon.png",
      "sizes": "144x144",
      "type": "image/png"
    }
  ]
}
```

手动安装

```js
window.addEventListener('appinstalled', (evt) => {
  console.log('appinstalled')
});

function install() {
      deferredPrompt.prompt();
      deferredPrompt.userChoice.then((choiceResult) => {
          if (choiceResult.outcome === 'accepted') {
            console.log('User accepted the A2HS prompt');
          } else {
            console.log('User declined the A2HS prompt');
          }
          deferredPrompt = null;
        });

    }
```

## serviceWorker
注册serviceWorker了，就可以把网络请求储存到本地

```js
if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        console.log("page load")
        navigator.serviceWorker.register('sw.js').then(function (registration) {
        }).catch(function (error) {
            console.error('Service Worker 注册失败：', error);
        });
    })
}
```

```js
//sw.js
const resources_js = ['/h5/src/settings.2aa2a.js']
const resources_json = ['/h5/assets/NewInvite/config.0b381.json']
const resources_png = ['/h5/assets/resources/native/11/111174a00.c9e97.png']
const resources_png2 = ['/h5/assets/8465-9cae0b5fe181.8e55b.png']
const resources_jpg = ['/3b6-1157d3e67ff3.3b709.jpg]
const resources_ogg = ['/h5/assets/5-9ee0e2a33631.8f7d5.ogg']
const resources_ttf = ['/h5/assets/110bc/NeuronExtraBold.ttf']

self.addEventListener("install", e => {
    console.log('install sw')
    e.waitUntil(
                    caches.open("resources_js").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_js);
                            }else{
                                for(let item of keys){
                                    if(resources_js.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    }),
                    caches.open("resources_json").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_json);
                            }else{
                                for(let item of keys){
                                    if(resources_json.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    }),
                    caches.open("resources_png").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_png);
                            }else{
                                for(let item of keys){
                                    if(resources_png.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    }),
                    caches.open("resources_png2").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_png2);
                            }else{
                                for(let item of keys){
                               if(resources_png2.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    }),
                    caches.open("resources_ogg").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_ogg);
                            }else{
                                for(let item of keys){
                                    if(resources_ogg.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    }),
                    caches.open("resources_jpg").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_jpg);
                            }else{
                                for(let item of keys){
                                    if(resources_jpg.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    }),
                    caches.open("resources_ttf").then(cache => {
                        cache.keys().then((keys)=>{
                            if(keys.length === 0){
                                cache.addAll(resources_ttf);
                            }else{
                                for(let item of keys){
                                    if(resources_ttf.includes(item.url.substring(item.url.indexOf('/h5')))){
                                        //复用的文件
                                        continue
                                    }else{
                                        //过时的文件
                                        cache.delete(item)
                                    }
                                }
                            }
                        })
                    })
    );
    self.skipWaiting()
});

  

/**
 * 激活 Service Worker
 */
self.addEventListener("activate", (e) => {
    console.log('activate sw')
});

  

self.addEventListener("fetch", event => {
    // console.log('service worker fetch', event.request.url);
    const url = event.request.url
    const a = new Date().getTime()
    if (url.indexOf('/h5/') > 0) {
        const filename = url.split('/').pop();
        const lastDotIndex = filename.lastIndexOf('.');
        if (lastDotIndex > 0 && lastDotIndex < filename.length - 1) {
            const suffix = filename.substring(lastDotIndex + 1);
            let _tmp = null
            switch (suffix) {
                case "png":
                    if (resources_png.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_png"
                    } else if (resources_png2.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_png2"
                    }
                    break;
                case "ogg":
                    if (resources_ogg.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_ogg"
                    }
                    break;
                case "manifest":
                    if (resources_ogg.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_ogg"
                    }
                    break;
                case "jpg":
                    if (resources_jpg.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_jpg"
                    }
                    break;
                case "js":
                    if (resources_js.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_js"
                    }
                    break;
                case "json":
                    if (resources_json.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_json"
                    }
                    break;
                case "ttf":
                    if (resources_ttf.includes(url.substring(url.indexOf('/h5')))) {
                        _tmp = "resources_ttf"
                    }
                    break;
            }
            if (_tmp) {
                event.respondWith(
                    caches
                        .open(_tmp)
                        .then(cache => {
                            return cache.match(event.request, { ignoreSearch: true }).then(response => {
                                if (response) {
                                    return response
                                } else {
                                    // console.log(event.request.url, "没有在缓存中找到")
                                    return fetch(event.request);
                                }
                            })
                        })
                );
            } else {
                try {
                    event.respondWith(fetch(event.request));
                } catch (e) {
                    console.warn(e)
                }
            }
        } else {
            try {
                event.respondWith(fetch(event.request));
            } catch (e) {
                console.warn(e)
            }
        }
    }
    else {
        try {
            event.respondWith(fetch(event.request));
        } catch (e) {
            console.warn(e)
        }
    }
});

```