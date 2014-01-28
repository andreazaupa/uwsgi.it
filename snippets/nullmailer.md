Running the nullmailer spooler
------------------------------

Distribution: Saucy Salamander

Nullmailer emulates the venerable sendmail approach giving you a safe spool directory in which messages are stored.

Periodically (or when a special event is triggered) this directory is scanned and its mail forwarded to an external smtp server.

Albeit you could configure your applications to directly connect to external smtp services (like mandrill or sendgrid), this approach is safer as your app can survive remote servers unavailability.

First step is creating the needed directories:

```sh
mkdir /var/spool/nullmailer
mkdir /var/spool/nullmailer/queue
mkdir /var/spool/nullmailer/tmp
```

and the trigger fifo:

```sh
mkfifo /var/spool/nullmailer/trigger
```

then you need (at least) 2 configuration files: /etc/nullmailer/remotes and /etc/nullmailer/defaulthost

The first one contains the address of the smtp server that will send the enqueued emails.

The second one is the default hostname to use when sending unqualified mail with sendmail


/etc/nullmailer/remotes
```sh
smtp.example.com smtp
```

/etc/nullmailer/defaulthost
```sh
example.com
```

Finally start the daemon in dumb mode dropping a file named nullmailer.ini in the vassals directory:

```ini
[uwsgi]
attach-daemon = /usr/sbin/nullmailer-send
```

Bonus: 20tab-nullmailer
-----------------------

nullmailer does not offer you a local smtp service to enqueue email. You can use a simple module developed by 20Tab S.r.l. to have a local smtp service using nullmailer spool system: 

https://github.com/20tab/20tab-nullmailer