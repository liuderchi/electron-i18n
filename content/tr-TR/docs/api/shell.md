# shell

> Varsayılan uygulamalarını kullanarak dosyaları ve URL'leri yönetin.

Process: [Main](../glossary.md#main-process), [Renderer](../glossary.md#renderer-process)

The `shell` module provides functions related to desktop integration.

Bir URL'yi kullanıcının varsayılan tarayıcısında açmaya örnek:

```javascript
const {shell} = require('electron')

shell.openExternal('https://github.com')
```

## Metodlar

The `shell` modülünün aşağıdaki yöntemleri vardır:

### `shell.showItemInFolder(fullPath)`

* `fullPath` String

`Boolean` Döndürür - Öğenin başarıyla gösterilip gösterilmediği

Verilen dosyayı bir dosya yöneticisinde görüntüler. Mümkünse, dosyayı seçin.

### `shell.openItem(fullPath)`

* `fullPath` String

`Boolean` Döndürür - Öğenin başarılı bir şekilde açılıp açılmadığı.

Verilen dosyayı masaüstünün varsayılan yöntemiyle açın.

### `shell.openExternal(url[, options, callback])`

* `url` Dize
* `ayarlar` Obje (isteğe bağlı) *macOS* 
  * `activate` Boolean - `true` to bring the opened application to the foreground. The default is `true`.
* `geri arama` Function (optional) - If specified will perform the open asynchronously. *macOS* 
  * `error` Hata 

Returns `Boolean` - Whether an application was available to open the URL. If callback is specified, always returns true.

Verilen harici protokol URL'sini masaüstünde varsayılan şekilde açın. (Örneğin, mailto: kullanıcının varsayılan posta aracısındaki URL'leri).

### `shell.moveItemToTrash(fullPath)`

* `fullPath` String

`Boolean` Döndürür - Öğenin çöp kutusuna başarıyla taşınıp taşınmadığı

Verilen dosyayı çöp kutusuna taşır ve işlem için bir boolean durumu döndürür.

### `shell.beep()`

Bip sesini oynatın.

### `shell.writeShortcutLink(shortcutPath[, operation], options)` *Windows*

* `shortcutPath` String
* `operation` String (optional) - Default is `create`, can be one of following: 
  * `create` - Creates a new shortcut, overwriting if necessary.
  * `update` - Updates specified properties only on an existing shortcut.
  * `replace` - Overwrites an existing shortcut, fails if the shortcut doesn't exist.
* `options` [ShortcutDetails](structures/shortcut-details.md)

`Boolean` Döndürür - Kısayolun başarıyla oluşturulup oluşturulmadığı

`shortcutPath` bir kısayol bağlantısı oluşturur veya güncelleştirir.

### `shell.readShortcutLink(shortcutPath)` *Windows*

* `shortcutPath` String

Returns [`ShortcutDetails`](structures/shortcut-details.md)

Kısayol bağlantısını `shortcutPath` adresinde çözer.

Bir hata oluştuğunda istisna atılır.