# Related 
## cmake
[[cmake_basic#Flex and Bison]]

# Example for simple parser

Suppose we want to parse the following text

```
Freq: avgfreq(v(/FOUT, analysisName=\"tran\"))
FvsT: fvst(v(/FOUT, analysisName=\"tran\"))
Eye_obj: create_eye(v(/FOUT, analysisName=\"tran\"),ui_type=auto)
Eye_opening: measure_eyesize(Eye_obj,0.4)
Jitter_P2P: measure_aperture(o(Eye_obj, analysisName=\"tran\"),0.4)
```

And we to use parser as exprParse, then we can use the following code to do:

## main.cpp

```cpp
#include <cstdio>
#include <iostream>
#include <sstream>
#include <fstream>
#include "utils.h"
using namespace std;


int main(int argc, char**argv) {
  string fN(argv[1]);
  ifstream infile(fN);
  stringstream ss;
  for (string line; getline(infile, line);) {
    ss << line << "\n";
  }
  map<string, string> m = buildMap(ss.str());
  for (auto val: m) {
    cout << "key: " << val.first << ", value: " << val.second << endl; 
  }
  return 0;
}
```

## utils.h
```cpp
#ifndef UTILS_H /* -*-c++-*- */
#define UTILS_H
#include <map>
#include <string>
std::map<std::string, std::string> buildMap(const std::string &fN);
#endif /* UTILS_H */
```
# Flex
## scanner.ll


```cpp
%{
  #include <cstdio>

  #include "tokens.h"  // to get the token types from Bison 
  int line_num = 1;
  #define yylval exprlval // define for tokens.h will not declare yylval
%}
%option noyywarp
// define patterns
dseq            ([[:digit:]]+)
dseq_opt        ({dseq}?)
frac            (({dseq_opt}"."{dseq})|{dseq}".")
exp             ([eE][+-]?{dseq})
exp_opt         ({exp}?)
integer         ({dseq})
floa           (({frac}{exp_opt})|({dseq}{exp}))
number          ({floa}|{integer})
intvar          ([[:upper:]])
fltvar          ([[:lower:]])
var ([[:alnum:]\\"_\/]+) // \\ for \, \/ for / " for \" 

%%
[ \t]       {    ;}
"(" { return '('; }
")" { return ')'; }
":" { return ':';}
"=" { return '='; }
"," { return ',';}
{number} { yylval.sval = strdup(yytext); return NUMBER; }
{var} { yylval.sval = strdup(yytext); return VAR; }
\n { ++line_num; return ENDL;}
%%
```

## include syntax in file

For file like veriloga or c, there is a include statement that need to read another file in that point, we can use the following scheme to read the cotain of include file.

```cpp
%{
#include <math.h>
#include <vector>
#include <iostream>
  using namespace std;
  /* vector to store buffer states, act like stack*/
  std::vector<YY_BUFFER_STATE> include_stack; 
%}
%x incl /* incl condition */
%%

include { BEGIN(incl); /* start incl condition */ }

[a-z]+ {ECHO; /* just echo test code */ 
[^a-z\\\\n]*\\\\n? { ECHO; /* just echo test code */ }
<incl>[ \\\\t]* /* eat the whitespace */
<incl>[^ \\\\t\\\\n]+ { /* get the file name in yytext */
  /* push YY_CURRENT_BUFFER to stack */ 
  include_stack.push_back(YY_CURRENT_BUFFER);
  yyin = fopen(yytext, "r");
  if (!yyin) {
    cout << "error for open file:" << yytext << endl;
    exit(1);
  }
  /* swith to new BUFFER */
  yy_switch_to_buffer(yy_create_buffer(yyin, YY_BUF_SIZE));
  /* from the beginning of the buffer set condition to initial */
  BEGIN(INITIAL);
 }
<<EOF>> {
  /* when encounter end of file, check if stack has buffer, if not then complete,
     else get the next state from stack and clear the last buffer
  */
  if (include_stack.empty()) {
    yyterminate();
  }
  else {
    yy_delete_buffer(YY_CURRENT_BUFFER);
    yy_switch_to_buffer(include_stack.back());
    include_stack.pop_back();
  }
  }
%%
int main (int argc, char *argv[])
{
  yyin = fopen("test.txt", "r");
  while(yylex());
  return 0;
}

```
## flex inclusive and exclusive start condition
``` cpp
         %s example
         %%
         <example>foo   do_something();
         bar            something_else();
```
is equivalent to
```cpp
         %x example
         %%
         <example>foo   do_something();
         <INITIAL,example>bar    something_else();
```
## input for flex
### file
```cpp
FILE *myfile = fopen("your_file", "r");
yyin = myfile;
```
### string
```cpp
    yy_scan_buffer("a test string");
    yylex();
```
### from stdin
```cpp
yylex();
```
## command
```bash
flex --header-file=cfwvexprflex.h --prefix=cfwvexpr --outfile=scanner.cpp scanner.ll
```
# Bison
## paser.yy

```cpp
%{
  #include <cstdio>
  #include <iostream>
  #include <string>
  #include <map>
  #include <cstring>
  #include "util.h"
  #include "cfwvexprflex.h"
  
  using namespace std;

  // Declare stuff from Flex that Bison needs to know about:
  extern int yylex();
  extern int yyparse();
 
  void yyerror(const char *s);
  static map<string, string> equanMap_;
  extern int line_num;
%}

// Bison fundamentally works by asking flex to get the next token, which it
// returns as an object of type "yystype".  Initially (by default), yystype
// is merely a typedef of "int", but for non-trivial projects, tokens could
// be of any arbitrary data type.  So, to deal with that, the idea is to
// override yystype's default typedef to be a C union instead.  Unions can
// hold all of the types of tokens that Flex could return, and this this means
// we can return ints or floats or strings cleanly.  Bison implements this
// mechanism with the %union directive:
%union {
  int ival;
  float fval;
  char *sval;
}

// Define the "terminal symbol" token types I'm going to use (in CAPS
// by convention), and associate each with a field of the %union:
%token ENDL
%token <ival> INT
%token <fval> FLOAT
%token <sval> VAR NUMBER
%type <sval> func

%%
// This is the actual grammar that bison will parse, but for right now it's just
// something silly to echo to the screen what bison gets from flex.  We'll
// make a real one shortly:
statements:
statements statement
| statement
;

statement:
VAR ':' func ENDLS {
  equanMap_[$1] = $3;
  //  cout << $1 << ":" << $3 << endl;
}
;
func :
VAR '(' VAR ',' NUMBER ')' {
  string s;
  if (!equanMap_.count($3)) {
    cerr << $3 << "can't find its definition\n";
    exit(-1);
  }
  s = string($1) + "(" + equanMap_[$3] + "," + $5 + ")";
  $$ = strdup(s.c_str());
}
| VAR '(' VAR ',' assignment ')' {
  string s;
  if (!strcmp($1, "v")) {
    s = string($1) + "(" + $3 + ")";
    $$ = strdup(s.c_str());
  }
  else {
    if (!equanMap_.count($3)) {
      cerr << $3 << "can't find its definition\n";
      exit(-1);
    }
    s = string($1) + "(" + equanMap_[$3] + ")";
    $$ = strdup(s.c_str());
  }
}
| VAR '(' VAR ')' {
  string s;
  if (!strcmp($1, "v")) {
    s = string($1) + "(" + $3 + ")";
    $$ = strdup(s.c_str());
  }
  else {
    if (!equanMap_.count($3)) {
      cerr << $3 << "can't find its definition\n";
      exit(-1);
    }
    s = string($1) + "(" + equanMap_[$3] + ")";
    $$ = strdup(s.c_str());
  }
}
| VAR '(' func ',' assignment ')' {
  string s;
  s = string($1) + "(" + $3 + ")";
  $$ = strdup(s.c_str());
}
| VAR '(' func ',' NUMBER ')' {
  string s;
  s = string($1) + "(" + $3 + "," + $5 + ")";
  $$ = strdup(s.c_str());
}
| VAR '(' func ')' {
  string s;
  s = string($1) + "(" + $3 + ")";
  $$ = strdup(s.c_str());
}
;

assignment:
VAR '=' VAR {
}
;

ENDLS:
ENDLS ENDL
| ENDL
;

%%
/* define this file when error occur */
void yyerror(const char *s) {
  cout << "EEK, parse error!  In Line number: " << line_num
       << "! msg :" << s << endl;
  // might as well halt now:
  exit(-1);
}

map<string, string> buildMap(const string &str) {
  equanMap_.clear();
  // FILE *myfile = fopen(fN.c_str(), "r");
  // exprin = myfile;
  cfwvexpr_scan_string(str.c_str());
  yyparse();
  return equanMap_;
}
```

## command

```bash
bison --defines=cfwvexprtokens.h --output parser.cpp --name-prefix=cfwvexprexpr parser.yy

```

