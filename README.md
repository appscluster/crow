![Crow logo](http://i.imgur.com/wqivvjK.jpg)

Crow is C++ microframework for web. (inspired by Python Flask)

[![Travis Build](https://travis-ci.org/ipkn/crow.svg?branch=master)](https://travis-ci.org/ipkn/crow)
[![Coverage Status](https://coveralls.io/repos/ipkn/crow/badge.svg?branch=master)](https://coveralls.io/r/ipkn/crow?branch=master)

```c++
#include "crow.h"

int main()
{
    crow::SimpleApp app;

    CROW_ROUTE(app, "/")([](){
        return "Hello world";
    });

    app.port(18080).multithreaded().run();
}
```

## Features

 - Easy routing
   - Similiar to Flask
   - Type-safe Handlers (see Example)
 - Very Fast
   - ![Benchmark Result in one chart](https://camo.githubusercontent.com/890a8cc686d800e3e5a074ec8b6922ada48accfe/68747470733a2f2f646f63732e676f6f676c652e636f6d2f7370726561647368656574732f642f314b69644f395870757743525a32705f4a52444a6a326165703631483853685f4b444f684170697a76344c452f70756263686172743f6f69643d3230343134363737383926666f726d61743d696d616765)
   - More data on [crow-benchmark](https://github.com/ipkn/crow-benchmark)
 - Fast built-in JSON parser (crow::json)
 - [Mustache](http://mustache.github.io/) based templating library (crow::mustache)
 - Header only
 - Provide an amalgamated header file `crow_all.h' with every features
 - Middleware support

## Still in development
 - Built-in ORM

## Examples

#### JSON Response
```c++
CROW_ROUTE(app, "/json")
([]{
    crow::json::wvalue x;
    x["message"] = "Hello, World!";
    return x;
});
```

#### Arguments
```c++
CROW_ROUTE(app,"/hello/<int>")
([](int count){
    if (count > 100)
        return crow::response(400);
    std::ostringstream os;
    os << count << " bottles of beer!";
    return crow::response(os.str());
});
```
Handler arguments type check at compile time
```c++
// Compile error with message "Handler type is mismatched with URL paramters"
CROW_ROUTE(app,"/another/<int>")
([](int a, int b){
    return crow::response(500);
});
```

#### Handling JSON Requests
```c++
CROW_ROUTE(app, "/add_json")
.methods("POST"_method)
([](const crow::request& req){
    auto x = crow::json::load(req.body);
    if (!x)
        return crow::response(400);
    int sum = x["a"].i()+x["b"].i();
    std::ostringstream os;
    os << sum;
    return crow::response{os.str()};
});
```

## How to Build

If you just want to use crow, copy amalgamate/crow_all.h and include it.

### Requirements

 - C++ compiler with good C++11 support (tested with g++>=4.8)
 - boost library
 - CMake for build examples
 - Linking with tcmalloc/jemalloc is recommended for speed.

 - Now supporting VS2013 with limited functionality (only run-time check for url is available.)

### Building (Tests, Examples)

Out-of-source build with CMake is recommended.

```
mkdir build
cd build
cmake ..
make
```

You can run tests with following commands:
```
ctest
```


### Installing missing dependencies

#### Ubuntu
    sudo apt-get install build-essential libtcmalloc-minimal4 && sudo ln -s /usr/lib/libtcmalloc_minimal.so.4 /usr/lib/libtcmalloc_minimal.so
    
#### Fedora 22
    dnf groupinstall "Development Tools"
    dnf install boost google-perftools google-perftools-devel

#### OSX
    brew install boost google-perftools

### Attributions

Crow uses the following libraries.

    qs_parse

    https://github.com/bartgrantham/qs_parse

    Copyright (c) 2010 Bart Grantham
    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:
    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.

