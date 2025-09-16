1brc-php
=  
My solution to the [one billion row challenge](https://www.morling.dev/blog/one-billion-row-challenge/) in php.

solution
-
This solution uses non-thread safe `php` (NTS) and requires the `pcntl` extention to fork processes.  It uses `stream_socket_pairs` and `stream_select` to do interprocess communiction (IPC).

disabling xdebug and enabling opcache
-
You can get better performance if you disable the `xdebug` extension (if you have it installed) and enable the `opcache` extension with the `opcache.jit` option on.

Instead of running `php` like this:
```
php -v

```
run it like this:
```
XDEBUG_MODE=off php -d opcache.enable_cli=1 -d opcache.jit=on -d opcache.jit_buffer_size=128M -v

```

input
-
Create the input file using the `create-measurements.php` script.
```
Usage: php create-measurements.php <num_records> [file]
  num_records  Number of records [1..1e9]
  file         Output file (default: measurements.txt)
```
```
XDEBUG_MODE=off php -d opcache.enable_cli=1 -d opcache.jit=on -d opcache.jit_buffer_size=128M \
create-measurements.php 1e9
```
```
Creating file measurements.txt with 1,000,000,000 records...
>>>>>>>>>> Wrote   100,000,000 records in 0:00:24.8393
...
>>>>>>>>>> Wrote 1,000,000,000 records in 0:04:10.2801
Done.
```

running
-  
Run the solution in the `calculate-average.php` script by specifying the number of processes to fork.  Start with twice the number of cpu cores available and adjust up and down as necessary.
```  
Usage: php calculate-average.php <num_procs> [file]
  num_procs  Number of processes [1..36] (cpu reports 12 cores)
  file       Input file (default: measurements.txt)
```  
```
XDEBUG_MODE=off php -d opcache.enable_cli=1 -d opcache.jit=on -d opcache.jit_buffer_size=128M \
calculate-average.php 24
```  
```
Forking 24 processes...
{Abha=-34.0/18.0/64.9,Abidjan=-22.7/26.1/73.8
...
Ürümqi=-41.2/7.4/56.9,İzmir=-31.2/17.9/69.3}
Execution time: 14.7163 seconds
   Peak memory: 0.8877 MiB
```

links
-  
Gunnar Morling's [One Billion Row Challenge](https://www.morling.dev/blog/one-billion-row-challenge/)
<br>
Official java github: [gunnarmorling/1brc](https://github.com/gunnarmorling/1brc)
<br>
github: [ben-morin/1brc-php](https://github.com/ben-morin/1brc-php)