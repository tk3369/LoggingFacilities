[![Travis Build Status](https://travis-ci.org/tk3369/LoggingFacilities.jl.svg?branch=master)](https://travis-ci.org/tk3369/LoggingFacilities.jl)
[![codecov.io](http://codecov.io/github/tk3369/LoggingFacilities.jl/coverage.svg?branch=master)](http://codecov.io/github/tk3369/LoggingFacilities.jl?branch=master)

# LoggingFacilities

This package contains some general logging facilities.  It uses the [LoggingExtras.jl](https://github.com/oxinabox/LoggingExtras.jl) framework for building composable loggers.

Sink
- `SimplestLogger`, which is simpler than the SimpleLogger from Base :-)

Logging Formats
- `TimestampLoggingTransformer`: either prepend to `message` string or added as a variables
- `OneLineLoggingTransformer`: append `variable=value` pairs to the `message` string
- `JSONLoggingTransformer`: format log as JSON string

## Usage

Use the `logger` function to create new Transformer logger with the desired format.

### Logging with timestamp

```julia
julia> ts_fmt = TimestampLoggingTransformer("yyyy-mm-dd HH:MM:SS", InjectByPrependingToMessage());

julia> with_logger(logger(ConsoleLogger(), ts_fmt)) do
           @info "hey there"
       end
[ Info: 2020-06-13 21:39:13 hey there
```

### Logging everything in a single line

```julia
julia> oneline_fmt = OneLineLoggingTransformer();

julia> with_logger(logger(ConsoleLogger(), oneline_fmt)) do
           x = 1
           y = "abc"
           @info "hey there" x y
       end
[ Info: hey there x=1 y=abc
```

### Logging JSON string

```julia
json_logger = logger(
                SimplestLogger(),
                TimestampLoggingTransformer("yyyy-mm-dd HH:MM:SS", InjectByAddingToKwargs()),
                JSONLoggingTransformer(indent = 2))
```

_Voila!_

```julia
julia> with_logger(json_logger) do
           @info "hey"
           @info "great!"
       end
{
  "timestamp": "2020-06-27 21:45:00",
  "level": "Info",
  "message": "hey"
}
{
  "timestamp": "2020-06-27 21:45:01",
  "level": "Info",
  "message": "great!"
}
```

## Credits

This package was conceived part of [this coding live stream](https://www.youtube.com/watch?v=89xlkSUh_dA).

Special credit to [Chris de Graff](https://github.com/christopher-dG) for joining
the stream and helping out.
