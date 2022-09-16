# tiny.Logger
#### **Most powerful, multithreaded file logger for high touch application written for .net** 
#### 
[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Features

- Thread safe file logger.
- Log shipping based on size.
- File Name support for custom formats
- Safe execution handles.
- .net framework is also supported. 

## Installation

``` cmd
Install-Package tiny.Logger
```


## **.net core / .net 5 Sample**

### Example 1
![code example](https://github.com/tinyVishal/tiny.Logger/blob/master/example1.PNG?raw=true)
``` csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Tiny.CreateDefaultBuilderAndTinyLogger(args).....

```

### Example 2 
![code example](https://github.com/tinyVishal/tiny.Logger/blob/master/example2.PNG?raw=true)

``` csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureTinyLogger().....
```

#### Customization from appsettings.json

``` yaml
"Logging": {
    "LogLevel": {
      "Default": "Trace",
      "Microsoft": "Trace",
      "Microsoft.Hosting.Lifetime": "Trace"
    },
    "options": {
      "file": "MYLOG_$|DATE[dd_MMM_yyyy HH_mm]|$.log",  //<--- (1)
      "path":  "c:\\temp", //<--- (2)
      "size": 5242880 //<--- (3),
      "retention-duration": 30 // <--- (4)
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
4. retention duration in days
   - number greater than 0. this means, delete old log file whos age is grater than specified number.
   - if option is **missing** or  **0** means **do not clear old** log files.
.
-----
## .Net Framework Setup.
``` csharp
namespace Sample
{
    using tiny;
    using Microsoft.Extensions.Logging;

    class Program
    {
        static void Main(string[] args)
        {
            // first line of execution or before using logger....
            Extensions.ConfigureTinyLogger(@"c:\temp", MinLogLevel: LogLevel.Trace, ...);
            
            // Code to execute.
            
            var data = new Data() { Property1 = "Some value" };
            Extensions.LogInformation("First Log as information", data);
            
            // Code to execute.
            
```

# Safe Executions
 
 ##### Example in .net core / .net 5
 
 Use this option to wrap exuection with exception handling
 ``` csharp
 
        public int Sample1Sum(int i, int j)
        {
            int k = 0;
            _logger.ExecuteWithLog(() => { k = i + j; });
            return k;
        }

        public int Sample2Sum(int i, int j)
        {
            return _logger.ExecuteWithLog<int>(() => i + j);
        }

        public int Sample3Sum(int i, int j)
        {
            return _logger.ExecuteWithLog<int>(() => i/0 + j, defaultResult: 0);
        }
```
___
##### Example in .net framework
``` csharp
            Extensions.ILogger.ExecuteWithLog(() => 
            { 
                // code 
            });
```

###### ps: You can use Exception, for after execution deligates to perform operations based on your needs

 
## License

MIT
