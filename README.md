# textmate-runner
===============

A template project (mostly usefuly for tmBundle authors) for creating little runner apps that can be used to associate custom file types with TextMate (using the Mac OS Launch Service). That way users can open documents written in any language that is supported by the according TextMate bundle right from the underlying OS; e.g. by double clicking a file in Finder/Path Finder. Bundle authors can also throw in some fancy file icons and short description texts for their supported file types.

A "runner app" will act as proxy between the underlying OS and TextMate. Everytime it is called for opening a given document, it delegates this "request" to the `mate` command line tool. Thus, it's not a transparent solution, but a simple and solid one.

## The template project includes

+ Runner app whose info.plist where you just have to fill in the blanks
+ Makefile for un-/registering the runner app
+ PSD files for custom file icons in all (?) sizes (even for Retina displays)

## Instructions

### The blanks (aka TextMate2 (XYZ).app/Contents/info.plist)

```xml
<key>CFBundleDisplayName</key>
<string>TextMate2 (XYZ)</string>
```

It is strongly recommended to keep that naming scheme. Just replace `XYZ` with your custom extension. When you want to register multiple extensions you could write something like `TextMate2 (XXX/YYY/ZZZ)`.

The actual party starts in the `CFBundleDocumentTypes` array. It contains one/more dictionaries, each of which representing one custom document type you want to claim:

```xml
<dict>
	<key>CFBundleTypeIconFile</key>
	<string>XXX</string>
	<key>CFBundleTypeExtensions</key>
	<array>
		<string>xxx</string>
	</array>
	<key>CFBundleTypeRole</key>
	<string>Editor</string>
	<key>CFBundleTypeName</key>
	<string>Description text for XXX file type</string>
</dict>
```

Note that `CFBundleTypeExtensions` may contain multiple alternative file suffixes. In order to make Mac OS display your custom icon image (set through the `CFBundleTypeIconFile` key; just the file's basename without extension) for `.xxx` files, you must assert that your ICNS file resides at `TextMate2 (XYZ).app/Contents/Resources/XXX.icns`.

### Makefile

```
make icns2imgs:       Converts to ICNS image bundle into multiple separate PNG image files
make imgs2icns:       Compile multiple separate PNG image files into a ICNS image bundle
make register:        Register file extensions with current TextMate Proxy Runner
make unregister:      Unregister current TextMate Proxy Runner for supported file extensions
```

#### icns2imgs

Call it like this: `make icns2imgs ICNS_FILE=path/to/your.icns` and it will expand your icon image bundle into multiple separate PNG image files that will be saved under `path/to/your.iconset`.

#### imgs2icns

Call it like this: `make imgs2icns IMG_DIR=path/to/your.iconset` and it compile your .iconset directory into a icon image bundle and save it at `path/to/your.icns`.

#### register / unregister

Called without any arguments. 

### Icon images

The PSD files that come with textmate-runner already contain properly aligned/scaled/positioned/formatted text layers just waiting for you to fill in the blanks. __TIP__: Just create a Pho'shop action that exports all opened docs as the PNGs for Web/Devices (IMPORTANT: 24bit with transparency).