# Args Parser

# Basic

```python
import argparse
parser = argparse.ArgumentParser(description='import file')
parser.add_argument('-file', metavar='datafile', type=str, required=True, \\
help='plot data file')
args = parser.parse_args()
print(args.file)

```

# add_argument

-   _--file_ and _-f_ can be used in options

```python
import argparse
parser = argparse.ArgumentParser(description='import file')
parser.add_argument('--file', '-f', metavar='datafile', type=str, required=True,
help='plot data file')

```

-   _nargs_ for number of element for the option

```python
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', nargs=2)
>>> parser.add_argument('bar', nargs=1)
>>> parser.parse_args('c --foo a b'.split())
Namespace(bar=['c'], foo=['a', 'b'])

```

-   _type_ include _int_, _open_, _str_, _float_

```python
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('foo', type=int)
>>> parser.add_argument('bar', type=open)
>>> parser.parse_args('2 temp.txt'.split())

```

-   _required_

```python
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', required=True)
>>> parser.parse_args(['--foo', 'BAR'])
Namespace(foo='BAR')
>>> parser.parse_args([])
usage: argparse.py [-h] [--foo FOO]
argparse.py: error: option --foo is required

```

-   _metavar_ for help information

```python
>>> parser.add_argument('--foo', metavar='YYY')
>>> parser.add_argument('bar', metavar='XXX')
>>> parser.parse_args('X --foo Y'.split())
Namespace(bar='X', foo='Y')
>>> parser.print_help()
usage:  [-h] [--foo YYY] XXX

positional arguments:
 XXX

optional arguments:
 -h, --help  show this help message and exit
 --foo YYY
```

# Parse_args()

```python
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('-x')
>>> parser.add_argument('--foo')
>>> parser.parse_args(['-x', 'X'])
Namespace(foo=None, x='X')
>>> parser.parse_args(['--foo', 'FOO'])
Namespace(foo='FOO', x=None)

```