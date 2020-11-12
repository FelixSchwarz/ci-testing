# ci-testing
repo to test various CI configurations.

[Test in-page anchor](#cookbook-custom-wrapper-for-mq-run)


### Usage (mail submission)

    from schwarz.mailqueue import init_smtp_mailer, MaildirBackend, MessageHandler
    # settings: a dict-like instance with keys as shown below in the "Configuration" section
    settings = {}
    # Adapt the list of transports as you like (ordering matters):
    # - always enqueue: use "MaildirBackend()" only
    # - never enqueue: use "init_smtp_mailer()" only
    transports = [
        init_smtp_mailer(settings),
        MaildirBackend('/path/to/queue-dir'),
    ]
    handler = MessageHandler(transports)
    msg = b'…' # RFC-822/RFC-5322 message as bytes or email.Message instance
    was_sent = handler.send_message(msg, sender='foo@site.example', recipient='bar@site.example')
    # "was_sent" evaluates to True if the message was sent via SMTP or queued
    # for later delivery.
    was_queued = (getattr(send_result, 'queued', None) is not False)


### Usage (mq-run)

The `mq-run` script sends all queued messages to an SMTP server:

    mq-run /path/to/config.ini /path/to/queue

If you want to test your configuration you can send a test message to ensure
the mail flow is set up correctly:

    mq-send-test /path/to/config.ini /path/to/queue --to=recipient@site.example

### Configuration (mq-run)

The configuration file uses the traditional "ini"-like format:

    [mqrunner]
    smtp_hostname = hostname
    smtp_port = 587
    smtp_username = someuser@site.example
    smtp_password = secret
    # optional, format as described in
    # https://docs.python.org/3/library/logging.config.html#logging-config-fileformat
    logging_conf = /path/to/logging.conf



### Cookbook: Custom wrapper for mq-run

something, something
