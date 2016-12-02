* 配置日志设置

```

---

 

version: 1

 

disable_existing_loggers: False

 

formatters:

 

    simple:

 

        format: "%(asctime)s - %(name)s - %(levelname)s : %(message)s"

 

handlers:

 

    console:

 

        class: logging.StreamHandler

 

        level: DEBUG

 

        formatter: simple

 

        stream: ext://sys.stdout

 

    info_file_handler:

 

        class: logging.handlers.RotatingFileHandler

 

        level: INFO            

 

        formatter: simple

 

        filename: /tmp/1688.log

 

        maxBytes: 10485760 # 10MB

 

        backupCount: 20

 

        encoding: utf8

 

    error_file_handler:

 

        class: logging.handlers.RotatingFileHandler

 

        level: ERROR            

 

        formatter: simple

 

        filename: /tmp/1688.log

 

        maxBytes: 10485760 # 10MB

 

        backupCount: 20

 

        encoding: utf8

 

loggers:

 

    alibaba:

 

        level: DEBUG

 

        handlers: [console, info_file_handler, error_file_handler]

 

    root:

 

        level: INFO

 

        handlers: [console, info_file_handler, error_file_handler]

 

...

```

* 格式设置函数

```

#!/usr/bin/env python

# -*- coding: utf-8 -*-

#

# author: wanqiang

# description: ""





import logging.config

import os

import logging

import yaml





def setup_logging(

    default_path='logging.yaml',

    default_level=logging.INFO,

    env_key='LOG_CFG'

):

    """Setup logging configuration



    """

    path = default_path

    value = os.getenv(env_key, None)

    if value:

        path = value

    if os.path.exists(path):

        print("read config successfully")

        with open(path, 'rt') as f:

            config = yaml.load(f.read())

        logging.config.dictConfig(config)

    else:

        print("read config failed")

        logging.basicConfig(level=default_level)





def main():

    setup_logging()

    logger = logging.getLogger("root")

    logger.info("你好")

    logger.error("卧槽")





if __name__ == '__main__':

    main()

```
