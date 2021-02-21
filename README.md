# tiny.Logger
## _Most powerful, multithreaded file logger for high touch application written for .net 5_

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Features

- Thread safe file logger.
- Log shipping based on size.
- File Name support for custom formats

## Installation
``` cmd
Install-Package tiny.Logger
```

## Example 1
![code example](https://github.com/tinyVishal/tiny.Logger/blob/master/example1.PNG?raw=true)
``` cmd
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Tiny.CreateDefaultBuilderAndTinyLogger(args).....

```

## Example 2 
![code example](https://github.com/tinyVishal/tiny.Logger/blob/master/example2.PNG?raw=true)

``` cmd
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureTinyLogger().....
```

## Customization from appsettings.json

```
"Logging": {
    "LogLevel": {
      "Default": "Trace",
      "Microsoft": "Trace",
      "Microsoft.Hosting.Lifetime": "Trace"
    },
    "options": {
      "file": "MYLOG_$|DATE[dd_MMM_yyyy HH_mm]|$.log",  <--- (1)
      "path":  "c:\temp", <--- (2)
      "size": 5242880 <--- (3)
    }
  },
```
1. "file" => Name of file. 
    - available options
        - **$|DATE|$** = for date without format
        - **$|DATE[FORMAT]|$** = customize format as per your need e.g. **MYLOG_$|DATE[dd_MMM_yyyy HH_mm]|$.log** => **MYLOG_DATE01_JAN_2021 10_45.log** 
2. path: folder path for log file
   - available options
     - using environment variables support **%temp%\logs** => **C:\Users\...\AppData\Local\Temp**
3. size
   - minimum is **1048576 (1MB)** in case of smaller value than 1MB will be ignored.
   - if option is **missing** means **disable** log shipping based on size.
## License

MIT
