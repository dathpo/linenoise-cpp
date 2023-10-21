linenoise-cpp
=============

Multi-platform (Unix, Windows) C++ header-only linenoise-based readline library.

This library gathered code from following excellent libraries, cleaned it up and put it into a C++ header file for convenience.

 * `linenoise.h` and `linenoise.c` ([antirez/linenoise](https://github.com/antirez/linenoise))
 * `ANSI.c` ([adoxa/ansicon](https://github.com/adoxa/ansicon))
 * `Win32_ANSI.h` and `Win32_ANSI.c` ([MSOpenTech/redis](https://github.com/MSOpenTech/redis))

The licenses for the libraries are included in `linenoise.hpp`.

Usage
-----

```c++
#include "linenoise.hpp"

...

const auto path = "history.txt";

// Setup completion words every time when a user types
std::vector<std::string> commands;
auto completion_cb = [&commands](const char *buf, std::vector<std::string> &completions) {
    for (const auto &cmd : commands) {
        // If command starts with what the user has typed, add it
        if (cmd.substr(0, strlen(buf)) == buf) {
            completions.push_back(cmd);
        }
    }
};
linenoise::SetCompletionCallback(completion_cb);

// Enable the multi-line mode
linenoise::SetMultiLine(true);

// Set max length of the history
linenoise::SetHistoryMaxLen(4);

// Load history
linenoise::LoadHistory(path);

while (true) {
    // Read line
    std::string line;
    auto quit = linenoise::Readline("hello> ", line);

    if (quit) {
        break;
    }

    cout <<  "echo: '" << line << "'" << endl;

    // Add text to history
    linenoise::AddHistory(line.c_str());
}

// Save history
linenoise::SaveHistory(path);
```

API
---

```c++
namespace linenoise;

std::string Readline(const char* prompt);

void SetMultiLine(bool multiLineMode);

typedef std::function<void (const char* editBuffer, std::vector<std::string>& completions)> CompletionCallback;

void SetCompletionCallback(CompletionCallback fn);

bool SetHistoryMaxLen(size_t len);

bool LoadHistory(const char* path);

bool SaveHistory(const char* path);

bool AddHistory(const char* line);

const std::vector<std::string>& GetHistory();
```

Tested compilers
----------------

  * Visual Studio 2022
  * GCC 13.2.0

License
-------

BSD license (© 2015 Yuji Hirose)
