# xmindparser

[![PyPI](https://img.shields.io/pypi/v/xmindparser.svg)](https://pypi.org/project/xmindparser/)

Parse xmind file to programmable data type (e.g. json, xml). Python 3.x required. Now it supports XmindZen file type as well.

See also: [xmind2testlink](https://github.com/tobyqin/xmind2testlink) / [中文文档](https://betacat.online/posts/2018-07-01/parse-xmind-to-programmable-data-type/)

## Installation

```shell
pip install xmindparser
```

## Usage - Command Line

```shell
cd /your/xmind/dir

xmindparser your.xmind -json
xmindparser your.xmind -xml
```

Note: Parse to xml file type require [dicttoxml](https://pypi.org/project/dicttoxml/).

## Usage - via Python

```python
from xmindparser import xmind_to_dict

d = xmind_to_dict('/path/to/your/xmind')
print(d)
```

See example output: [json](doc/example.json)

## Configuration

If you use `xmindparser` via Python, it provides a `config` object, check this example:

```python
import logging
from xmindparser import xmind_to_dict,config

config = {'logName': 'your_log_name',
          'logLevel': logging.DEBUG,
          'logFormat': '%(asctime)s %(levelname)-8s: %(message)s',
          'showTopicId': True, # internal id will be included, default = False
          'hideEmptyValue': False  # empty values will be hidden, default = True
          }

d = xmind_to_dict('/path/to/your/xmind')
print(d)

```

## Limitations (for Xmind legacy version)

Please note, following xmind features will not be supported or partially supported.

- Will not parse Pro features, e.g. Task Info, Audio Note
- Will not parse floating topics.
- Will not parse linked topics.
- Will not parse summary info.
- Will not parse relationship info.
- Will not parse boundary info.
- Will not parse attachment object, only name it as `[Attachment] - name`
- Will not parse image object, only name it as `[Image]`
- Rich text format in notes will be parsed as plain text.

## XmindZen Supporting

`xmindparser` will auto detect xmind file created by XmindZen or XmindPro, you can pass in ZEN file as usual.

```python
from xmindparser import xmind_to_dict

d = xmind_to_dict('/path/to/your/xmind_zen_file')
print(d)
```

Please note, there are a few differences between xmind legacy and xmind zen.

- Comments feature removed, so I will not parse it in ZEN.
- Add feature - sticker, I parse it as `image` dict type.
- Add feature - callout, I parse it as `list` type. (not sure existed in lagacy?)

Since XmindZen has upgraded the internal content file as json, you can read it by code like this:

```python
import json

def get_xmind_zen_json(file_path):
    name = "content.json"
    with ZipFile(file_path) as xmind:
        if name in xmind.namelist():
            content = xmind.open(name).read().decode('utf-8')
            return json.loads(content)

        raise AssertionError("Not a xmind zen file type!")

# xmindparser also provides a shortcut
from xmindparser import get_xmind_zen_builtin_json

content_json = get_xmind_zen_builtin_json(xmind_zen_file)
```

## Examples

![Xmind Example](doc/xmind.png)

- [Download xmind pro example](tests/xmind_pro.xmind)
- [Download xmind zen example](tests/xmind_zen.xmind)
- Output: [json example](doc/example.json)
- Output: [xml example](doc/example.xml)

## License

MIT
