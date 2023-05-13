wi# Json dump and read from file
``` python
import json
with open(fileName) as f:
	self.data_ = json.load(f)
with open(fileName, 'w') as f:
      f.write(json.dumps(self.data_, indent=4))
```