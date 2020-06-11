# CLJS source map bug demo


## The problem

On some conditions, generated source map is pointing to itself, 
not to compiled javascript file.


## Reproducing

1. Clone the repo
2. Compile `clj --main cljs.main -co main.cljs.edn -c`
3. Open `out/hello/hello.js.map` file
4. `file` key of js map is pointing not to js file, but to map itself


## Possible cause

`cljs.closure/emit-optimized-source-map` is spitting 
source map to the file [and passing](https://github.com/clojure/clojurescript/blob/master/src/main/clojure/cljs/closure.clj#L1408)
the same file to `cljs.source-map/encode` as `file` parameter. 

This parameter,  
(is used)[https://github.com/clojure/clojurescript/blob/master/src/main/cljs/cljs/source_map.cljs#L236] 
as a file name source map is pointing to.

Thus, source map is pointing to itself, not to javascript file.

