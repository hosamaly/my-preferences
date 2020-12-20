This guide describes how to add some basic syntax coloring for
[jq](https://stedolan.github.io/jq)
files in JetBrains IDEs.
It is based on [jq v1.6](https://stedolan.github.io/jq/manual/v1.6/).

I copied the keywords list from
[vito-c/jq.vim](https://github.com/vito-c/jq.vim). Thanks!

The keyword lists were generated using the following jq commands:
```sh
# get the sorted list of functions
jq --null-input --raw-output 'builtins | sort | .[]'

# produce lists of functions by the number of their arguments
functions_by_arity='
  builtins
  | map(split("/"))
  | map({ function: .[0], arity: .[1] })
  | reverse  # let `unique_by` return the first arity
  | unique_by(.function)
  | group_by(.arity)
  | map({
      arity: (. | first | .arity),
      functions: (. | map(.function) | sort)
    })
  | sort_by(.arity)
'
jq --null-input $functions_by_arity

# get a list of functions with a particular arity
arity_filter='map(select(.arity == $arity) | .functions) | flatten | .[]'
jq --null-input --raw-output --arg arity 1 "$functions_by_arity | $arity_filter"
```

To configure syntax highlighting in JetBrains IDEs:
* Open the preferences dialog and navigage to Editor > File Types
* Add a new file type with a name like 'jq filter'
* Set the Line comment to `#`
* Enable "Support paired parens"

Configure the keyword lists as follows:

**List 1** (_keywords_):
* and
* as
* break
* catch
* def
* elif
* else
* end
* false
* foreach
* if
* import
* include
* label
* module
* not
* null
* or
* then
* true
* try

**List 2** (_functions with 1 argument_):
* INDEX
* bsearch
* capture
* combinations
* contains
* del
* delpaths
* endswith
* format
* fromstream
* getpath
* group_by
* has
* in
* index
* indices
* inside
* isempty
* join
* ltrimstr
* map
* map_values
* match
* max_by
* min_by
* nth
* path
* paths
* repeat
* rindex
* rtrimstr
* scan
* select
* sort_by
* splits
* startswith
* strflocaltime
* strftime
* strptime
* test
* truncate_stream
* unique_by
* walk
* with_entries

**List 3** (_functions with 0 arguments_; moved to list 2 because I like this color better!):
* acos
* acosh
* add
* all
* any
* arrays
* ascii_downcase
* ascii_upcase
* asin
* asinh
* atan
* atanh
* booleans
* builtins
* cbrt
* ceil
* cos
* cosh
* debug
* empty
* env
* erf
* erfc
* error
* exp
* exp10
* exp2
* explode
* expm1
* fabs
* finites
* first
* flatten
* floor
* frexp
* from_entries
* fromdate
* fromdateiso8601
* fromjson
* gamma
* get_jq_origin
* get_prog_origin
* get_search_list
* gmtime
* halt
* halt_error
* implode
* infinite
* input
* input_filename
* input_line_number
* inputs
* isfinite
* isinfinite
* isnan
* isnormal
* iterables
* j0
* j1
* keys
* keys_unsorted
* last
* leaf_paths
* length
* lgamma
* lgamma_r
* localtime
* log
* log10
* log1p
* log2
* logb
* max
* min
* mktime
* modf
* modulemeta
* nan
* nearbyint
* normals
* not
* now
* nulls
* numbers
* objects
* pow10
* recurse
* recurse_down
* reverse
* rint
* round
* scalars
* scalars_or_empty
* significand
* sin
* sinh
* sort
* sqrt
* stderr
* strings
* tan
* tanh
* tgamma
* to_entries
* todate
* todateiso8601
* tojson
* tonumber
* tostream
* tostring
* transpose
* trunc
* type
* unique
* utf8bytelength
* values
* y0
* y1

**List 4** (_functions with 2 or more arguments_):
* IN
* JOIN
* atan2
* copysign
* drem
* fdim
* fma
* fmax
* fmin
* fmod
* gsub
* hypot
* jn
* ldexp
* limit
* nextafter
* nexttoward
* pow
* range
* remainder
* scalb
* scalbln
* setpath
* split
* sub
* until
* while
* yn
