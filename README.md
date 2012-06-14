modsecurity on OpenShift
===================

This git repository helps you quickly setup mod_security 
on OpenShift. modsecurity will perform the function of a WAF or
Web Application Firewall / proxy. This means it will inspect the incoming
protocol requests (most commonly HTTP in this case) for malicious content
such as SQL Injection, CRLF, XPATH Injection etc. This is done primarily 
using the CRS (Core Rule Set). For more details, see modsecurity.org and
owasp.org.

In this README.md we'll use modsec as the DIY gear name, but you can use
whatever you want. Keep in mind that after setting this up, it is this gear
and not your other application gear dns that you'll be submitting requests to. 
The mod_proxy config in this gear will route requests to your actual application
gear.


Running on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/

Create a DIY application

    rhc app create -a modsec -t diy-0.1

Add this upstream repo

    cd modsec
    git remote add upstream -m master git://github.com/prelegalwonder/openshift_modsecurity.git
    git pull -s recursive -X theirs upstream master
    
Modify modsec/etc/httpd/conf.d/mod_proxy.conf

    change Proxypass directives from http://<gear>-<namespace>.rhcloud.com to the gear(s) you want to protect.

Commit your changes to proxy config

    git commit -a -m "tweaking mod_proxy.conf for my gears"

Then push the repo upstream

    git push

That's it, you can now checkout your application at:

    http://modsec-$yournamespace.rhcloud.com

I'm not very savy yet with mod_security so I was unable to figure out how to setup the console for web access
to the audit logs. Feel free to contribute if you understand how to do this.

Reference
------------------------

[Modsecurity Homepage](http://www.modsecurity.org)

[OWASP Homepage](http://www.owasp.org)

For logging, $OPENSHIFT_LOG_DIR/mlogc has the audit and transaction logs. 
