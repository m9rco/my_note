```
# -*- coding: utf-8 -*-

import tarfile
import os

with tarfile.open("testtarfile.tar", "w") as tar:
    for root, dir, files in os.walk('/home/tt67wq/code/test'):
        for file in files:
            fullpath = os.path.join(root, file)
            tar.add(fullpath, arcname=file)
```
