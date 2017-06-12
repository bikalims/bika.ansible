# BIKA Varnish Role

Dependencies:
https://galaxy.ansible.com/geerlingguy/varnish

Plone Varnish Reference:
https://docs.plone.org/manage/deploying/caching/varnish4.html


## Logging in Varnish

Import `std`:

    import std;

Show logs on the Server:

    varnishlog -i VCL_Log

Example:

    std.log("Cookies: " + req.http.Cookie);
