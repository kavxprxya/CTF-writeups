# Psst

![date](https://img.shields.io/badge/date-07.08.2021-brightgreen.svg)
<!-- Date you solved the question -->

![category](https://img.shields.io/badge/category-Misc-lightgrey.svg)
<!-- Category of the question -->

![score](https://img.shields.io/badge/score-159/500-blue.svg)
<!-- Score of the question / Max Score possible in any question -->

## Description

Psst! Want to know a secret? Here, take this...
[chall file](https://storage.googleapis.com/noida_ctf/Misc/psst.tar.gz)

## Summary
Extracted the data from the file and decoded it to string.



## Detailed solution
This is the code to extract the data from the file given and decode it. 
```py 
import os
for root, subdirs, files in os.walk('.'):#assuming executes from chall/
    for filename in files:
        file_path = os.path.join(root, filename)
        if('DS_Store' not in file_path and '.py' not in file_path):
            with open(file_path, 'rb') as file:
                try:
                    print(file.read().decode("utf-8").rstrip(), end='')
                except Exception:
                    pass
```


## Flag

```
BSNoida{d1d_y0u_u53_b45h_5cr1pt1ng_0r_5t4ck0v3rfl0w?}
```
