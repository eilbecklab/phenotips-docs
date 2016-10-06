## Setup

1.  Download the all-in-one package from <http://phenotips.org/Download>.
2.  (Optional) Open an independent session by running `screen`.
3.  Unzip the PhenoTips package, cd into the phenotips directory, and run
    `./start.sh`.
4.  (Optional) After starting PhenoTips, exit `screen` by pressing Ctrl+A, then
    Ctrl+X. PhenoTips will continue running even if you close the terminal. You
    can return by typing `screen -r`.

If you have SELinux, you will have to open up a port for PhenoTips to use. For
example, if you start PhenoTips with `JETTY_PORT=4000 ./start.sh`, you will have
to run `semanage port -a -t http_port_t -p tcp 4000` or SELinux will block any
attempt to connect on that port. See
https://wiki.centos.org/HowTos/SELinux#head-ad837f60830442ae77a81aedd10c20305a811388

If proxying through Apache, mod_security will think that normal PhenoTips page
requests are SQL injection attacks. To get around this, make sure httpd.conf
includes SecRuleEngine Off.

## Backup

PhenoTips's builtin backup function (*Administration* > *Backup*) stops working
once the database contains about 10,000 patients (see
[PhenoTips bug 2369](https://phenotips.atlassian.net/browse/PT-2369)). However,
you can back up a PhenoTips site of any size just fine by making a copy of its
data directory. To restore the backup, follow the same instructions below that
you would use for upgrading.

This is also very useful for creating a perfect copy of a PhenoTips site for
local testing.

If you want to copy just your data model and UI customizations (no patient
data) to another PhenoTips site, go to
/bin/export/PhenoTips/WebHome?format=xar&name=data-model-and-ui-customizations&pages=xwiki%3APhenoTips.%25&pages=xwiki%3AStudies.%25
and import the file on the other site through *Administration* > *Import*. This
will overwrite the data model and any conflicting UI customizations on the
target site.

## Upgrading

1.  Get a new standalone version and unzip it.
2.  Delete its data directory.
3.  Copy the data directory from the old instance.
4.  Start the new instance.
5.  Log in as Admin.
6.  Follow the onscreen "distribution wizard".
