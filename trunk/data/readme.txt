/**
@mainpage "Dungeon Hunter PHD" readme
@author (c) Gameloft 2018, Ukraine, Kharkiv

Detailed info about the project can be found at:
	- https://confluence.gameloft.org/display/DHP/Dungeon+Hunter+PHD+Home

1) Tools to install:
	- Common tools for programmers and non-programmers:
		- Mandatory:
			1. "TortoiseSVN" with command line tools - http://tortoisesvn.net/downloads.html.
				- We use version 1.8 and higher.
				- [!] Please pay attention that "command line client tools" option should be enabled when installing.
			2. "Python" 2.x.x - https://www.python.org/downloads/.
				- We use version 2.7.9. Do not use Python 3.0 and higher.
				- [!] Please pay attention that "x86" version should be used.
				- [!] Do not forget to check "Add python.exe to Path" option.
			3. "Lua" - ".\tools\bin\Installs\vcredist_x86.exe" and ".\tools\bin\Installs\Lua\LuaForWindows_v5.1.4-40.exe".
				- [!] Please pay attention that "Lua Modules" option should be enabled
			4. "Java for Windows - Offline Installation" - https://www.java.com/en/download/manual.jsp. Search and download "Windows Offline".
				- We use jre-8u144-windows-i586 and jre-7u80-windows-i586

		- Optional (in case you have errors while making data / game/tools run):
			1. "Visual C++ Redistributable for Visual Studio 2013" - https://www.microsoft.com/en-us/download/details.aspx?id=40784.
			2. "Visual C++ 2008 Redistributable Package (x86) " - http://www.microsoft.com/en-us/download/confirmation.aspx?id=29.
			3. "Visual C++ Redistributable for Visual Studio 2012 Update 4" - http://www.microsoft.com/en-us/download/details.aspx?id=30679.
				- Both x86 and x64 versions

	- Tools for programmers only:
		- For Windows:
			1. "Microsoft Visual Studio Express 2015 Update2" (used as main tool) - http://go.microsoft.com/fwlink/?LinkID=699339&clcid=0x409.
			2. "Microsoft Visual Studio Express 2013 Update5" (for Platform Toolset v120) - https://go.microsoft.com/fwlink/?LinkId=532500&clcid=0x409.
				- [!] Please pay attention that project needs VS2013 Update5
			3. "Python Tools for Visual Studio" if you want to debug game-portal scripts locally - http://pytools.codeplex.com/
		- For Android:
			1. Download "Android SDK" - http://developer.android.com/sdk/index.html
			   Installer without Android Studio available by link - https://dl.google.com/android/installer_r24.4.1-windows.exe
				- Android SDK Tools: 24.1.1
				- Android SDK Platform-tools: 23.0.1
				- Android SDK Build Tools: 23.0.1
				- API level: 24 (Android 7.0)
			2. Download "Android NDK" r13b+ x64 from NDK Downloads page - https://developer.android.com/ndk/downloads/index.html.
				- We use r13b
			3. Download "JDK" - http://www.oracle.com/technetwork/java/javase/downloads/index.html. Click on "JDK Download" button and select "Java SE Development Kit" for "Windows x64" (development tools and Public JRE).
				- We use "Java SE Development Kit 8u144".
			4. Download "Cygwin" - https://svn01/vc/gllegacy/release_v2.0/trunk/tools/sdk/Cygwin.
				- We use version 1.7.25
			5. Install 'Cmake' tool from Android SDK Manager or download 'cmake.zip' from https://svn02/vc/dungeon_hunter_phd/stuff/programming/Android and unpack it to the Android SDK folder to the "cmake" folder.
			6. Download and install Samsung USB driver - http://developer.samsung.com/technical-doc/view.do?v=T000000117.

2) Project configuration:
	- Generic instructions:
		1. Apply patches by running "applyPatches.bat" (once and each time when something is committed to the "patches" folder).

3) Resource exporting:
	- General:
		1. Win and Android build uses the same resources and folders structure (except that Android uses archived version of resources).
		2. Each type of resource goes to the specified folder. For example: "levels", "models", etc.
		3. We use "Substance" tool to generate textures. All Substance textures goes to the ".\build\win\textures_sbs_*\" folder. They have ".sbsbin" extension.
		4. All non-generated textures converted to the "ETC1" format and copied to the ".\build\win\textures".
		5. All non-generated textures with alpha became split on two textures: one with `RGB` channels and second with `Alpha` channel.
	- For WIN32:
		1. Run "makedata.bat".
		2. All generated game data can be found at the ".\build\win\" folder.
	- For Android:
		1. Run "makedataAndroid.bat".
		2. All generated data after archiving with "7z.exe" tool can be found at the ".\build\android\assets" folder.
	- You can use "clean" parameter if you need to make data with cleanup.
	- Generate texture preload data:
		1. preliminary setup 'lxml' python library:
			- goto https://pypi.python.org/pypi/lxml/3.4.4  and download version with "*-cp27-none-win32.whl"
			- run: "pip install *-cp27-none-win32.whl"
			- [!] Please note that you need Python 2.7.9 or higher in order to use "pip".
		2. Go to ".\tools\scripts\" folder and run "generate_texture_preload_data.bat"
		3. Commit the changes
		- [!] Please note that preload info generation does not included in regular "makedata" script and you should make it by your own (we use "Jenkins" in order to automatize this process).

4) Compiling:
	- preliminary:
		- [!] Please note that "revision.h" file should be generated first by calling "makedata*.bat" or by calling ".\tools\scripts\get_revision.bat".

	- For WIN32:
		1. Open and compile ".\sources\prj\vs2013\DHPHD.sln" (you can select `Debug` or `Release` configuration) or just run "buildWin.bat" to compile `Release` configuration.
			- [!] Please do not upgrade VC++ Compiler and Libraries on first solution open (just click "Cancel") because all projects use v120 toolset.
		2. "DHPHD.exe" file can be found at the ".\build\win\" folder. You can run game directly from this folder.
		3. "buildWin.bat" file will generate "bm_DHPHD.exe" file that should be committed to the SVN in order to have actual executable available for the non-programmers.
		- [!] If you want to use project with compilation units use `Debug_CU` or `Release_CU` configurations (it runs special script from  ".tools\scripts\" folder to generate CU).

	- For Android:
		- preliminary:
			1. run ".\sources\trident\libs\AndroidFramework\start_build_configurator.bat"
			2. open "Build Tools" folder
			3. provide path to the 'Android SDK', 'Android NDK', 'Cygwin', 'JDK (Java SDK)' and 'Python'
			4. close the tool
			5. distributed `FastBuild` setup:
				a. add environment variable named: FASTBUILD_BROKERAGE_PATH to the user variable section and set its value with the network folder:
				- Our team uses: \\khawks0216\share\dhphd_distributed_build
				b. copy: ".\sources\trident\libs\AndroidFramework\tools\FastBuild\FBuildWorker.exe" to the: "C:\Users\yourname.surname\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"
				c. run "FBuildWorker.exe" from the new path and setup it

		1. First building should be slow because building tool downloads some utils from Internet.
			- In case if you get errors about timeout, please ask your team lead to write mail to GNS to grant access to the following sites:
				- https://jcenter.bintray.com/
				- https://repo1.maven.org/maven2/
				- https://services.gradle.org/
		2. In order to make Android build please use "buildAndroid.bat" file to compile `Release` or `Debug` configuration and install apk to the connected the device.
		3. Generated ".apk" file can be found at the ".\build\android\" folder.
		4. You can use "buildAndroid.bat debug" command to compile `Debug` configuration.
		5. You can use "buildAndroid.bat clean" to clean all compiled sources and object files.
			- [!] Also use this to make Android build from the scratch or when you have errors with native or Java compilation and also when you add/delete some plugin or SDK
		6. You can use "installAndroid.bat" command to install previously generated `Release` ".apk" to the device.
		7. You can use "installAndroid.bat debug" command to install previously generated `Debug` ".apk" to the device.
		8. In case you want to use Android Studio to debug the application you need:
			a. copy NDK to the Android SDK folder (and name it `ndk-bundle` for example).
			- it is actually better to move NDK here instead of copying (just move NDK and update 'Android NDK' path properly).
			b. perform "buildAndroid.bat debug noinstall" to generate Android Studio project.
			- in case of changes in config files you can run "buildAndroid.bat reconfig" (but please keep an eye that you need to have "android:debuggable" set true in AndroidManifest file in order to be able to debug).
			c. open Android Studio and select ".\sources\trident\libs\AndroidFramework\generated\as_prj".
			d. click on "Cancel" button when welcome screen appears and prompts to validate current SDK.
				Also click on "Don't remind me again for this project" button when "Android Gradle Plugin Update Recommended" popup appears then wait till it project caches.
			e. now you can compile and debug java/native code from the IDE.
			- apply suggested updates if popup appears after clicking on "Debug 'app'" button.
		- tested on: Android Studio 2.3.3 and 3.0.1
		- refer to the https://docs.gameloft.org/android-studio-2-2-native-debugging/ for the additional information and FAQs.
*/