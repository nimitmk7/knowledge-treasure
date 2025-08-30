Threads are not blocked, but keep reacting to each other and never progress. 

> [!TIP] Analogy:
> Two people in a hallway both step left, then right, repeatedly trying to let each other pass — and neither gets through.

>[!faq]  Difference from Deadlock:**
> - In deadlock: threads are stuck **waiting**.
> - In livelock: threads are **actively doing work** but not making progress.

# Avoiding Livelocks
1.  Adding randomness (backoff)
2. Using retry counters or giving up after a threshold


