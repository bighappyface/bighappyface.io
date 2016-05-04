## Background

ETags are HTTP headers used to minimize client-server traffic when browser cache headers are enabled and content hasn't changed.

An HTTP server will generate a unique value for an ETag based on various criteria and when a client, which has previously requested a given resource, requests the resource again, the HTTP server will compare the ETag value with the "If-None-Match" header submitted by the client to determine if the resource has changed.

If the resource is the same, a `304 Not Modified` response is returned with an empty body; otherwise, the request is fulfilled as if it were not cached.

## Problem

The criteria used to generate an ETag value is typically server-specific. In the case of Apache web server, three pieces of information are used by default:

* INode (the most server-specific)
* File size
* Last modified time

For more information on this, please visit the <a href="http://httpd.apache.org/docs/2.4/mod/core.html#fileetag">Apache documentation</a>.

Because INode is server-specific, a multi-server environment serving the same content will respond with different ETag values under default settings.

In practice, this isn't a big concern considering that the `Cache-Control` and `Expires` headers are the main source of information used to determine if a resource should be cached client-side, and for how long. If a resource is cached properly, no subsequent request is made to the server, hence no 304 is ever issued from the server as no request was made by the client. The only real benefit is to save the bytes over the wire in the event a resource expires in cache or a hard-refresh is issued client-side.

In an attempt to solve this problem, I have developed a simple proof of concept on how to achieve a common ETag value based strictly on the file size.

https://github.com/bighappyface/common-etags

The repo provides a Vagrant multi-machine configuration and provisions two identical web servers with Apache and a common HTML file. All information is contained in the README but is reproduced below for convenience:

1. Ensure VirtualBox and Vagrant are installed
2. Ensure ports 8080 and 8081 are available for use
3. Clone this repo
4. Open a Terminal session and navigate to the clone directory
5. Run command `vagrant up`
6. Compare ETag values between the two virtual boxes with cURL
7. `curl -sI http://localhost:8080 | grep ETag`
8. `curl -sI http://localhost:8081 | grep ETag`

## Concerns

Some concerns with this approach:

1. If content changes and the file size remains the same, the server could issue a 304 and the user would never get the latest version. This is where the modified time comes into play. While very possible, I do not think this would happen often in practice as typically files are not changed unless something is added or removed, which would almost always affect the size. For an image, a single pixel change would affect the size. For a document, a single character change would affect the size.
2. Fixing the multiple-server ETag issue doesn't really solve a problem. If a client caches content, we are good to go, and the longer the cache, the better. I am all for saving bandwidth by returning 304s with empty bodies, but in practice, a strong cache policy prevents the unnecessary request/response involved with the 304, as well as the server-side comparison. Strong cache solves the real problem and actually improves performance whereas high-fidelity to 304s only saves on bandwidth in certain scenarios. It feels a little bit like diminishing returns, but at scale, it could have a significant impact.

## Goals

Some goals:

1. ETags are part of the Google PageSpeed scoring criteria. This approach allows you to finely tune HTTP traffic and achieve strong scores.
2. The setup is vanilla Apache configuration.

## Final thoughts

I hope this post and repo are helpful to some of you out there. If anything, the Vagrant multi-machine facilities are super cool and provide for easy proof-of-concepts like this for easy digestion and brainstorming on other multi-machine scenarios typically only found in production. As far as ETags are concerned, I am personally indifferent about leaving them or synchronizing them, but I am interested in obtaining high PageSpeed scores to ward off naysayers.

Diving a little deeper into the tech, and finding ways to build examples, always provides excellent learning opportunities. Enjoy!
