# HTTP in Detail

**Fundamentals · Easy · Completed**  
Retrospective notes · Revision exercises pending

[Room](https://tryhackme.com/room/httpindetail) · [Back to the journey](../../README.md)

> I completed this room some time ago and didn't keep notes from the original attempt. This is a retrospective reference, written on November 10, 2026. The original completion date is unknown. Examples below are illustrative, and revision exercises are still to do.

## The focus

Understand what travels between a browser and a web server: requests, responses, methods, headers, status codes, and cookies.

## HTTP and HTTPS

HTTP defines how clients request resources and how servers respond. HTTPS protects that communication using TLS, providing encryption, integrity, and server authentication when certificates are properly validated.

HTTPS protects data in transit; it doesn't prove that a website or its content is trustworthy.

### Reading a URL

Example: `https://example.com:443/notes?topic=http#headers`

| Part | Meaning |
| --- | --- |
| `https` | Scheme |
| `example.com` | Host |
| `443` | Explicit port; also the default for HTTPS |
| `/notes` | Path |
| `topic=http` | Query string |
| `headers` | Fragment used by the client; not sent in the HTTP request |

## Requests and responses

Illustrative HTTP/1.1 request:

```http
GET /notes?topic=http HTTP/1.1
Host: example.com
Accept: text/html

```

The first line gives the method, request target, and version. Headers provide context. A blank line ends the header section; some requests also have a body.

Illustrative response, not captured lab output:

```http
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 5

Hello
```

The response contains a status, headers, and optionally a body. HTTP/2 and HTTP/3 use different wire formats; these readable examples show HTTP/1.1.

## Methods to recognise

| Method | Intended purpose |
| --- | --- |
| `GET` | Retrieve a representation of a resource |
| `HEAD` | Request the headers that a GET would return, without the response body |
| `POST` | Submit data for processing |
| `PUT` | Create or replace the target resource's representation |
| `PATCH` | Apply a partial modification |
| `DELETE` | Request removal of the target resource |

A method describes the intended operation. The application must still enforce authentication and authorization. POST does not encrypt data; HTTPS provides transport protection.

## Reading the outcome

| Range | Meaning |
| --- | --- |
| `1xx` | Informational |
| `2xx` | Successful |
| `3xx` | Redirection or related response, such as cache validation |
| `4xx` | Client-error response |
| `5xx` | Server-error response |

A status code is one piece of evidence. A successful HTTP response doesn't automatically mean a login succeeded or a security test worked; inspect the body and application behaviour too.

## Headers worth noticing

| Header | What to look for |
| --- | --- |
| `Host` | Target host in an HTTP/1.1 request |
| `Content-Type` | Format of the message body |
| `Location` | Destination supplied for a redirect |
| `User-Agent` | Claimed client identity; clients can change it |
| `Set-Cookie` | Server instruction to store a cookie |
| `Cookie` | Cookies sent back by the client |

## Cookies and sessions

HTTP is stateless. Applications can connect separate requests to a session using a cookie containing a session identifier.

Illustrative response header with a dummy value:

```http
Set-Cookie: session=demo-value; Path=/; Secure; HttpOnly; SameSite=Lax
```

- **Secure:** restricts transmission to secure connections, with browser exceptions for localhost.
- **HttpOnly:** prevents JavaScript from reading the cookie through APIs such as `document.cookie`; browsers can still send it with requests.
- **SameSite:** controls when cookies accompany cross-site requests. It helps reduce some CSRF risks.

Real session values should be removed from screenshots and notes.

<details>
<summary>Revision practice — not yet performed</summary>

These are new exercises, not a reconstruction of my original room attempt.

### 1. Inspect a browser request

Open `https://example.com`, open Developer Tools, select **Network**, and reload.

- [ ] Select the document request.
- [ ] Record its URL, method, status, and response Content-Type.
- [ ] Compare request headers with response headers.
- [ ] Save a redacted screenshot and explain what it shows.

### 2. Compare GET and HEAD

On Windows, use `curl.exe` to avoid the Windows PowerShell curl alias. On Linux/macOS, use `curl`.

```powershell
curl.exe -i https://example.com/
curl.exe -I https://example.com/
```

- `-i` includes response headers alongside the body.
- `-I` sends a HEAD request and displays response headers.

- [ ] Compare the outputs and record any differences.
- [ ] Note the actual protocol and status returned.
- [ ] Explain why HEAD has no response body.

### 3. Review cookies

In an authorized lab, inspect a response that sets a cookie and a subsequent request that sends it.

- [ ] Identify the two different header names.
- [ ] Record attributes without recording the session value.
- [ ] Explain what each attribute controls.

**Evidence status:** no revision outputs or screenshots have been collected yet.

</details>

## Where this helps

For SOC investigations, these fields provide a starting point for understanding web logs: which path was requested, which method was used, and how the server responded. Repeated unusual requests need context before being labelled malicious.

For pentesting, understanding the request makes it easier to reason about input, sessions, and access controls in an authorized lab.

## References

- [TryHackMe — HTTP in Detail](https://tryhackme.com/room/httpindetail)
- [MDN — HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview)
- [MDN — HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
- [MDN — Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)

---

**No flags or direct answers. Only my process and learnings.**
