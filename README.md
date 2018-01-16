# lambda-logger

Logger useful for AWS lambda applications, particularly those which are aggregated in Splunk. Logs in JSON format using [pino](https://github.com/pinojs/pino).

This was created to provide a simple logger, compatible with lambda, which outputs in a JSON format ([n-logger](https://github.com/Financial-Times/n-logger)) was previously used but didn't handle nested JSON fields or provide a JSON option).

[![CircleCI](https://circleci.com/gh/Financial-Times/lambda-logger.svg?style=svg&circle-token=95d28799bf7519d6c9628cb0cdb053f08ff9ff30)](https://circleci.com/gh/Financial-Times/lambda-logger) [![Coverage Status](https://coveralls.io/repos/github/Financial-Times/lambda-logger/badge.svg?branch=master)](https://coveralls.io/github/Financial-Times/lambda-logger?branch=master)

## API

The logger's API is identical to that of pino with the following exceptions:

* The property `sourcetype: _json` is added to logs in production for Splunk compatibility.
* Lambda related environment variables are added by default:
  * `AWS_EXECUTION_ENV`,
  * `AWS_LAMBDA_FUNCTION_NAME`,
  * `AWS_LAMBDA_FUNCTION_MEMORY_SIZE`,
  * `AWS_LAMBDA_FUNCTION_VERSION`
* Defaults to ISO timestamp logging for splunk compatiblity. At the time of writing this incurs a 25% pino performance penalty.

### Pino properties

Pino adds the following properties to logs by default:

* `level` - the log level in string form. This is translated from the `pino` default of logging an integer representation.
* `v` - the pino logger API version.
* `hostname` - the hostname the process is running on.
* `pid` - the process PID.

## Configuration

* `NODE_ENV` - pretty printing is enabled when `NODE_ENV !== 'production'`.
* `CONSOLE_LOG_LEVEL` - determines the level to log at (pinto `level` option).
* `SYSTEM_CODE` - adds the `systemCode` property to every log.
* `ENVIRONMENT|STAGE` - adds the `environment` property to every log. `STAGE` is used as a fallback due to it's default definition in the [serverless](https://serverless.com) framework.
