![Internet Tools](assets/logo.png)

# Internet Tools

Ask your assistant about a domain in plain words and it runs the checks from
[internet-tools.net](https://internet-tools.net) against live DNS: records, propagation,
DNSSEC, SPF/DKIM/DMARC, mail blocklists, WHOIS, TLS certificates and HTTP security
headers. Every finding carries a level, a message and, where there is one, the fix.

Works in Claude (chat, Cowork, Claude Code), ChatGPT and Codex. No account, no key.

## What it provides

| Tool | What it does |
|------|--------------|
| `dns_records` | Every common record type for a domain (A, AAAA, CNAME, MX, TXT, NS, SOA, CAA), or the PTR for an IP |
| `dns_propagation` | Compares what five public resolvers answer for one record, to tell whether a change has propagated |
| `dnssec` | Whether a domain is signed, whether the DS matches the published keys, and whether resolvers validate it |
| `email_health` | MX, SPF (with the RFC 7208 lookup count), DKIM on common selectors and DMARC, graded |
| `blacklist_check` | An IPv4 address or a domain against the mail blocklists receivers consult |
| `whois` | Registrar, creation and expiry dates, name servers and status, over RDAP or WHOIS |
| `ssl_certificate` | The certificate a host serves on port 443: issuer, expiry, name coverage and chain |
| `http_headers` | Fetches a public URL, follows its redirects and grades its security headers |

There is also an `audit` prompt that runs every check on a domain and lists what to fix,
worst first.

Try: "Check SPF, DKIM and DMARC for example.com and tell me what to fix."

## What it sends and where

This plugin contains no code. It connects your assistant to one remote MCP server:

    https://internet-tools.net/mcp

The only data sent is what the assistant passes to a tool: a domain name, an IP address or
a URL. The server then:

- resolves names over DNS-over-HTTPS through public resolvers (Cloudflare, Google,
  AdGuard, DNS.SB, NextDNS),
- asks the registry for the domain over RDAP, or its WHOIS server where there is no RDAP,
- connects to the host you named to read its TLS certificate or HTTP headers,
- looks the address up in public DNS blocklists.

Every tool is read-only. Private, reserved and local addresses are refused, at every
redirect. Requests are rate limited per IP address. Nothing is stored beyond what the
[privacy policy](https://internet-tools.net/privacy-policy) describes. Terms of use are in
the [legal notice](https://internet-tools.net/legal-notice).

## Without the plugin

Any MCP client that speaks streamable HTTP can use the URL above directly. Setup for each
client is on [internet-tools.net/mcp-server](https://internet-tools.net/mcp-server).

## License

MIT
