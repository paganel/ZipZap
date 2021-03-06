[![Build Status](https://travis-ci.org/pixelglow/ZipZap.svg)](https://travis-ci.org/pixelglow/ZipZap)

**ZipZap** is a zip file I/O library for Mac OS X and iOS.

The zip file is an ideal container for compound Objective-C documents. Zip files are widely used and well understood. You can randomly access their parts. The format compresses decently and has extensive operating system and tool support. So we want to make this format an even easier choice for you. Thus, the library features:

* **Easy-to-use interface**: The public API offers just two classes! Yet you can look through zip files using familiar *NSArray* collections and properties. And you can zip, unzip and rezip zip files through familiar *NSData*, *NSStream* and Image I/O classes.
* **Efficient implementation**: We've optimized zip file reading and writing to reduce virtual memory pressure and disk file thrashing. Depending on how your compound document is organized, updating a single entry can be faster than writing the same data to a separate file.
* **File format compatibility**: Since *ZipZap* closely follows the [zip file format specification](http://www.pkware.com/documents/casestudies/APPNOTE.TXT), it works with most Mac, Linux and Windows zip tools.

Install
-------

As an independent project:

* In the Terminal, run `git clone https://github.com/pixelglow/ZipZap.git`.
* Within the *ZipZap* directory, open the *ZipZap.xcodeproj* Xcode project.
* In the Xcode project, select either the *ZipZap (iOS Framework)*, *ZipZap (iOS Static Library)*, *ZipZap (OS X Framework)* or *ZipZap (OS X Static Library)*  scheme from the drop down.
* You can now build, test (Mac OS X only) or analyze with the selected scheme.
* The built libraries and test cases are in a subdirectory of *~/Library/Developer/Xcode/DerivedData*.

As a project integrated with your own workspace:

* In the Terminal, run `cd workspace` then `git submodule add https://github.com/pixelglow/ZipZap.git`.
* In your Xcode workspace, choose the *File > Add Files to "workspace"* menu item, then within the *ZipZap* directory pick the *ZipZap.xcodeproj* Xcode project.
* In any project target that will use *ZipZap*:
  * In *Build Phases > Link Binary With Libraries*, add the corresponding *libZipZap.a* and any other library listed in the Require Link section below.
  * Under *Build Settings > Search Paths > Header Search Paths*, add *../ZipZap*.
* You can now build, test or analyze those project targets.

Use
---

Header includes:

	#import <ZipZap/ZipZap.h>
	
Reading an existing zip file:

	ZZArchive* oldArchive = [ZZArchive archiveWithURL:[NSURL fileURLWithPath:@"/tmp/old.zip"]
	                                            error:nil];
	ZZArchiveEntry* firstArchiveEntry = oldArchive.entries[0];
	NSLog(@"The first entry's uncompressed size is %lu bytes.", (unsigned long)firstArchiveEntry.uncompressedSize);
	NSLog(@"The first entry's data is: %@.", [firstArchiveEntry newDataWithError:nil]);
	
Writing a new zip file:

	ZZArchive* newArchive = [[ZZArchive alloc] initWithURL:[NSURL fileURLWithPath:@"/tmp/new.zip"]
	                                               options:@{ZZOpenOptionsCreateIfMissingKey : @YES}
	                                                 error:nil];
	[newArchive updateEntries:
						 @[
						 [ZZArchiveEntry archiveEntryWithFileName:@"first.text"
														 compress:YES
														dataBlock:^(NSError** error)
															  {
																  return [@"hello, world" dataUsingEncoding:NSUTF8StringEncoding];
															  }]
						 ]
					    error:nil];

Updating an existing zip file:

	ZZArchive* oldArchive = [ZZArchive archiveWithURL:[NSURL fileURLWithPath:@"/tmp/old.zip"]
	                                            error:nil];
	[oldArchive updateEntries:
	 [oldArchive.entries arrayByAddingObject:
	  [ZZArchiveEntry archiveEntryWithFileName:@"second.text"
									  compress:YES
									 dataBlock:^(NSError** error)
										   {
											   return [@"bye, world" dataUsingEncoding:NSUTF8StringEncoding];
										   }]]
						error:nil];

Advanced uses: [Recipes](https://github.com/pixelglow/ZipZap/wiki/Recipes)

API references: [References](http://pixelglow.github.io/ZipZap/api/index.html)

Require
-------

* **Build**: Xcode 6 and later.
* **Link**: Only system libraries; no third-party libraries needed.
  * *ApplicationServices.framework* (Mac OS X) or *ImageIO.framework* (iOS)
  * *Foundation.framework*
  * *libz.dylib*
* **Run**: Mac OS X 10.9 (Mavericks) or iOS 7.0 and later.

Support
-------

* Follow us on Twitter: [@pixelglow](http://twitter.com/pixelglow).
* Raise an issue on [ZipZap issues](https://github.com/pixelglow/ZipZap/issues).

License
-------

*ZipZap* is licensed with the BSD license.

Donate
------

* Tip with [ChangeTip](http://pixelglow.tip.me).
