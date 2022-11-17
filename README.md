# Project: Parallel Web Crawler

### Abstract
I am interested in how popular terms on the internet could optimize the efficiency and user convinience of search engines.
However, legacy sequential web crawler is too slow for the purpose, so in this project I transformed a legacy web crawler to a multi-thread web crawler using the multi-core architecture. Furthermore, I developed a performance profiler to show that given the same amount of time, how much faster parallel web crawler is in comparison with sequential web crawler.

### Dependencies

  * Java JDK Version 17
  * Maven 3.6.3 or higher
  * IntelliJ IDEA

### Crawler Configuration

#### JSON Configuration Example

```
{
  "startPages": ["http://example.com", "http://example.com/foo"],
  "ignoredUrls": ["http://example\\.com/.*"],
  "ignoredWords": ["^.{1,3}$"],
  "parallelism": 4,
  "implementationOverride": "com.udacity.webcrawler.SequentialWebCrawler",
  "maxDepth": 10,
  "timeoutSeconds": 7,
  "popularWordCount": 3,
  "profileOutputPath": "profileData.txt"
  "resultPath": "crawlResults.json"
}
```
  * `startPages` - These URLs are the starting point of the web crawl.
  
  * `ignoredUrls` - A list of regular expressions defining which, if any, URLs should not be followed by the web crawler. In this example, the second starting page will be ignored.
  
  * `ignoredWords` - A list of regular expressions defining which words, if any, should not be counted toward the popular word count. In this example, words with 3 or fewer characters are ignored.
  
  * `parallelism` - The desired parallelism that should be used for the web crawl. If set to 1, the legacy crawler should be used. If less than 1, parallelism should default to the number of cores on the system.
  
  * `implementationOverride` - An explicit override for which web crawler implementation should be used for this crawl. In this example, the legacy crawler will always be used, regardless of the value of the "parallelism" option.
  
* `maxDepth` - The max depth of the crawl. The "depth" of a crawl is the maximum number of links the crawler is allowed to follow from the starting pages before it must stop. This option can be used to limit how far the crawler drifts from the starting URLs, or can be set to a very high number if that doesn't matter.
  

* `timeoutSeconds` - The max amount of time the crawler is allowed to run, in seconds. Once this amount of time has been reached, the crawler will finish processing any HTML it has already downloaded, but it is not allowed to download any more HTML or follow any more hyperlinks.
  
* `popularWordCount` - The number of popular words to record in the output. In this example, the 3 most frequent words will be recorded. If there is a tie in the top 3, word length is used as a tiebreaker, with longer words taking preference. If the words are the same length, words that come first alphabetically get ranked higher.
  
* `profileOutputPath` - Path to the output file where performance data for this web crawl should be 
. If there is already a file at that path, the new data should be appended. If this option is empty or unset, the profile data should be printed to standard output.
  
* `resultPath` - Path where the web crawl result JSON should be written. If a file already exists at that path, it should be overwritten. If this option is empty or unset, the result should be printed to standard output.


#### Example JSON Output

```
{
  "wordCounts": {
    "foo": 54,
    "bar": 23,
    "baz": 14
  },
  "urlsVisited": 12 
}
```
  * `wordCounts` - The mapping of popular words. Each key is a word that was encountered during the web crawl, and each value is the total number of times a word was seen.
     
    When computing these counts for a given crawl, results from the same page are _never_ counted twice.
  
  * `urlsVisited` - The number of distinct URLs the web crawler visited.
                  
    A URL is considered "visited" if the web crawler attempted to crawl that URL, even if the HTTP request to download the page returned an error.
                    
    When computing this value for a given crawl, the same URL is never counted twice.

#### Performance Profiler Example

```
Run at Fri, 18 Sep 2020 02:04:26 GMT
com.udacity.webcrawler.ParallelWebCrawler#crawl took 0m 1s 318ms
com.udacity.webcrawler.parser.PageParserImpl#parse took 0m 2s 18ms
```
The profile data should be written to a file defined by the `profileOutputPath` in the crawl configuration. 

### Commands

First, open a terminal of your operating system or IDE:

#### To check everything is working:

```
mvn test -Dtest=WebCrawlerTest -DcrawlerImplementations=com.udacity.webcrawler.SequentialWebCrawler
```

#### Running the Legacy Crawler

First, build the project:

```
mvn package -Dmaven.test.skip=true
```

You can run the legacy sequantial crawler using the sample configuration file:

```
java -classpath target/udacity-webcrawler-1.0.jar \
    com.udacity.webcrawler.main.WebCrawlerMain \
    src/main/config/sample_config_sequential.json
```

#### Run the Parallel Crawler

You can run the parallel web crawler with performance profiling with the following commands:

```
mvn package
java -classpath target/udacity-webcrawler-1.0.jar \
    com.udacity.webcrawler.main.WebCrawlerMain \
    src/main/config/sample_config.json
```
