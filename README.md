# virtualdomains_exim4
This files allow to use a virtual domains debian 11 or the last version on exim4

The domains must be a file on /etc/exim4/virtual/domain.com

The format must be;

Each file will contain lines of the form:

address : username@localhost

Where "address" is the part of the email address to the left of the domain name, and "username" is the account name on the local system which should recieve that mail.

To make it more clear assume:

Alice gets all mail sent to example.net.
Bob gets all mail sent to example.org.
Eve has a mail account at example.com, as does Trent.
This gives /etc/exim4/virtual/example.com:

eve : eve@localhost

trent : trent@localhost

For Bob who has a "catchall" address setup for example.org - /etc/exim4/virtual/example.org:

* : bob@localhost
And similarly for Alice - /etc/exim4/virtual/example.net:

* : alice@localhost
You can also drop all mail for a user by using:

eve : :blackhole:
Or to generate a bounce with some text added so that the sender will know why they got it:

sharon : :fail: She no longer lives here.
Now we just need to make exim4 read these files to know what to do with the incoming mail, a simple enough job.

First of all we need to update the list of domains we handle mail for by editing the file /etc/exim4/conf.d/main/01_exim4-config_listmacrosdefs, change the current "local_domains" with this one:
