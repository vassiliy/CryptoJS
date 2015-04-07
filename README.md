CryptoJS for rails-assets
--------

> This repo is forked from https://github.com/sytelus/CryptoJS aimed to manage CryptoJS as bower component.

> To use this component with rails-assets gems, though, some minor tweaks implemented in this package and described in this **readme** are inevitably necessary, hence this **readme** is based on these projects documentation, but focused on own features. For more information don't hesitate to reference the original documentation.

This repo is straight unmodified-in-any-way copy of Google Code hosted CryptoJS project at https://code.google.com/p/crypto-js/ . This is hosted at github to add rails-assets compatible bower package so future updates can be managed better.

### Directory Structure
You have two folders:
* components
* rollups

The files in rollups folder is concatenation of one or more files in components folder followed by minification. This makes files in rollups folder standalone includable in your projects without worrying about its dependencies. You can view relation between files in rollup and components here: https://code.google.com/p/crypto-js/source/browse/tags/3.1.2/builder/build.yml

### Install
**node & npm**
```
brew install node
```
or with whatever package manager you use.

Then **Bower**:
```
sudo npm install -g bower
```
Probably you'll succeed without sudo, but let's not rely too much on that.

Whether you made steps above or already had **Bower** installed,
```
bower info cryptojs-railssets
```
will show you if you're ready for further, some interesting things about this package and, most valuable of all — **list of components you'll load to your Rails application assets by `#= require cryptojs-railssets` declaration** in `main` key of `bower.json`.

> Value of `main` key in original package made `#= require cryptojs` gain you nothing — that's why this project came to live.

From this point you know if you're satisfied with components list (by now it contains CryptoJS AES cipher rollup, JSON formatter for it and Joseph Myers' MD5 algorithm implementation). If this set is good for you, use it as it is rails-assets way or, if you need anything else from CryptoJS, start further magic.

Fork this, edit `main` key to meet your needs, switch `homepage` and `repository.url` to your forked repo, bump `version` key of `bower.json`, push the same git tag and register your component with bower with respect to http://bower.io/docs/creating-packages/#register

I'll humbly suggest to register it as `cryptojs-railssets-#{suffix you'll choose for your awesome project}`, but it's completely up to you.

Then check `bower info` of your brand new component and, if everything's fine, register it with Rails Assets: https://rails-assets.org/components/new

Then follow the rails-assets way: `bundle`, `#= require` and so on and enjoy you are not forced to junk the heap of files to your `vendor/assets` or anywhere even less appropriate.

### JSON formatter
Ultimately necessary to stringify encrypted of decrypted data. This formatter is described in **CryptoJS** manual, but not included in the original package, but here integrity is restored, we have it for you.
```javascript
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase", { format: JsonFormatter });
alert(encrypted); // {"ct":"tZ4MsEnfbcDOwqau68aOrQ==","iv":"8a8c8fd8fe33743d3638737ea4a00698","s":"ba06373c8f57179c"}

var decrypted = CryptoJS.AES.decrypt(encrypted, "Secret Passphrase", { format: JsonFormatter });
alert(decrypted.toString(CryptoJS.enc.Utf8)); // Message
```
Nice and easy, if you haven't forgot to include it to `main`.

### Joseph Myers' MD5 algorithm implemenation
In `md5.js` in package root. Not to shame Google or it's best practices, just for you to know. Please take a look at Joseph Myers' lenghty article http://www.webreference.com/programming/javascript/jkm3/index.html to find out why it is written how it is written.

*Supposed you've read the article* I hate the mess in `Object.keys(window)`, but sometimes performance is served before prettiness. Seeming awkward and kinda ugly `md5()` works 2.5–3.3 times faster than cool `CryptoJS.MD5()` according to **10k strings of 1k random chars each** digest benchmark I've held few minutes before writing this.

**Also IMPORTANT:** `md5()` is able to verify MD5 digests computed server-side, even if their source texts contain non-ANSI symbols. I haven't made `CryptoJS.MD5()` do the same trick, tired of fighting with `Malformed UTF-8 data`, so, maybe, there's a way to do it that I don't (yet) know.

Not to forget, use this function to provide `md5()` with server-side-compatible input (sorry for coffeescript):
```coffeescript
encode_utf8 = (string) ->
  unescape(encodeURIComponent string)
```

### Copyrights
Please see copyrights.txt file in the root folder which is copy of corresponding file from Google Code project. This github repository does not assert any copyrights beyond what original CryptoJS does.
