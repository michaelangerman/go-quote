# go-quote

[![GoDoc](http://godoc.org/github.com/markcheno/go-quote?status.svg)](http://godoc.org/github.com/markcheno/go-quote) 

A free quote downloader library and cli 

Downloads daily historical price quotes from Yahoo and daily/intraday data from various api's. Written in pure Go. No external dependencies. Now downloads crypto coin historical data from various exchanges.

- Update: 11/15/2021 - Removed obsolete markets, converted to go modules

- Update: 7/18/2021 - Removed obsolete Google support

- Update: 6/26/2019 - updated GDAX to Coinbase, added coinbase market

- Update: 4/26/2018 - Added preliminary [tiingo](https://api.tiingo.com/) CRYPTO support. Use -source=tiingo-crypto -token=<your_tingo_token> You can also set env variable TIINGO_API_TOKEN. To get symbol lists, use market: tiingo-btc, tiingo-eth or tiingo-usd

- Update: 12/21/2017 - Added Amibroker format option (creates csv file with separate date and time). Use -format=ami

- Update: 12/20/2017 - Added [Binance](https://www.binance.com/trade.html) exchange support. Use -source=binance

- Update: 12/18/2017 - Added [Bittrex](https://bittrex.com/home/markets) exchange support. Use -source=bittrex  

- Update: 10/21/2017 - Added Coinbase [GDAX](https://www.gdax.com/trade/BTC-USD) exchange support. Use -source=gdax All times are in UTC. Automatically rate limited. 

- Update: 7/19/2017 - Added preliminary [tiingo](https://api.tiingo.com/) support. Use -source=tiingo -token=<your_tingo_token> You can also set env variable TIINGO_API_TOKEN

- Update: 5/24/2017 - Now works with the new Yahoo download format. Beware - Yahoo data quality is now questionable and the free Yahoo quotes are likely to permanently go away in the near future. Use with caution!

Still very much in alpha mode. Expect bugs and API changes. Comments/suggestions/pull requests welcome!

Copyright 2018 Mark Chenoweth

Install CLI utility (quote) with:

```bash
go install github.com/markcheno/go-quote/quote
```

```
Usage:
  quote -h | -help
  quote -v | -version
  quote <market> [-output=<outputFile>]
  quote [-years=<years>|(-start=<datestr> [-end=<datestr>])] [options] [-infile=<filename>|<symbol> ...]

Options:
  -h -help             show help
  -v -version          show version
  -years=<years>       number of years to download [default=5]
  -start=<datestr>     yyyy[-[mm-[dd]]]
  -end=<datestr>       yyyy[-[mm-[dd]]] [default=today]
  -infile=<filename>   list of symbols to download
  -outfile=<filename>  output filename
  -period=<period>     1m|3m|5m|15m|30m|1h|2h|4h|6h|8h|12h|d|3d|w|m [default=d]
  -source=<source>     yahoo|tiingo|tiingo-crypto|coinbase|bittrex|binance [default=yahoo]
  -token=<tiingo_tok>  tingo api token [default=TIINGO_API_TOKEN]
  -format=<format>     (csv|json|hs|ami) [default=csv]
  -adjust=<bool>       adjust yahoo prices [default=true]
  -all=<bool>          all in one file (true|false) [default=false]
  -log=<dest>          filename|stdout|stderr|discard [default=stdout]
  -delay=<ms>          delay in milliseconds between quote requests

Note: not all periods work with all sources

Valid markets:
etfs:       etf
crypto:     bittrex-btc,bittrex-eth,bittrex-usdt,
            binance-bnb,binance-btc,binance-eth,binance-usdt,
            coinbase
```

## CLI Examples

```bash
# display usage
quote -help

# downloads 5 years of Yahoo SPY history to spy.csv 
quote spy

# downloads 1 year of bitcoin history to BTC-USD.csv
quote -years=1 -source=coinbase BTC-USD

# downloads 1 year of Yahoo SPY & AAPL history to quotes.csv 
quote -years=1 -all=true -outfile=quotes.csv spy aapl

# downloads full etf symbol list to etf.txt, also works for nasdaq,nyse,amex
quote etf

# download fresh etf list and 5 years of etf data all in one file
quote etf && quote -all=true -outfile=etf.csv -infile=etf.txt 

# download hourly data for all Bittrex BTC markets all in one file
quote bittrex-btc && quote -source=bittrex -all=true -period=1h -outfile=bittrex-btc.csv -infile=bittrex-btc.txt 
```

## Install library

Install the package with:

```bash
go get github.com/markcheno/go-quote
```

## Library example

```go
package main

import (
	"fmt"
	"github.com/markcheno/go-quote"
	"github.com/markcheno/go-talib"
)

func main() {
	spy, _ := quote.NewQuoteFromYahoo("spy", "2016-01-01", "2016-04-01", quote.Daily, true)
	fmt.Print(spy.CSV())
	rsi2 := talib.Rsi(spy.Close, 2)
	fmt.Println(rsi2)
}
```

## License

MIT License  - see LICENSE for more details
