# AssetCache Plugin

AssetCache plugin allows you to download a file from the world wide web and download it to the local web folder of your app. 

Then, you will be able to access it from web source or delete it aswell.

## How to use

* import the plugin to your project as explained [here](https://github.com/cobaltians/cobalt/wiki/Plugins-usage)
* Add the cobalt.asset-cache.js to your web JS folder
* Add an html link to the cobalt.assetcache.js plugin script after the cobalt link in the HEAD tag

## download

Downloads a distant file and store it locally in the web folder.

Use the `cobalt.assetCache.download` method like this

    cobalt.assetCache.download({
        url : 'https://server.com/path/file.ext'
      },
      function(data){
        cobalt.log('status', data.status, data);
    });

Parameters :

* First parameter is an object, that must contain :
  - `url` : (string) The url of the file to download from a server on the web
* `callback` : (function) The callback that will receive download status (see below).


### callback

Native side is sending download progress events using the web callback. Here is the data that can be sent in this callback :

**Download progress**

`{ status: 'downloading', progress : 42 }`

File is downloading. Download progress is at 42%.

**Download success and file available**

`{ status: 'success', path: 'file://.../a_random_file_name' }`

File has been successfully downloaded and is available at the given `path` for web use.

**Errors**

Network errors :

`{ status: 'error', cause : 'networkError' }`

Got a 404 error from distant server :

`{ status: 'error', cause : 'fileNotFound' }`

Error writing local file : 

`{ status: 'error', cause : 'writeError' }`

All other errors

`{ status: 'error', cause : 'unknownError' }`

## delete

Deletes a local file that was previously downloaded

Use the `cobalt.assetCache.delete` shortcut like this

    cobalt.assetCache.delete({ 
        url: 'https://server.com/path/file.ext'
        //or
        path: 'file:///.../previously_received_random_file_name'
      },
      function(data){
        cobalt.log('status', data.status, data);
    });

Here, in the first parameter, web can send `path` or `url`.

* `path` : (string) the local path of the file that was received in a download success callback.
* `url` : (string) the url of the file that was used in the download method.

If both are sent, nobody knows what happens.

### callback

Native side is sending callback to keep web updated of the deletion. Here are the possibilities :

**Success (file deleted)**

`{ status: 'success' }`

**Errors**

File not found

`{ status: 'error', cause : 'fileNotFound' }`

Other errrors

`{ status: 'error', cause : 'unknownError' }`

