# xojo-ATSApplicationFontsPath
You can use fonts in your app that are not installed on the user’s system.
First, add a copy step to your build process (to run AFTER the build step) and copy the font files into your app’s Resources directory. (I like to have a "Fonts" subdirectory within the Resources directory, but it’s not essental.)
Then you add a script step to run after the copy step.
This step uses PlistBuddy (provided with Apple’s Xcode install) to add an ATSApplicationFontsPath option to the newly built app’s Info.plist file.

var path as String = CurrentBuildLocation + "/" + CurrentBuildAppName.ReplaceAll( " ", "\ " )
var result as String = DoShellCommand( "/usr/libexec/PlistBuddy -c ""Add ::ATSApplicationFontsPath string Fonts/"" " + path + ".app/Contents/Info.plist" )
if result <> "" then print( result )

As you can see, error checking is pretty minimal.

CurrentBuildLocation is already escaped for command line use, CurrentBuildAppName is NOT escpaed, so you need to create a final fully espaced version of the whole path.

You pass this in to PlistBuddy, specifying the Fonts subdirectory.

Now when you run your app, it has access to any fonts that are in that folder, with no need for the user to install them.

The demo project file uses the font in the Paint event of the main app window to draw some simple text. Nothing terribly elaborate or clever. But this technique comes in to its own when including dingbats or other symbol or other special fonts and then using them within your app.

The demo font included with the project is "ChicagoFLF", which is a public domain font, created and made available by Robin Casady, based on the original Apple Chicago system typeface designed by Susan Kare.

https://fontlibrary.org/en/font/chicagoflf

https://en.wikipedia.org/wiki/Chicago_(typeface)
