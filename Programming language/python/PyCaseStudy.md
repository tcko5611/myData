# Find usolved symbol
# Introduction
read input file from argv[1], and compare file argv[2].

For each line in input, strip the line and add a '@' to the tail

find that symbol in compar file(from beginning), it not dump to a output file for symbol not find
# Algorithm
## main algorithm

```python
if __name__ == '__main__':
  out = open('unsolvedSymbol', 'w')
  with open(sys.argv[1]) as fp:
    line = fp.readline().rstrip()
    string = line + '@'
    while line:
      if inFile(string, sys.argv[2]) :
        print(line + ' In File')
      else :
        out.write(line + '\\n')
        print(line + ' Not in File')
      line = fp.readline().rstrip()
      string = line + '@'
  out.close()
```

## function for look string in file or not
```python
def inFile(str1, fileName): 
  with open(fileName) as fp: 
    line = fp.readline().rstrip() 
    while line: a = re.match(str1, line) # use match not find for match start from beginning 
      if a != None : 
        return True 
      line = fp.readline().rstrip() 
  return False
``````