# User Agent Parser Comparison

[![Build Status](https://travis-ci.org/diablomedia/useragent-parser-comparison.svg?branch=master)](https://travis-ci.org/diablomedia/useragent-parser-comparison) [![Type Coverage](https://shepherd.dev/github/diablomedia/useragent-parser-comparison/coverage.svg)](https://shepherd.dev/github/diablomedia/useragent-parser-comparison)

This application was put together to assist us in making a decision on which UserAgent parsing library to use, and then modified some to be a bit more user friendly to encourage use among the different parsing library maintainers.  It's still a bit work in progress, but in its current state it's very useful at benchmarking the different parsing libraries (across different languages) and comparing the results of the parsers against "known" useragents (right now these come from the parser libraries' unit tests, but a known list could be built and tested against).

This is loosely based on a benchmark utility that has been around for a while (but not currently maintained): https://github.com/kenjis/user-agent-parser-benchmarks

Right now this application is entirely a CLI application. Most of the data generated by the application is stored as JSON files, so it's certainly possible that a Web UI could be built around the data files which might make analysis easier.

The CLI is written in PHP (using the Symfony Console component), but the parsers (and tests) can be written in any language. Currently, most of the parsers are written in PHP, but the structure of the application (and how it calls the parsers) was intentionally built to support parsers from any language (PRs welcome!).

## Latest "Accuracy" Results (2018-06-08)

Parser        | Version          | Browser Results    | Platform Results   | Device Results      | Accuracy Score
-----|-----|-----|-----|-----|-----
3rd-eden-useragent-js-2   | 2.3.0          | 66025/87529 75.43% | 44378/57961 76.57% | 18918/89948 21.03%   | 129321/235438 54.93%
browscap-js-1             | 1.11.6000028   | 71865/87529 82.1%  | 54019/57961 93.2%  | 101811/149420 68.14% | 227695/294910 77.21%
browscap-js-2             | 2.2.6000028    | 71865/87529 82.1%  | 54019/57961 93.2%  | 101811/149420 68.14% | 227695/294910 77.21%
browscap-php-2            | 2.1.1-6000029  | 72107/87529 82.38% | 54093/57961 93.33% | 102103/149420 68.33% | 228303/294910 77.41%
browscap-php-3-full       | 3.1.0-6000029  | 71993/87529 82.25% | 54059/57961 93.27% | 102128/149420 68.35% | 228180/294910 77.37%
browscap-php-3-lite       | 3.1.0-6000029  | 31856/87529 36.39% | 24905/57961 42.97% | 41083/59472 69.08%   | 97844/204962 47.74%  
browscap-php-3-standard   | 3.1.0-6000029  | 66827/87529 76.35% | 31478/57961 54.31% | 51418/59472 86.46%   | 149723/204962 73.05%
browscap-php-4-full       | 4.1.0-6000029  | 71993/87529 82.25% | 54059/57961 93.27% | 102128/149420 68.35% | 228180/294910 77.37%
crossjoin-1               | v1.0.6-6000029 | 71990/87529 82.25% | 54060/57961 93.27% | 102127/149420 68.35% | 228177/294910 77.37%
crossjoin-2               | v2.0.5-6000029 | 71776/87529 82%    | 54067/57961 93.28% | 101705/149420 68.07% | 227548/294910 77.16%
crossjoin-3               | v3.0.5-6000029 | 71776/87529 82%    | 54067/57961 93.28% | 101705/149420 68.07% | 227548/294910 77.16%
device-detector-node-js-0 | 0.1.38         | 63203/87541 72.2%  | 48193/57973 83.13% | 91570/149432 61.28%  | 202966/294946 68.81%
donatj-0                  | v0.9.0         | 52365/87529 59.83% | 19322/29537 65.42% | 0/0 0.0%             | 71687/117066 61.24%  
endorphin-3               | 3.0.1          | 38358/87529 43.82% | 36046/57961 62.19% | 32979/104316 31.61%  | 107383/249806 42.99%
jenssegers-agent-2        | v2.6.0         | 41855/87529 47.82% | 48871/57961 84.32% | 58944/104316 56.51%  | 149670/249806 59.91%
php-get-browser           | 7.2.4-6000029  | 72123/87529 82.4%  | 54093/57961 93.33% | 102109/149420 68.34% | 228325/294910 77.42%
piwik-device-detector-3   | 3.10.2         | 64775/87529 74%    | 49654/57961 85.67% | 119636/149420 80.07% | 234065/294910 79.37%
sinergi-6                 | 6.1.2          | 42353/87529 48.39% | 46034/57961 79.42% | 30924/74417 41.56%   | 119311/219907 54.26%
ua-parser-php-3           | v3.5.0-ff9a4ef | 66453/87529 75.92% | 29412/57961 50.74% | 62414/89948 69.39%   | 158279/235438 67.23%
uaparser-js-0             | 0.7.18         | 53226/87529 60.81% | 45505/57961 78.51% | 87067/149420 58.27%  | 185798/294910 63%    
whichbrowser-js-0         | 0.2.9          | 67142/87529 76.71% | 53153/57961 91.7%  | 120824/149420 80.86% | 241119/294910 81.76%
whichbrowser-php-2        | v2.0.32        | 67140/87529 76.71% | 53157/57961 91.71% | 120860/149420 80.89% | 241157/294910 81.77%
woothee-js-1              | 1.7.0          | 41103/87529 46.96% | 48388/57961 83.48% | 18853/29899 63.06%   | 108344/175389 61.77%
woothee-php-1             | v1.7.0         | 41009/87529 46.85% | 48463/57961 83.61% | 18910/29899 63.25%   | 108382/175389 61.8%  
zsxsoft-php-1             | 1.4            | 61496/87529 70.26% | 46515/57961 80.25% | 39867/89948 44.32%   | 147878/235438 62.81%

[View Full Results](https://github.com/diablomedia/useragent-parser-comparison/wiki/fullresults)

## Latest Benchmark Results

Performed with the following command:

`./bin/console benchmark ./files/top-1000-201806.txt --iterations=10`

### VMWare ESX Virtual Server

2.33 GHz Intel Xeon E5410, 4 GB RAM, Magnetic Disk Drive (RAID), PHP 7.0.22 (2018-01-02)

Parser                  | Average Init Time | Average Parse Time | Average Extra Time | Average Memory Used
-----|-----|-----|-----|-----
browscap-js-1           | 0.066s            | 58.705s            | 0.406s             | 108.52 M
browscap-php-2          | 1.03s             | 72.901s            | 0.225s             | 141.64 M
browscap-php-3-full     | 0.043s            | 53.877s            | 0.166s             | 4.55 M
browscap-php-3-lite     | 0.032s            | 2.07s              | 0.13s              | 1.37 M
browscap-php-3-standard | 0.036s            | 20.131s            | 0.139s             | 2.68 M
crossjoin-1             | 0.019s            | 48.111s            | 0.149s             | 869.92 K
crossjoin-2             | 0.026s            | 108.172s           | 0.149s             | 1.04 M
crossjoin-3             | 0.035s            | 107.96s            | 0.148s             | 1.01 M
php-get-browser         | 0.029s            | 118.647s           | 1.93s              | **391.2 K**
piwik-device-detector-3 | 0.518s            | 10.882s            | 0.164s             | 3.02 M
ua-parser-php-3         | 0.027s            | 3.584s             | 0.149s             | 1.56 M
whichbrowser-2          | 0.039s            | 4.478s             | 0.157s             | 16.53 M
woothee-php-1           | 0.029s            | 0.079s             | 0.127s             | 822.23 K
zsxsoft-php-1           | 0.012s            | 1.418s             | 0.132s             | 982.43 K
donatj-0                | **0.003s**            | 0.205s             | 0.13s              | 513.16 K
endorphin-3             | 0.017s            | 1.201s             | 0.131s             | 551.91 K
jenssegers-agent-2      | 0.012s            | 43.464s            | 0.133s             | 797.51 K
sinergi-6               | 0.007s            | **0.02s**              | **0.122s**             | 509.16 K
uaparser-js-0           | 0.025s            | 0.097s             | 0.276s             | 5.83 M

### MacBook Pro

2.9 GHz Intel Core i7, 16 GB RAM, SSD Drive, PHP 7.2.4 (2018-06-10)

Parser                  | Average Init Time | Average Parse Time | Average Extra Time | Average Memory Used
-----|-----|-----|-----|-----
3rd-eden-useragent-js-2 | 0.157s            | 0.228s             | 0.4s               | 16.02 M             
browscap-js-1           | 0.024s            | 35.534s            | 0.127s             | 61.57 M             
browscap-js-2           | 0.029s            | 36.626s            | 0.122s             | 59.71 M             
browscap-php-2          | 0.686s            | 6.907s             | 0.083s             | 145.39 M            
browscap-php-3-full     | 0.012s            | 8.288s             | 0.072s             | 4.65 M              
browscap-php-3-lite     | 0.012s            | 0.474s             | 0.054s             | 1.23 M              
browscap-php-3-standard | 0.011s            | 3.52s              | 0.062s             | 2.67 M              
browscap-php-4-full     | 0.026s            | 14.444s            | 0.095s             | 4.64 M              
crossjoin-1             | 0.008s            | 9.096s             | 0.087s             | 748.77 K            
crossjoin-2             | 0.013s            | 28.353s            | 0.06s              | 1023.48 K           
crossjoin-3             | 0.016s            | 29.778s            | 0.07s              | 1002.4 K            
donatj-0                | 0.002s            | 0.01s              | 0.051s             | 534.52 K            
endorphin-3             | 0.006s            | 0.326s             | 0.051s             | 557.28 K            
jenssegers-agent-2      | 0.014s            | 0.892s             | 0.052s             | 871.7 K             
php-get-browser         | 0.005s            | 108.675s           | 1.638s             | 428.3 K             
piwik-device-detector-3 | 0.08s             | 0.892s             | 0.057s             | 3.31 M              
sinergi-6               | 0.002s            | 0.001s             | 0.049s             | 529.38 K            
ua-parser-php-3         | 0.027s            | 0.388s             | 0.051s             | 1.65 M              
uaparser-js-0           | 0.011s            | 0.042s             | 0.1s               | 7.64 M              
whichbrowser-js-0       | 0.233s            | 0.567s             | 0.1s               | 30.06 M             
whichbrowser-php-2      | 0.025s            | 0.403s             | 0.057s             | 17.69 M             
woothee-js-1            | 0.007s            | 0.009s             | 0.092s             | 6.27 M              
woothee-php-1           | 0.008s            | 0.007s             | 0.05s              | 804.58 K            
zsxsoft-php-1           | 0.005s            | 0.066s             | 0.051s             | 970.41 K            |         

## Windows 10 system requirements

Enabling sqlite php extension.

Enabling wget command.

## How To Use

Clone this repo, then install composer dependencies (if composer isn't installed on your system, reference http://getcomposer.org):

`composer install`

Prepare the parsers and test suites:

`./bin/prepare`

This will initialize each of the parsers and test suites. This may require your system to have certain commands accessible on the path, like `composer` or `npm`.  This process will need to be improved to be more portable.

### Run a comparison:

`./bin/console compare`

This will prompt you to select which Test Suite to use, as well as which parser you want to test against (multiples of both can be selected). After the tests are performed, a normalization step takes place where the values of the different properties will be normalized to make comparison more relevant (special characters and spaces replaced, lower-cased, various mappings performed). After that, the results can be analyzed.

### Perform a benchmark:

`./bin/console benchmark path/to/file --iterations=5`

This will prompt you to select which parsers to benchmark, then run the parsers against the specified file (needs to contain the useragents to parse, separated by new lines). After the benchmark is performed, a table like the one above will be output showing the time taken for each parser, as well as the memory consumed (reported by the parser script itself).

### Parse some useragents:

`./bin/console parse path/to/file`

Parses the user-agents in the specified file and outputs a table showing the agent and the parsed properties. Can dump this output to CSV format.

## How Parsers are Compared

### Accuracy

Every parser extracts different pieces of data and formats/names the data differently, so comparing the output of one parser to another isn't an easy thing to do. Obviously you should pick a parser that extracts the data that **you** need, not the one that scores the best here.  That said, we did want a way to compare the relative accuracy of a parser when it's run against useragents for which the properties are known.  In order to do this across different parsers of varying "complete-ness" we selected just a handful of properties that are shared across most parsers we've looked at so far (Browser Name, Browser Version, Platform/OS Name, Platform/OS Version, Device Name, Device Brand, Device Type, Device "Is Mobile?" Flag).  This list is subject to change, and perhaps even something that we could make configurable for any given comparison.

Each of the property values are then normalized, mainly by lowercasing the value and stripping out special characters and spaces ("google-bot" and "Google Bot" would become just "googlebot"). Unfortunately that's not enough, as parsers will name some of the common values differently, so there's a mapping stage of normalization. This handles cases like "IE" and "Internet Explorer" referencing the same value.  This is done at the "data source" level, meaning, where the parser gets its useragent data from (some parsers use a general data source, like the data from the Browscap project, others use their own).  While this handles a lot of data mismatches, it doesn't deal with all of it, especially when we look at Device Name parsing.

For this reason, the report you see above was broken into different sections, showing how well a parser interpreted the Browser, Platform/OS and Device values.  Typically the Device section is the lowest accuracy for a parser (except against its own test suite) due to the inconsistencies in naming devices and because it contains four properties (that must match) instead of just two.

#### The "Accuracy Score"

To account for parsers (and test suites) that don't contain certain pieces of data, we award a "point" to a parser when it correctly identifies a property, but don't penalize the parser if it (as a whole) doesn't provide a property that the test suite it is run against does.  For example, the Woothee parser doesn't contain any device data except for the "type", so for the Device section it's only scored on whether or not it parsed the "type" property correctly. On the flip side of that, parsers aren't penalized for providing data that a test suite doesn't test for.

### Benchmarking

The benchmark command doesn't do anything special other than run a parser against a list of useragents a specified number of times. The timing information is collected by the script that runs the parser and the benchmark script, just in case there is a lot of time spent in application startup that can't be measured by the parser script itself (an example of this is PHP's native `get_browser` function. When PHP is configured to load a browscap.ini file, it reads the entire file at PHP startup, which can't be measured from within a PHP script). We've broken out the times so it's easy to tell where time is spent for any given parser. This includes an "initialization time" cost and the actual "parse time" for each useragent parsed. Any other costs that the runner script imposes that aren't measured as part of the other two times are measured in the "extra time" column.

## The Parsers

The parsers can be any useragent parser from any language (web API based parsers are possible, though rate limits and network traffic should be considered for some of the larger test suites). All this application needs from a parser is an executable script named "parse" that accepts a filename as the first parameter (which contains the useragents to parse, one on each line of the file) and returns the parsed data in the following JSON format:

```json
{
    "results": [
        {
            "useragent": "Mozilla/5.0 (iPad; U; CPU OS 3_2_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405",
            "parsed": {
                "browser": {
                    "name": "Mobile Safari UIWebView",
                    "version": "0.0"
                },
                "platform": {
                    "name": "iOS",
                    "version": "unknown"
                },
                "device": {
                    "name": "iPad",
                    "brand": "Apple Inc",
                    "type": "Tablet",
                    "ismobile": "true"
                }
            },
            "time": 0.003
        }
    ],
    "parse_time": 0.105,
    "init_time": 0.003,
    "memory_used": 3456745
}
```

Currently this is the list of parsers included:

#### Javascript
* 3rd-Eden Useragent 2.x (https://github.com/3rd-Eden/useragent)
* Browscap JS 1.x (https://github.com/mimmi20/browscap-js/tree/v1)
* Browscap JS 2.x (https://github.com/mimmi20/browscap-js)
* Device Detector for Nodejs 0.x (https://github.com/MiGatoSeneca/device-detector-node)
* UAParser.js (https://github.com/faisalman/ua-parser-js)
* WhichBrowser JS 0.x (https://github.com/WhichBrowser/Parser-JavaScript)
* Woothee JS 1.x (https://github.com/woothee/woothee-js)

#### PHP
 * Browscap PHP 2.x (https://github.com/browscap/browscap-php/tree/2.x)
 * Browscap PHP 3.x (Full, Standard and Lite) (https://github.com/browscap/browscap-php/tree/3.1.0)
 * Browscap PHP 4.x (Full) (https://github.com/browscap/browscap-php)
 * Crossjoin Browscap 1.x (https://github.com/crossjoin/Browscap/tree/1.x)
 * Crossjoin Browscap 2.x (https://github.com/crossjoin/Browscap/tree/2.x)
 * Crossjoin Browscap 3.x (PHP7 only) (https://github.com/crossjoin/Browscap/tree/3.x)
 * BrowserDetector 5.x (https://github.com/mimmi20/BrowserDetector)
 * Donatj UserAgent Parser 0.x (https://github.com/donatj/PhpUserAgent)
 * Endorphin Browser Detector 3.x (https://github.com/endorphin-studio/browser-detector)
 * Jenssegers Agent 2.x (https://github.com/jenssegers/agent)
 * PHP's Native `get_browser` (https://secure.php.net/get_browser) - It is **strongly** recommended that this parser isn't used for large useragent lists unless you're on PHP **7.0.15/7.1.1 or later**. It is **much** too slow otherwise.
 * Piwik Device Detector 3.x (https://github.com/piwik/device-detector)
 * Sinergi Browser Detector 6.x (https://github.com/sinergi/php-browser-detector)
 * UA Parser PHP 3.x (https://github.com/ua-parser/uap-php)
 * WhichBrowser 2.x (https://github.com/WhichBrowser/Parser)
 * Wolfcast BrowserDetection 2.x (https://github.com/Wolfcast/BrowserDetection)
 * Woothee PHP 1.x (https://github.com/woothee/woothee-php)
 * Yzalis UA-Parser 0.x (https://github.com/yzalis/UAParser)
 * ZSXSoft PHP-UserAgent 1.x (https://github.com/zsxsoft/php-useragent)

## The Test Suites

Like the parsers, there is no language requirement for a test suite to be included here.  The only requirement is that an executable script named `build` can be called and returns JSON in the following format:

```json
{
    "Mozilla/5.0 (iPad; U; CPU OS 3_2_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405": {
        "browser": {
            "name": "Mobile Safari UIWebView",
            "version": "0.0"
        },
        "platform": {
            "name": "iOS",
            "version": "unknown"
        },
        "device": {
            "name": "iPad",
            "brand": "Apple Inc",
            "type": "Tablet",
            "ismobile": "true"
        }
    }
}
```

Due to the nature of this format, it is a requirement that any given unique useragent has only one set of parsed fields. If a test suite contains different sets of values for a single useragent (across different files, or because other headers are expected to be present), then these either need to be excluded or merged together.

These are the test suites that are currently included:

 * Browscap (https://github.com/browscap/browscap)
 * Donatj (https://github.com/donatj/PhpUserAgent)
 * Endorphin (https://github.com/endorphin-studio/browser-detector)
 * Piwik Device Detector (https://github.com/piwik/device-detector)
 * Sinergi (https://github.com/sinergi/php-browser-detector)
 * UA Parser (https://github.com/ua-parser/uap-core)
 * UAParser.js (https://github.com/faisalman/ua-parser-js)
 * WhichBrowser (https://github.com/WhichBrowser/Parser)
 * Woothee (https://github.com/woothee/woothee)
 * ZSXSoft (https://github.com/zsxsoft/php-useragent)
