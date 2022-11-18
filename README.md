Scrub Personal Identifying Information Problem
==================================================
Application event tracking to external services often involves meta data about the user who took the action. When this event information is sent to a third party, we want to scrub all personally identifying data reported, without losing information about what fields were actually recorded.

In order to do this on different platforms, it is useful to implement a method called `scrub()` that will take a JSON object and an array of sensitive fields as an input and returns a modified JSON object that replaces alphanumeric characters with an asterisk ("\*") from values corresponding to keys matching the sensitive field names.

For example, if the sensitive fields we want to "scrub" are `name`, `phone`, and `email`, a JSON input of:

```
{
  "name": "Kelly Doe",
  "email": "kdoe@example.com",
  "id": 12324,
  "phone": "5551234567"
}
```

should return:

```
{
  "name": "***** ***",
  "email": "****@*******.***",
  "id": 12324,
  "phone": "**********"
}

```

## Requirements
You will need to implement a command line executable that takes two arguments: a text file with a list of sensitive fields and a JSON file of user data to scrub. Calling `./scrub sensitive_fields.txt input.json ` should output a scrubbed JSON version of `input.json` with the keys in `sensitive_fields.txt` "scrubbed".

Any valid JSON input should be able to be handled. Value types for sensitive keys should be handled as follows:
  - `String`: replace alphanumeric characters with "*"
  - `Number`: convert to string and replace alphanumeric characters with "*"
  - `Boolean`: replace entire value with "-"
  - `Array`: each value of the array should be evaluated as described by other field types
  - `Object`: if the key matches a sensitive field, all values of the nested object should be scrubbed as described by other field types. If the nested object does not correspond to a sensitive key, each key/value pair of the nested object should be evaluated as described by other field types
  - `null`: value should be unmodified

## Tests

A handful of example test "scrubs" are in the `/tests/` directory. Each subdirectory has an `input.json`, `sensitive_fields.txt`, and corresponding `output.json` that is expected.

Tests can be run using the basic `test_runner` script, with the following command:
```sh
$ ./test_runner
```
This should run the `scrub` script using data in sub-directories of the `tests` directory, checking that the `output.json` data from the script matches the expected data found in the `output.json` file of each test sub-directory.

## Running `scrub` script

The `scrub` script accepts two positional arguments, one for the filepath to the sensitive fields file, and one for the filepath to the input JSON data.

The following command will run the `scrub` script with a `sensitive_fields.txt` file and `input.json` file found in the current directory, which should be the same location as the `scrub` file.
```sh
$ ./scrub sensitive_fields.txt input.json
```
