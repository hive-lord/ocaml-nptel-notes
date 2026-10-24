# Tutorial: walking Heartbleed end to end

# Tutorial: walking Heartbleed end to end

## Tutorial: walking Heartbleed end to end
Lectures built the safety picture in generality. This tutorial lands all of it on one concrete, exhaustively-documented case study: **Heartbleed**, CVE-2014-0160, the 2014 OpenSSL bug. OpenSSL ran on roughly two-thirds of the internet.

Best-documented memory-safety incident in computing history. The bug is two lines of code, the exploit is one paragraph, the fix is a single length comparison, and the OCaml equivalent is *structurally incapable* of the same bug.

## Roadmap
1. The TLS heartbeat extension, what it is supposed to do
2. The OpenSSL bug: the actual code, the actual mistake
3. The exploit: read up to 64 KB of server memory per request
4. The fix: one length comparison
5. The OCaml equivalent: same shape, bug class impossible

## The TLS Heartbeat Extension
TLS is the protocol behind every HTTPS URL. In 2012, RFC 6520 added a *heartbeat* extension: long-lived TLS connections sometimes go quiet, middleboxes drop them during silence, so either side may send a small message asking the peer to echo it back, as a liveness check.

A heartbeat *request* carries a type byte, a two-byte *payload length*, the *payload* of that length, and padding. The peer must echo back the exact payload.

## The OpenSSL Bug
```c
// Simplified OpenSSL 1.0.1 heartbeat handler
int dtls1_process_heartbeat(SSL *s) {
  unsigned char *p = &s->s3->rrec.data[0];
  unsigned char *pl = p;
  unsigned short hbtype = *p++;  // heartbeat type
  unsigned int payload = n2s(p); // payload length from packet!
  
  // ... allocate buffer ...
  // memcpy from pl to buffer using payload as length
  
  // BUG: no check that payload <= actual received data!
  // If payload = 65535 but packet only has 1 byte,
  // memcpy reads 64KB of adjacent server memory
}
```

## The Over-read, Visually
The bug is an *out-of-bounds read* (CWE-126). Shipped in OpenSSL 1.0.1 through 1.0.1f, disclosed 7 April 2014. By evening, every public TLS server was running it.

## The Exploit, and the Fix
**Exploit**: legitimate TLS connection, `payload = 65535`, 1 real byte, receive ~64 KB of server memory, repeat. Observed leaks: TLS private keys, session tokens, passwords, log buffers.

**The official fix** (OpenSSL commit 96db9023):
```c
if (1 + 2 + payload + 16 > s->s3->rrec.length)
    return 0;  /* discard the malformed request */
```
One line. Checks that incoming record is at least big enough to hold what the length field claims.

## The OCaml Equivalent: Bug Class Impossible
```ocaml
let handle_heartbeat record =
  let payload_len = get_payload_length record in
  let payload = Bytes.sub record 3 payload_len in
  (* ... echo back ... *)
```
We never wrote a length check. We just asked for `Bytes.sub record 3 payload_len`. What happens when `payload_len = 65535` but record is 4 bytes long?

`Bytes.sub` raises `Invalid_argument` if `pos` and `len` do not designate a valid range. The bounds check is *part of the API*, not optional, not a performance opt-in, not a thing a programmer can forget.

The exception propagates to the handler's caller, which closes the connection. The attacker gets a connection close, not 64 KB of memory.

## The Big Picture
> **OCaml's safety is not magic; it is GC + types + exhaustive matching + bounds-checked stdlib, applied consistently.**

## What's next
Module 10 complete. Next modules pick up the safety story at the type level (extending what the type checker can guarantee) and then apply the whole stack to building real systems in OCaml.

**Concepts**: tutorial, Heartbleed, OpenSSL, bounds checking, memory safety, CVE
**Keywords**: OCaml, Heartbleed, CVE-2014-0160, OpenSSL, bounds check, Bytes.sub, memory safety
**Next**: M11 · L01 · Locality: safe stack allocation