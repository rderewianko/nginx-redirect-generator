# nginx-redirect-generator
A simple tool to generate nginx server configuration for redirects by a given URL list.

## Use case
I had to create an nginx server just to redirect from some old domains to new ones. The list of URLs was big and I couldn't generate a general rule because the path on the target url was different from the source. So I created this to help me out :)

It transforms this:

```
http://sample-domain.com/test/123123 http://another-domain.com/awe123123
http://sample-domain.com/test/555 http://another-domain.com/aweaw
http://sample-domain.com/test/3 http://another-domain.com/tawe3

http://different-domain.com/test/123123 http://another-domain.com/awe123123
http://different-domain.com/test/4555 http://another-domain.com/aweaw
http://different-domain.com/test123/3 http://another-domain.com/tawe3
```

Into this:

```nginx
events {}

http {
  server {
    listen 8080;
    server_name sample-domain.com;
    rewrite ^/test/123123$ http://another-domain.com/awe123123 break;
    rewrite ^/test/555$ http://another-domain.com/aweaw break;
    rewrite ^/test/(.*)$ http://another-domain.com/path/ break;
  }
  server {
    listen 8080;
    server_name different-domain.com;
    rewrite ^/test/123123$ http://another-domain.com/awe123123 break;
    rewrite ^/test/4555$ http://another-domain.com/aweaw break;
    rewrite ^/test123/3$ http://another-domain.com/tawe3 break;
  }
}
```

## How to use
* Install NodeJS (https://nodejs.org)
* Clone the repository and open its folder
* Run `npm start <path-to-url-list-file>`
* Done!

## TODO
* [ ] Configure if I want code `301` or `302` on my redirects
