# ads.txt crawler [![Build Status](https://travis-ci.org/infectious/adstxtcrawler.svg?branch=master)](https://travis-ci.org/infectious/adstxtcrawler)

### Installation

    pip install git+https://github.com/botlabio/adstxtcrawler.git

This crawler relies on you having the latest Python installed and accessible on your path.  You'll need to setup a virtual environment path to isolate deps.

### Use

The usecase is where you have ae list of sites, and the crawler will check for ads.txt file in each of the sites in the list, and then store the results into a db.

    # Setup a virtualenv to use and activate it
    python3.7 -m venv adstxt_env
    source adstxt_env/bin/activate

Data can be written to MariaDB or MySQL based on the db_uri parameter. The simplest example is to use sqlite:

    adstxt --file --file_path=/tmp/adstxt_domains --db_uri=sqlite:///name_of_file.db --crawler_tag=name_of_your_crawler

This will start up the crawler, using the file argument and the specified path.

### Performance

The initial buildtime depends on the number of sites. Roughly speaking, several thousand sites may be processed per minute.

### Configuration

Configuration is done either through CLI paramaters or using environment variables.  Required configuration is variable dependent upon configuration. The crawler currently either takes a single column CSV of domains, or an elasticsearch query which can return a list of domains.

#### Configuration paramaters

| Configurable                    | Environment Variable  | Explanation                                                                           |
| ------------------------------- | --------------------- | ------------------------------------------------------------------------------------- |
| Database URI                    | ADSTXT_DB_URI         | Database URL to write to.                                                             |
| File path                       | ADSTXT_FILE_PATH      | File path of CSV of domains to scan (only used when file argument is given).          |
| Elasticsearch URI               | ADSTXT_ES_URI         | Elasticsearch URL to read data from (only used when elasticsearch argument is given). |
| Elasticsearch query             | ADSTXT_ES_QUERY       | Elasticsearch query to read data from (needs to return a valid result as per docs).   |
| Crawler Tag                     | ADSTXT_CRAWLER_TAG    | Unique identifier that's added to user agent to identify crawler.                     |
| Log level                       | ADSTXT_LOG_LEVEL      | Log level to run at, defaults to info                                                 |
| Log formatter                   | ADSTXT_LOG_FORMATTER  | Log formatter to write output as, takes normal python logging format.                 |
| Sentry URI                      | SENTRY_DSN            | Sentry URL to write any exception data to.                                            |
| Git Hash                        | GIT_HASH              | Git version to report sentry exceptions as.                                           |

### Developing

Setup a new virtualenv, and run the following to get started.

```sh
python setup.py develop
mypy adstxt/main.py --ignore-missing-imports --strict-optional
# Run tests including integration suite.
pytest -vv --integration
```

All tests should return green.  Please don't open a PR with failing tests
unless you're unsure why they're failing.


### Models

SQLAlchemy Models are available via [models](./adstxt/models.py).

These are importable and can be used instead of writing raw sql upstream.


### Bugs

Please submit any bugs you find here, we've done our best efforts to not
include any, but I'm sure some exist.


### Missing features

1. Async database support will greatly improve the throughput vs writing to the
    database in a single background thread.
2. Postgres support. 
3. Statsd integration, there's very little monitoring currently.  This is soon
    to come.
