## 🧱 1. What happens when you call `requests.get()` every time

Every time you do:

```python
requests.get(url)
```

the `requests` library has to perform **all of this from scratch**:

- **DNS lookup** — resolve `www.dirk.nl` → IP address  
- **TCP handshake** — create a new network connection to that IP  
- **TLS/SSL handshake** — negotiate HTTPS encryption  
- **Send HTTP headers**  
- **Receive response**  
- **Close the connection**

Those first three steps (DNS + TCP + TLS) are **expensive**.  
They easily take **50–300 ms per request**, depending on latency and SSL negotiation.

If you do this for **1,000 product pages**, you waste *tens of seconds* just reconnecting over and over again.

---

## ⚙️ 2. What `requests.Session()` changes

A `Session` object keeps a **persistent connection pool** to the same host.

When you call:

```python
session = requests.Session()
response = session.get(url)
```

it does this internally:

1. The first request **creates a TCP + TLS connection**.  
2. That connection stays open (**keep-alive**).  
3. When you make the next `session.get()` to the same domain,  
   it **reuses the same connection** — no new handshake needed.

So you skip all the setup overhead for every subsequent request to the same site.  
Essentially, `Session` uses **HTTP persistent connections** (“connection reuse”).
```
