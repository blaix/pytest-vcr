# Configuration

This plugin is configured, for the most part, by overriding py.test fixtures
or passing options to the vcr marker.

# Command line options

## --vcr-record-mode

Selects the [VCR.py record mode](http://vcrpy.readthedocs.io/en/latest/usage.html#record-modes).
Useful in CI (where you want --vcr-record-mode=none).

# Configuration fixtures

## vcr_config

Default: `{}`

Additional arguments for the [VCR constructor](http://vcrpy.readthedocs.io/en/latest/configuration.html#configuration), as a dictionary.
This will be overridden by marker and command-line options.

## vcr_cassette_path

Default: `test_file_directory + "/cassettes/" + vcr_cassette_name`

Path to current test's cassette.

## vcr_cassette_name

Default: name of the test

Name of the current test's cassette

## vcr

Default: an instance of `VCR`

This is the instance used for creating cassettes.
You'd override this fixture to register your own matchers, serializers or persisters, for example:

```python
@pytest.fixture
def vcr(vcr):
    vcr.register_matcher('my_matcher', my_matcher)
    vcr.match_on = ['my_matcher']  # This can also go into vcr_config or marker kwargs
    return vcr
```


# Marker options
All options provided on the marker will be passed to the VCR constructor, for example:

```python
@pytest.mark.vcr(ignore_localhost=True)
def test_local_and_remote():
    # This one will replay from the cassette
    urlopen('http://www.iana.org/domains/reserved').read()
    # This one will always be downloaded
    urlopen('http://127.0.0.1/').read()
```
