# HTTPClientSocket
An HTTP client socket for Xojo that sucks less

## What?
Xojo.Net.HTTPSocket and URLConnection suffer greatly from the "leaky abstraction" problem, that is, they don't abstract away enough of the platform's http libraries. For example, error codes are platform-specific, and the system requirements vary wildly. The built-in sockets use NSURLConnection on Mac, WinHTTP on Windows, and libsoup on Linux. Did you know that your Windows users must install KB3140245 and update the registry to enable TLS 1.1 and newer on Windows 7? Do you know what ciphers are supported by WinHTTP? Should you have to be an expert on this stuff? No.

That's why HTTPClientSocket exists. It's a near drop-in replacement for URLConnection based on The curl classes in the MBS plugin. More specifically, the CURLSMBS class, which includes its own curl library so you get the same library and same behavior on all platforms. You're not at the mercy of your user's systems, which are so often lacking in security updates.

## The Catch
Well... it requires the MBS Curl plugin of course. That should be obvious. There are also two bits of functionality _lost_ when switching to this class, which is why it's a "near" drop-in replacement:
1. This class cannot automatically use system proxy information
2. The ReceivingProgressed Event's NewData parameter is always empty. While not impossible to replicate the original functionality, the performance cost is too great for a parameter that is rarely used.

Oh and your error codes won't match anymore when switching to this class, so if you use them, you'll need to make adjustments.

## Other Benefits
1. Consistent error codes. No more being a platform-wizard to understand connection errors.
2. Additional control, such as disabling redirect following and requiring OCSP stapling.
3. New features, such as Get and Post convenience methods.

## Project State: Very Rough
This project is very new and poorly tested. Test. Contribute. Complain. That's why this is open source. At the moment, the ReceivingProgressed and SendingProgressed events are of particular interest.
