# Selenium::Remote::Driver [![Build Status](https://travis-ci.org/gempesaw/Selenium-Remote-Driver.svg?branch=master)](https://travis-ci.org/gempesaw/Selenium-Remote-Driver)

[Selenium WebDriver][wd] is a test tool that allows you to write
automated web application UI tests in any programming language against
any HTTP website using any mainstream JavaScript-enabled browser. This
module is a Perl implementation of the client for the Webdriver
[JSONWireProtocol that Selenium provides.][jsonwire]

[wd]: http://www.seleniumhq.org/
[jsonwire]: https://code.google.com/p/selenium/wiki/JsonWireProtocol
[standalone]: http://selenium-release.storage.googleapis.com/index.html

## Installation

It's probably easiest to use the `cpanm` or `CPAN` commands:

```bash
$ cpanm Selenium::Remote::Driver
```

If you want to install from this repository, see the
[installation docs][] for more details.

[installation docs]: /INSTALL.md

## Usage

You can use this module to directly start the webdriver binaries in
your `$PATH` (after having [downloaded the driver servers][dl]). This
method does not require the JRE/JDK to be installed, nor does it
require the standalone server jar, despite the name of the module.

You can also use this module with the `selenium-standalone-server.jar`
to let it handle browser start up for you, and also manage Remote
connections where the server jar is not running on the same machine as
your test script is executing.

[dl]: #download-servers

### with a standalone server

Download the [standalone server][] and have it running on port 4444:

    $ java -jar selenium-server-standalone-X.XX.X.jar

Then the following should start up Firefox for you:

[standalone server]: http://selenium-release.storage.googleapis.com/index.html

#### Locally

```perl
use strict;
use warnings;
use Selenium::Remote::Driver;

my $driver = Selenium::Remote::Driver->new;
$driver->get('http://www.google.com');
print $driver->get_title . "\n"; # "Google"

my $query = $driver->find_element('q', 'name');
$query->send_keys('CPAN Selenium Remote Driver');

my $send_search = $driver->find_element('btnG', 'name');
$send_search->click;

# make the find_element blocking for a second to allow the title to change
$driver->set_implicit_wait_timeout(2000);
my $results = $driver->find_element('search', 'id');

print $driver->get_title . "\n"; # CPAN Selenium Remote Driver - Google Search
$driver->quit;
```

#### Saucelabs

If using Saucelabs, there's no need to have the standalone server
running on a local port, since Saucelabs provides it.

```perl
use Selenium::Remote::Driver;

my $user = $ENV{SAUCE_USERNAME};
my $key = $ENV{SAUCE_ACCESS_KEY};

my $driver = Selenium::Remote::Driver->new(
    remote_server_addr => $user . ':' . $key . '@ondemand.saucelabs.com',
    port => 80
);

$driver->get('http://www.google.com');
print $driver->get_title();
$driver->quit();
```

There are additional usage examples on [metacpan][meta], and also
[in this project's wiki][wiki], including
[setting up the standalone server][setup], running tests on
[Internet Explorer][ie], [Chrome][chrome], [PhantomJS][pjs], and other
useful [example snippets][ex].

[wiki]: https://github.com/gempesaw/Selenium-Remote-Driver/wiki
[setup]: https://github.com/gempesaw/Selenium-Remote-Driver/wiki/Getting-Started-with-Selenium%3A%3ARemote%3A%3ADriver
[ie]: https://github.com/gempesaw/Selenium-Remote-Driver/wiki/IE-browser-automation
[chrome]: https://github.com/gempesaw/Selenium-Remote-Driver/wiki/Chrome-browser-automation
[pjs]: https://github.com/gempesaw/Selenium-Remote-Driver/wiki/PhantomJS-Headless-Browser-Automation
[ex]:
https://github.com/gempesaw/Selenium-Remote-Driver/wiki/Example-Snippets

### [no standalone server](#download-servers)

- _Firefox 48 & newer_: install the Firefox browser, download
  [geckodriver][gd] and put it in your `$PATH`. If the Firefox browser
  binary is not in the default place for your OS and we cannot locate
  it via `which`, you may have to specify the binary location during
  startup.

- _Firefox 47 & older_: install the Firefox browser in the default
  place for your OS. If the Firefox browser binary is not in the
  default place for your OS, you may have to specify the binary
  location during startup.

- _Chrome_: install the Chrome browser, [download Chromedriver][dcd]
  and get `chromedriver` in your `$PATH`

- _PhantomJS_: install the PhantomJS binary and get `phantomjs` in
  your `$PATH`

When the browser(s) are installed and you have the appropriate binary
in your path, you should be able to do the following:

```perl
my $firefox = Selenium::Firefox->new;
$firefox->get('http://www.google.com');

my $chrome = Selenium::Chrome->new;
$chrome->get('http://www.google.com');

my $ghost = Selenium::PhantomJS->new;
$ghost->get('http://www.google.com');
```

Note that you can also pass a `binary` argument to any of the above
classes to manually specify what binary to start. Note that this
`binary` refers to the driver server, _not_ the browser executable.

```perl
my $chrome = Selenium::Chrome->new(binary => '~/Downloads/chromedriver');
```

See the pod for the different modules for more details.

[dcd]: https://sites.google.com/a/chromium.org/chromedriver/downloads
[gd]: https://github.com/mozilla/geckodriver/releases

## Selenium IDE Plugin

[ide-plugin.js](./ide-plugin.js) is a Selenium IDE Plugin which allows you to export tests recorded 
in Selenium IDE to a perl script.

### Installation in Selenium IDE

  1. Open Selenium IDE
  2. Options >> Options
  3. Formats Tab
  4. Click Add at the bottom
  5. In the name field call it 'Perl-Webdriver'
  6. Paste this entire source in the main textbox
  7. Click 'Save'
  8. Click 'Ok'
  9. Close Selenium IDE and open it again.

## Support and Documentation

There is a mailing list available at

https://groups.google.com/forum/#!forum/selenium-remote-driver

for usage questions and ensuing discussions. If you've come across a
bug, please open an issue in the [Github issue tracker][issue]. The
POD is available in the usual places, including [metacpan][meta], and
in your shell via `perldoc`.

```bash
$ perldoc Selenium::Remote::Driver
$ perldoc Selenium::Remote::WebElement
```

[issue]: https://github.com/gempesaw/Selenium-Remote-Driver/issues
[meta]: https://metacpan.org/pod/Selenium::Remote::Driver

## Contributing

Thanks for considering contributing! The contributing guidelines are
[in the wiki][contrib]. The documentation there also includes
information on generating new Linux recordings for Travis.

[contrib]: https://github.com/gempesaw/Selenium-Remote-Driver/wiki/Contribution-Guide

## Copyright and License

Copyright (c) 2010-2011 Aditya Ivaturi, Gordon Child

Copyright (c) 2014-2016 Daniel Gempesaw

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
