ocaml-xapi-plugin [![Build status](https://travis-ci.org/xapi-project/ocaml-xapi-plugin.png?branch=master)](https://travis-ci.org/xapi-project/ocaml-xapi-plugin)
-----------------

Library to simplify writing xapi plugins in OCaml

Build dependencies
------------------

* [rpc](https://github.com/samoht/ocaml-rpc)

Usage
-----

Simple xapi plugin which waits a specified number of seconds, then returns the string "done":

```ocaml
let main session_id args =
  if List.mem_assoc "sleep" args then begin
    let time = int_of_string (List.assoc "sleep" args) in
    Unix.sleep time
  end;
  "done"

let () =
  Xapi_plugin.dispatch [
    "main", main;
  ]
```

Compile with:

```
$ ocamlfind ocamlopt -package unix -package rpclib -package xapi-plugin -linkpkg test_ocaml_plugin.ml -o test-ocaml-plugin
```

Copy the resulting binary to `/etc/xapi.d/plugins` on an XCP/XenServer host,
and then run like so:

```
# xe host-call-plugin host-uuid=0775b809-2064-4b1c-9b20-c02cab711ac8 plugin=test-ocaml-plugin fn=main args:sleep=5
done
```

It's also possible to run the same code as an interpreted script with these lines added to the top:

```ocaml
#!/path/to/ocaml

#use "topfind"
#require "xapi-plugin"
```
