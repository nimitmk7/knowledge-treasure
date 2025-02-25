

## Drawbacks
1. A browser's connection pool limit may limit concurrent HTTP connections to about 6 per server. Its 6 for Chrome, 8 for Firefox.
	1. It is meant to prevent DDoS attacks from Browser.


# Practical Considerations
1. WebSocket should generally not talk to the database directly.
2. 