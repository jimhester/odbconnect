
<!-- README.md is generated from README.Rmd. Please edit that file -->
odbconnect
==========

The goal of odbconnect is to provide a DBI-compliant interface to ODBC drivers.

Benchmarks vs RODBC / RODBCDBI
------------------------------

Simply reading a postgres table with the nytflights13 'flights' database.

``` r
# First using RODBC / RODBCDBI
library(DBI)
library(RODBCDBI)
rodbc <- dbConnect(RODBCDBI::ODBC(), dsn="database1")
rodbc_query <- dbSendQuery(rodbc, "SELECT * from flights")
system.time(rodbc_result <- dbFetch(rodbc_query))
#>    user  system elapsed 
#>  10.753   2.096  13.001

# Now using odbconnect
library(odbconnect)
odbconnect <- dbConnect(odbconnect::odbconnect(), "DSN=database1")
odbconnect_query <- dbSendQuery(odbconnect, "SELECT * from flights")
system.time(odbconnect_result <- dbFetch(odbconnect_query))
#>    user  system elapsed 
#>   2.436   0.531   2.968

# Using a larger buffer size can reduce the time further (default is 2^12)
odbconnect_query <- dbSendQuery(odbconnect, "SELECT * from flights", buffer_size = 2^14)
system.time(odbconnect_result <- dbFetch(odbconnect_query))
#>    user  system elapsed 
#>   1.600   0.164   1.766

identical(dim(rodbc_result), dim(odbconnect_result))
#> [1] TRUE
```

ODBC Documentation
------------------

<https://msdn.microsoft.com/en-us/library/ms712628(v=vs.85).aspx> <https://msdn.microsoft.com/en-us/library/ms714086(v=vs.85).aspx>
