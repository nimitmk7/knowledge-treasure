![[Pasted image 20250221145136.png]]

1. Message Request happening over synchronous API calls.
	1. Uses where persistence is super critical
	2. Example: Slack
2. Realtime communication + Asynchronous persistence
	1. Store messages on local device.
	2. Message is immediately sent but then asynchronously processed on the side to store in the database.
	3. Example: Whatsapp
3. Realtime + No persistence
	1. All users connected to the websocket get the message.
	2. No persistence
	3. Example: Zoom chat

Depening on the gurantees needed to be given, you would pick one of these systems.