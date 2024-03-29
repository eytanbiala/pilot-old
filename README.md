<h3 align="center">
  <a href="https://github.com/KrauseFx/fastlane">
    <img src="assets/fastlane.png" width="150" />
    <br />
    fastlane
  </a>
</h3>
<p align="center">
  <a href="https://github.com/KrauseFx/deliver">deliver</a> &bull; 
  <a href="https://github.com/KrauseFx/snapshot">snapshot</a> &bull; 
  <a href="https://github.com/KrauseFx/frameit">frameit</a> &bull; 
  <a href="https://github.com/KrauseFx/PEM">PEM</a> &bull; 
  <a href="https://github.com/KrauseFx/sigh">sigh</a> &bull; 
  <a href="https://github.com/KrauseFx/produce">produce</a> &bull; 
  <a href="https://github.com/KrauseFx/cert">cert</a> &bull; 
  <a href="https://github.com/KrauseFx/codes">codes</a> &bull;
  <a href="https://github.com/fastlane/spaceship">spaceship</a> &bull;
  <b>pilot</b> &bull;
  <a href="https://github.com/fastlane/boarding">boarding</a>
</p>
-------

<p align="center">
  <img src="assets/PilotTextTransparentSmall.png" width="500">
</p>

Pilot
============
[![Twitter: @KauseFx](https://img.shields.io/badge/contact-@KrauseFx-blue.svg?style=flat)](https://twitter.com/KrauseFx)
[![License](http://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://github.com/fastlane/pilot/blob/master/LICENSE)
[![Gem](https://img.shields.io/gem/v/pilot.svg?style=flat)](http://rubygems.org/gems/pilot)


###### The best way to manage your TestFlight testers and builds from your terminal

This tool allows you to manage all important features of Apple TestFlight using your terminal.

- Upload new builds and distribute them to all testers
- List all available builds
- Add and remove beta testers
- Get information about testers, like the registered devices
- Export and import all your testers

Get in contact with the developer on Twitter: [@KrauseFx](https://twitter.com/KrauseFx)

`pilot` uses [spaceship.airforce](https://spaceship.airforce) to interact with iTunes Connect :rocket:

-------
<p align="center">
    <a href="#installation">Installation</a> &bull; 
    <a href="#usage">Usage</a> &bull; 
    <a href="#tips">Tips</a> &bull; 
    <a href="#need-help">Need help?</a>
</p>

-------

<h5 align="center"><code>pilot</code> is part of <a href="https://fastlane.tools">fastlane</a>: connect all deployment tools into one streamlined workflow.</h5>

# Installation

    sudo gem install pilot

# Usage

For all commands you can specify the Apple ID to use using `-u felix@krausefx.com`. If you execute `pilot` in a project already using [fastlane](https://fastlane.tools) the username and app identifier will automatically be determined.

## Uploading builds

To upload a new build, just run 

```
pilot upload
```

This will automatically look for an `ipa` in your current directory and tries to fetch the login credentials from your [fastlane setup](https://fastlane.tools).

You'll be asked for any missing information. Additionally, you can pass all kinds of parameters:

```
pilot --help
```

You can also skip the submission of the binary, which means, the `ipa` file will only be uploaded and not distributed to testers:

```
pilot upload --skip_submission
```

`pilot` does all kinds of magic for you:

- Automatically detects the bundle identifier from your `ipa` file
- Automatically fetch the AppID of your app based on the bundle identifier

`pilot` uses [spaceship](https://spaceship.airforce) to submit the build metadata and the iTunes Transporter to upload the binary.

## List builds

To list all builds for specific application use

```
pilot list
```

The result lists all active builds and processing builds:

```
+-----------+---------+----------+----------+----------+
|                   Great App Builds                   |
+-----------+---------+----------+----------+----------+
| Version # | Build # | Testing  | Installs | Sessions |
+-----------+---------+----------+----------+----------+
| 0.9.13    | 1       | Expired  | 1        | 0        |
| 0.9.13    | 2       | Expired  | 0        | 0        |
| 0.9.20    | 3       | Expired  | 0        | 0        |
| 0.9.20    | 4       | Internal | 5        | 3        |
+-----------+---------+----------+----------+----------+
```

## Managing beta testers

### List of Testers

This command will list all your testers, both internal and external.

```
pilot list
```

The output will look like this:

```
+--------+--------+--------------------------+-----------+
|                    Internal Testers                    |
+--------+--------+--------------------------+-----------+
| First  | Last   | Email                    | # Devices |
+--------+--------+--------------------------+-----------+
| Felix  | Krause | felix@krausefx.com       | 2         |
+--------+--------+--------------------------+-----------+

+-----------+---------+----------------------------+-----------+
|                       External Testers                       |
+-----------+---------+----------------------------+-----------+
| First     | Last    | Email                      | # Devices |
+-----------+---------+----------------------------+-----------+
| Max       | Manfred | email@email.com            | 0         |
| Detlef    | Müller  | detlef@krausefx.com        | 1         |
+-----------+---------+----------------------------+-----------+
```

### Add a new tester

To add a new tester to both your iTunes Connect account and to your app (if given), use the `pilot add` command. This will create a new tester (if necesssary) or add an existing tester to the app to test.

```
pilot add email@invite.com
```

Additionally you can specify the app identifier (if necessary): 

```
pilot add email@email.com -a com.krausefx.app
```

### Find a tester

To find a specific tester use

```
pilot find felix@krausefx.com
```

The resulting output will look like this:

```
+---------------------+---------------------+
|            felix@krausefx.com             |
+---------------------+---------------------+
| First name          | Felix               |
| Last name           | Krause              |
| Email               | felix@krausefx.com  |
| Latest Version      | 0.9.14 (23          |
| Latest Install Date | 03/28/15 19:00      |
| 2 Devices           | • iPhone 6, iOS 8.3 |
|                     | • iPhone 5, iOS 7.0 |
+---------------------+---------------------+
```

### Remove a tester

This command will only remove external beta testers.

```
pilot remove felix@krausefx.com
```

### Export testers

To export all external testers to a CSV file. Useful if you need to import tester info to another system or a new account.

```
pilot export
```

### Import testers

Add external testers from a CSV file. Sample CSV file available [here](https://itunesconnect.apple.com/itc/docs/tester_import.csv).

```
pilot import
```

You can also specify the directory using

```
pilot export -c ~/Desktop/testers.csv
pilot import -c ~/Desktop/testers.csv
 ```

# Tips

## [`fastlane`](https://fastlane.tools) Toolchain

- [`fastlane`](https://fastlane.tools): Connect all deployment tools into one streamlined workflow
- [`deliver`](https://github.com/KrauseFx/deliver): Upload screenshots, metadata and your app to the App Store
- [`snapshot`](https://github.com/KrauseFx/snapshot): Automate taking localized screenshots of your iOS app on every device
- [`frameit`](https://github.com/KrauseFx/frameit): Quickly put your screenshots into the right device frames
- [`PEM`](https://github.com/KrauseFx/pem): Automatically generate and renew your push notification profiles
- [`produce`](https://github.com/KrauseFx/produce): Create new iOS apps on iTunes Connect and Dev Portal using the command line
- [`cert`](https://github.com/KrauseFx/cert): Automatically create and maintain iOS code signing certificates
- [`spaceship`](https://github.com/fastlane/spaceship): Ruby library to access the Apple Dev Center and iTunes Connect
- [`pilot`](https://github.com/fastlane/pilot): The best way to manage your TestFlight testers and builds from your terminal
- [`boarding`](https://github.com/fastlane/boarding): The easiest way to invite your TestFlight beta testers 

##### [Like this tool? Be the first to know about updates and new fastlane tools](https://tinyletter.com/krausefx)

## How is my password stored?

`pilot` uses the [CredentialsManager](https://github.com/fastlane/CredentialsManager) from `fastlane`.

# Need help?
- If there is a technical problem with `pilot`, submit an issue.
- I'm available for contract work - drop me an email: pilot@krausefx.com

# License
This project is licensed under the terms of the MIT license. See the LICENSE file.

> This project and all fastlane tools are in no way affiliated with Apple Inc. This project is open source under the MIT license, which means you have full access to the source code and can modify it to fit your own needs. All fastlane tools run on your own computer or server, so your credentials or other sensitive information will never leave your own computer. You are responsible for how you use fastlane tools.

# Contributing

1. Create an issue to start a discussion about your idea
2. Fork it (https://github.com/fastlane/pilot/fork)
3. Create your feature branch (`git checkout -b my-new-feature`)
4. Commit your changes (`git commit -am 'Add some feature'`)
5. Push to the branch (`git push origin my-new-feature`)
6. Create a new Pull Request

