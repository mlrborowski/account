This service manages a stock portfolio.  The data is backed by two DB2 tables, communicated with via JDBC.  The following operations are available:

GET / - gets summary data for all portfolios
POST /{owner} - creates a new portfolio for the specified owner
GET /{owner} - gets details for the specified owner
PUT /{owner} - updates the portfolio for the specified owner (by adding a stock)
DELETE /{owner} - removes the portfolio for the specified owner

All operations return JSON.  A portfolio object contains fields named owner, total, and loyalty, plus an array of stocks.  A stock object contains fields named symbol, shares, price, total, and date.  The PUT operation takes query params named symbol and shares.

For example, doing a PUT to http://localhost:9080/portfolio/John?symbol=IBM&shares=123 (against a freshly created portfolio for John) would return JSON like {"owner": "John", "total": 19120.35, "loyalty": "Bronze", "stocks": [{"symbol": "IBM", "shares": 123, "price": 155.45, "total": 19120.35, "date": "2017-06-26"}]}.

The above REST call would also add a row to the Stocks table via a SQL statement like "INSERT INTO Stock (owner, symbol, shares, price, total, dateQuoted) VALUES ('John', 'IBM', 123, 155.45, 19120.35, '2017-06-26')", and would update the corresponding row in the Portfolio table via a SQL statement like "UPDATE Portfolio SET total = 19120.35, loyalty = 'Bronze' WHERE owner = 'John'".
