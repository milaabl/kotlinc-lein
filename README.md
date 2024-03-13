[![Clojars Project](https://img.shields.io/clojars/v/kotlinc-lein.svg)](https://clojars.org/kotlinc-lein)
[![License](https://img.shields.io/badge/License-EPL%201.0-red.svg)](https://opensource.org/licenses/EPL-1.0)

# kotlinc-lein

A [Leiningen](https://github.com/technomancy/leiningen/blob/master/README.md) plugin for compiling
[Kotlin](https://github.com/JetBrains/kotlin) source files. It behaves similarly with the Leiningen javac plugin.

####Latest version:
[![Clojars Project](https://clojars.org/kotlinc-lein/latest-version.svg)](http://clojars.org/kotlinc-lein)

## Requirements

The kotlinc-lein plugin works with Leiningen version 2.0.0 or higher.

## Installation

You can install the plugin by adding kotlinc-lein to your `project.clj` file in the `:plugins` section:

```clojure
(defproject megacorp/superservice "1.0.0-SNAPSHOT"
  :min-lein-version "2.0.0"
  :plugins          [[kotlinc-lein "0.1.0"]])
  :dependencies     [[org.jetbrains.kotlin/kotlin-runtime "1.0.2"]]
)
```

In addition, notice that you should also add the Kotlin Runtime dependency explicitly.

## Source Layout

By default, Leiningen assumes your project only has Clojure source code under
`src`. When using both Clojure and Kotlin in the same codebase, however, it is
necessary to tell Leiningen where to find Kotlin sources.

To do so, use `:source-paths` and `:kotlin-source-paths` options in the project
definition:

```clojure
(defproject megacorp/superservice "1.0.0-SNAPSHOT"
  :source-paths        ["src/clojure"]
  :kotlin-source-paths ["src/kotlin"])
```

Having one source root contain another (e.g. `src` and `src/kotlin`) can
cause obscure problems.

## Kotlin Source Compilation

To compile Kotlin sources, you can run

    $ lein kotlinc

To automatically compile Kotlin sources add `"kotlinc"` to the `:prep-tasks` vector

## Setting Kotlin Compiler Options With Leiningen

When compiling Kotlin sources, it may be necessary to pass extra arguments to the
compiler. For example, it is very important to target the version of the generated
JVM bytecodes. This can be done via the `:kotlinc-options` which takes a vector of
arguments as you would pass them to `kotlinc` on the command line. In this case we
want to generate JVM bytecode `1.6` and to use Kotlin language version `1.0`:

```clojure
(defproject megacorp/superservice "1.0.0-SNAPSHOT"
  :kotlinc-options ["-jvm-target" "1.6" "-language-version" "1.0"])
```

Equivalently through the command line:

    $ lein kotlinc -jvm-target 1.6 -language-version 1.0

Take note that the example options above are only available when using the snapshot of the
Kotlin compiler version `1.1`

## Changing the Default Kotlin Compiler Version (Currently `1.0.2`)

To use different version of the Kotlin compiler, do it like this:

```clojure
(defproject megacorp/superservice "1.0.0-SNAPSHOT"
  ;; add desired version here
  :kotlin-compiler-version "1.1-SNAPSHOT"
  ;; get snaphot version from repository
  :repositories            [["snapshots" "http://oss.sonatype.org/content/repositories/snapshots"]]
  ;; match runtime version of the compiler
  :dependencies            [[org.jetbrains.kotlin/kotlin-runtime "1.1-SNAPSHOT"]])
```

## License

Copyright © 2016 Clyde Tan

Distributed under the Eclipse Public License either version - v 1.0.
