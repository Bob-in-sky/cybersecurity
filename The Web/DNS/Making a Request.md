# Making a Request

<img src="../../_resources/5f04259cf9bf5b57aed2c476-1724075620083.png" alt="5f04259cf9bf5b57aed2c476-1724075620083.png" width="495" height="445" style="display: block; margin: 0 auto;" class="jop-noMdConv">

When you request a domain name, your computer first checks its local cache to see if you've previously looked up the address recently; if not, a request to your Recursive DNS Server will be made.

A recursive DNS Server is usually provided by your ISP, but you can also choose your own. This server also has a local cache of recently looked up domain names. If a result is found locally, this is sent back to your computer, and your request ends here (this is common for popular and heavily requested services such a Google, Facebook, Twitter). If the request cannot be found locally, a journey begins to find the correct answer, starting with the internet's root DNS servers.

The root server acts as the DNS backbone of the internet; their job is to redirect you to the correct Top Level Domain Server, depending on your request. If, for example, you request www.tryhackme.com, the root server will recognize the Top Level Domain of .com and refer you to the correct TLD server that deals with .com addresses.

The TLD server holds records for where to find the authoritative server to answer the DNS request. The authoritative server is often also known as the name server for the domain. For example, the name server for tryhackme.com is kip.cloudflare.com and uma.ns.cloudflare.com. You'll often find multiple name servers for a domain name to act as a backup in case one goes down.

An Authoritative DNS server is the server that is responsible for storing the DNS records for a particular domain name and where any updates to your domain name DNS records would be made. Depending on the record type, the DNS record is then sent back to the Recursive DNS Server, where a local copy will be cached for future requests and then relayed back to the original client that made the request. DNS records all come with a Time To Live (TTL) value. Thiss value is a number represented in sseconds that the response should be saved for locally until you have to look it up again. Caching saves on having to make a DNS request every time you communicate with a server.