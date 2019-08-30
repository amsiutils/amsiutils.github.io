---
layout: post
title:  "JSON Dataframe to Excel with Python"
date:   2019-08-30 00:00:00 -0400
categories: 
---

#### Requirements


```
pip3 install openpyxl pandas
```

#### Convert


```
import pandas as pd
import json
data = json.load(open('computers.json'))
computers = pd.DataFrame(data["computers"])
computers.to_excel("output.xlsx")
```
