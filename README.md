# User Agent Parser Comparison

This application was put together to assist us in making a decision on which UserAgent parsing library to use, and then modified some to be a bit more user friendly to encourage use among the different parsing library maintainers.  It's still a bit work in progress, but in its current state it's very useful at benchmarking the different parsing libraries (across different languages) and comparing the results of the parsers against "known" useragents (right now these come from the parser libraries' unit tests, but a known list could be built and tested against).

This is loosely based on a benchmark utility that has been around for a while (but not currently maintained): https://github.com/kenjis/user-agent-parser-benchmarks

Right now this application is entirely a CLI application. Most of the data generated by the application is stored as JSON files, so it's certainly possible that a Web UI could be built around the data files which might make analysis easier.

The CLI is written in PHP (using the Symfony Console component), but the parsers (and tests) can be written in any language. Currently just PHP parsers are included, but the structure of the application (and how it calls the parsers) was intentionally built to support parsers from any language (PRs welcome!).

## Latest "Accuracy" Results

Parser                  | Browser Results    | Platform Results   | Device Results     | Time Taken | Accuracy Score
-----|-----|-----|-----|-----|-----
browscap-js-1           | 36922/46375 79.62% | 43999/46375 94.88% | 23092/46375 49.79% | 1403.217s  | 118963/181899 65.4%
browscap-php-2          | 36983/46375 79.75% | 44008/46375 94.9%  | 23092/46375 49.79% | 3211.344s  | 119146/181899 65.5%
browscap-php-3          | 36923/46375 79.62% | 43998/46375 94.87% | 23092/46375 49.79% | 996.962s   | 118962/181899 65.4%
crossjoin-1             | 36926/46375 79.62% | 43998/46375 94.87% | 23093/46375 49.8%  | 1037.01s   | 118966/181899 65.4%
crossjoin-2             | 36844/46375 79.45% | 44006/46375 94.89% | 22921/46375 49.43% | 2549.295s  | 118478/181899 65.13%
crossjoin-3             | 36844/46375 79.45% | 44006/46375 94.89% | 22921/46375 49.43% | 2139.953s  | 118478/181899 65.13%
piwik-device-detector-3 | 38840/46375 83.75% | 42910/46375 92.53% | 32771/46375 70.67% | 438.676s   | 146516/181899 80.55%
ua-parser-php-3         | 38974/46375 84.04% | 42279/46375 91.17% | 35776/46375 77.15% | 212.557s   | 121488/150929 80.49%
whichbrowser-2          | 39789/46375 85.8%  | 44668/46375 96.32% | 29832/46375 64.33% | 202.637s   | 150334/181896 82.65%
woothee-php-1           | 31620/46375 68.18% | 42251/46375 91.11% | 39695/46375 85.6%  | 6.266s     | 70444/109753 64.18%

## Latest Benchmark Results (files/ua-list-all.txt, 437 useragents)

Parser                  | Average Init Time | Average Parse Time | Average Memory Used
-----|-----|-----|-----
browscap-js-1           | 0.032s            | 12.257s            | 115.47 M
browscap-php-2          | 1.062s            | 20.741s            | 122.86 M
browscap-php-3          | 0.072s            | 7.394s             | 4.97 M
crossjoin-1             | 0.036s            | 5.627s             | 2.26 M
crossjoin-2             | 0.025s            | 9.472s             | 2.5 M
crossjoin-3             | 0.051s            | 8.962s             | 1.86 M
piwik-device-detector-3 | 0.859s            | 3.973s             | 4.72 M
ua-parser-php-3         | 0.038s            | 1.626s             | 2.57 M
whichbrowser-2          | 0.058s            | 1.781s             | 20.3 M
woothee-php-1           | 0.034s            | 0.059s             | 2.28 M

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

The benchmark command doesn't do anything special other than run a parser against a list of useragents a specified number of times. The timing information is collected by the script that runs the parser.  This includes an "initialization time" cost and the actual "parse time" for each useragent parsed. Any other costs that the runner script imposes isn't included in these times (like opening and parsing the text file that contains the useragents).

However, the memory usage does (currently) include the cost of building an array that is output (mainly for the comparisons). This should be able to be removed easily (at least for the benchmarking command). It's still useful as a relative comparison, but total memory used will grow as the size of the useragent list grows (for every parser). It is not known yet if other languages provide a similar and comparable metric.

## The Parsers

The parsers can be any useragent parser from any language (web API based parsers are possible, though rate limits and network traffic should be considered for some of the larger test suites). All this application needs from a parser is an executable script named "parse" that accepts a filename as the first parameter (which contains the useragents to parse, one on each line of the file) and returns the parsed data in the following JSON format:

```json
{
    "results": [
        {
            "user_agent": "Mozilla 5.0",
            "parsed": {
                // The expected fields (not-normalized)
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

 * Browscap JS 1.x (https://github.com/mimmi20/browscap-js)
 * Browscap PHP 2.x (https://github.com/browscap/browscap-php/tree/2.x)
 * Browscap PHP 3.x (https://github.com/browscap/browscap-php)
 * Crossjoin Browscap 1.x (https://github.com/crossjoin/Browscap/tree/1.x)
 * Crossjoin Browscap 2.x (https://github.com/crossjoin/Browscap/tree/2.x)
 * Crossjoin Browscap 3.x (PHP7 only) (https://github.com/crossjoin/Browscap/tree/3.x)
 * Piwik Device Detector 3.x (https://github.com/piwik/device-detector)
 * UA Parser PHP 3.x (https://github.com/ua-parser/uap-php)
 * WhichBrowser 2.x (https://github.com/WhichBrowser/Parser)
 * Woothee PHP 1.x (https://github.com/woothee/woothee-php)

## The Test Suites

Like the parsers, there is no language requirement for a test suite to be included here.  The only requirement is that an executable script named `build` can be called and returns JSON in the following format:

```json
{
    "Mozilla 5.0": {
        // The expected fields (not normalized, should match the parser output)
    }
}
```

Due to the nature of this format, it is a requirement that any given unique useragent has only one set of parsed fields. If a test suite contains different sets of values for a single useragent (across different files, or because other headers are expected to be present), then these either need to be excluded or merged together.

These are the test suites that are currently included:

 * Browscap (https://github.com/browscap/browscap)
 * Piwik Device Detector (ttps://github.com/piwik/device-detector)
 * UA Parser (https://github.com/ua-parser/uap-core)
 * WhichBrowser (https://github.com/WhichBrowser/Parser)
 * Woothee (https://github.com/woothee/woothee)
