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
We downloaded the file and we found out that there were nested directories containing txt files .
Each of the txt files had a single character of the flag in the order of the directories.
We attempted to open each directory and figure out the flag but we faced issues.
After which, we tried to solve it by code.

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
This is what the output was:
```
BSNoida{d1d_y0u_u53_b45h_5cr1pt1ng_6f7220737461636b6f766572666c6f773f}
```

On decoding the hex string, we got the flag.




## Flag

```
BSNoida{d1d_y0u_u53_b45h_5cr1pt1ng_0r_5t4ck0v3rfl0w?}
```
