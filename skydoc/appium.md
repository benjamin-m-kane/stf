https://github.com/sky-uk/com-client-sdk-test-framework-appium-core 
The following instructions will enable you to setup Appium on a Mac running OS X 10.10

# Installation

## 1. Install the Android SDK
At a minimum, the standalone SDK tools are required. However, Android Studio can also be downloaded which includes these tools.
Download here: http://developer.android.com/sdk/index.html#Other 
For the SDK tools, unzip and store somewhere permanent on your machine – for example, your home directory.
Keep note of where you stored it, as it will be referenced later in the environment variable $ANDROID_HOME

## 2. Install Homebrew
Homebrew, a package manager for Mac will enable you to install ruby and other dependencies for Appium. Install it with the following command, or learn more at http://brew.sh/ .

$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor

## 3. Install Ruby via Homebrew
Now that Homebrew is installed, the brew command can be used to install packages. Install ruby 2.1.1 with the following commands:

$ brew install chruby
$ brew install ruby-build
$ ruby-build 2.1.1 ~/.rubies/2.1.1
4. Modify shell profile
Change directories to your home directory.
$ cd ~

If you’re using bash as your shell (the default shell of the OS X Terminal), then use the following commands:

Edit your .bash_profile. If you don’t have one, then a blank file will simply show.

$ vim .bash_profile

If you are using zsh, edit .zshrc instead

$ vim .zshrc

Add the following lines to your .bash_profile or .zshrc

source /usr/local/opt/chruby/share/chruby/chruby.sh
source /usr/local/opt/chruby/share/chruby/auto.sh
chruby 2.1.1
export 

Add the following line, modifying the section in quotes to the full path to your Android SDK location.
ANDROID_HOME="/Users/your-username/development/adt-bundle-mac-x86_64-20140321/sdk"

Quit terminal or close all windows and re-open it to source your modified shell profile

Check that Ruby 2.1.1 is now your default version of Ruby

$ ruby -v

## 4. Modify path to include the Android SDK
Open your paths file

$ sudo vim /etc/paths

Add the following lines to the file. '[android sdk location]' should be replaced by the full path of your local Android SDK

[android sdk location]/build-tools
[android sdk location]/platform-tools
[android sdk location]/tools

## 5. Install Appium
This will install the command line version of appium. If you want, you can download the GUI version from the appium website (comes in very useful for getting XPATH of elements from iOS apps). At the time of writing, the framework and dependencies work with Appium version 1.4.0

$ brew install node
$ npm install -g appium@1.4.0
$ npm install wd

## 6. Clone the project and install dependencies
Ensure your git is configured with the account you use with Sky’s GitHub 

Clone the framework
$ git clone https://github.com/sky-uk/com-client-sdk-test-framework-appium-core.git

Navigate to the framework directoy

$ cd com-client-sdk-test-framework-appium-core

Install bundler and the gems the framework is dependent on
$ gem install bundler
$ bundle install

### 6.1 Potential setup problems

Nokogiri - if during the 'bundle install' step in section 2.6, you encounter a problem with xcode and Nokogiri, run the following command xcode-select —install

iOS Simulator - if the simulator asks for your password every time you run a test, run the following command DevToolsSecurity —enable

## 7. Running tests
As an example, this zip can be downloaded to run a simple Appium test that proves your environment is correctly setup. https://sky.slack.com/files/umar.amin/F0DDUDDMJ/internal_test_lab_appium_test.zip 

Connect an Android device on 4.3 or above to your mac with USB debugging enabled on the device

Check the device is connected succesfully by running this command:

$ adb devices

You should see the device in the List of devices attached

Unzip the folder and navigate to within the unzipped folder from Terminal
$ cd internal_test_lab_appium_test

Start the appium server from within this directory by typing:
$ appium

Open another terminal window or tab, change directory to internal_test_lab_appium_test run and run the test:
$ cd internal_test_lab_appium_test
$ ruby ‘run_test.rb’

Watch the simple script run on the device

## 8. Running tests via STF
Log into the STF web application at https://10.65.88.133 (URL may change)

Select a device to use by clicking the Use button



Paste the adb connect command from the Remote debug section into your terminal and run it


Run $ adb devices to confirm the device is now connected

sch56:~/ $ adb devices                                               
List of devices attached
10.65.88.133:15009  device
If you only have one device connected, Appium will run tests on that device by default. 

# Troubleshooting
If you are seeing “unauthorized” instead of “device” next to the IP address of a connected device, you may need to click the “trust” button on the screen in the STF browser to add the adb key to STF.
