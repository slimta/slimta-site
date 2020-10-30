
.. include:: /global.rst

Queue Services
==============

When a client sends an email, that email may go through several email servers
before arriving at its final destination. Each server is designed to take
responsibility for delivering the message to the next one, retrying if
necessary. Once a server has taken responsibility for a message, the connecting
client (or server) may disconnect.

The *queue* service is responsible for making sure email messages are stored
persistently somewhere (in case of catastrophic failure) and that delivery is
tried and retried with due diligence.

Delivery Attempts
"""""""""""""""""

Delivery attempts for a queue are performed with the |Relay| object passed in to
the |Queue| constructor. The delivery attempt will ultimately produce a |Reply|
object indicating its success or failure. If delivery was successful, the queue
will remove the message from persistent storage.

If delivery failed permanently, with a ``5xx`` code or too many ``4xx`` codes,
a |Bounce| envelope is created from the original message, which is delivered
back to the original message sender. The original message is removed from
storage and not retried.

If delivery failed transiently, with a ``4xx`` code (which usually includes
connectivity issues), the message is left in storage and a new delivery attempt
is scheduled in the future. The time between delivery attempts is managed by the
``backoff`` function passed in to the |Queue| constructor. If this ``backoff``
function returns ``None``, the message is permanently failed.

Here is an example ``backoff`` function that makes 5 delivery attempts with an
exponentially increasing backoff time::

    def exponential_backoff(envelope, attempts):
        if attempts <= 5:
            return 12.0 * (5.0 ** attempts)
        return None


Persistent Storage
""""""""""""""""""

A storage mechanism should store the entirety of an |Envelope| object, such that
it can be recreated on restart. Along with the envelope, queue services must
also keep track of when a message's next delivery attempt should be and how many
attempts a message has undergone. In essence, a queue's storage mechanism allows
|slimta| to be stopped and restarted without losing state.

In-Memory
'''''''''

The :class:`~slimta.queue.dict.DictStorage` class is a simple storage mechanism
that, by itself, *does not* provide on-disk persistence. By default, it creates
two dicts in memory for queue data, but passing in :mod:`shelve` objects will
allow basic persistence. Be aware, however, that :mod:`shelve` may not handle
system or process failure and could leave corruption.

The :class:`~slimta.queue.dict.DictStorage` class is very useful for development
and testing, but probably should be avoided for live systems.

Local Disk
''''''''''

.. _pyaio: https://github.com/felipecruz/pyaio

In the fashion of traditional MTAs, :mod:`~slimta.diskstorage` writes
|Envelope| data and queue metadata directly to disk to configurable
directories. This functionality relies on `pyaio`_ to asynchronously write and
read files on disk, and as such it is *only available on Linux*.

To ensure file creation and modification is atomic, files are
first written to a scratch directory and then :func:`os.rename` moves them to
their final destination. For this reason, it is important that the scratch
directory (``tmp_dir`` argument in the constructor) reside on the same
filesystem as the envelope and meta directories (``env_dir`` and ``meta_dir``
arguments, respectively).

The files created in the envelope directory will be identified by a
:func:`~uuid.uuid4` :attr:`hexadecimal string <uuid.UUID.hex>` appended with
the suffix ``.env``. The files created in the meta directory will be identified
by the same uuid string as its corresponding envelope file, but with the suffix
``.meta``. The envelope and meta directories can be the same, but two
:class:`~slimta.diskstorage.DiskStorage` should not share directories.

To use this storage::

    $ pip install python-slimta[disk]

And to initialize a new :class:`~slimta.diskstorage.DiskStorage`::

    from slimta.diskstorage import DiskStorage

    queue_dir = '/var/spool/slimta/queue'
    queue = DiskStorage(queue_dir, queue_dir)

Redis
'''''

.. _redis: http://redis.io/
.. _redis-py: https://github.com/andymccurdy/redis-py

Taking advantage of the advanced data structures and ease of use of the redis_
database, :mod:`~slimta.redisstorage` simply creates a hash key for each queued
message, containing its delivery metadata and a pickled version of the
|Envelope|.

The keys created in redis will look like the following::

    redis 127.0.0.1:6379> KEYS *
    1) "slimta:28195d3b0a5847f9853e5b0173c85151"
    2) "slimta:5ebb94976cd94b418d6063a2ca4cbf8f"
    3) "slimta:d33879cf66244472b983770ba762e07b"
    redis 127.0.0.1:6379> 

Each key is a hash that will look something like::

    redis 127.0.0.1:6379> HGETALL slimta:d33879cf66244472b983770ba762e07b 
    1) "attempts"
    2) "2"
    3) "timestamp"
    4) "1377121655"
    5) "envelope"
    6) "..."
    redis 127.0.0.1:6379> 

On startup, the |Queue| will scan the keyspace (using the customizable prefix
``slimta:``) and populate the queue with existing messages for delivery.

To use redis you must install::

    $ pip install python-slimta[redis]

And to initialize a new :class:`~slimta.redisstorage.RedisStorage`::

    from slimta.redisstorage import RedisStorage

    store = RedisStorage('redis.example.com')

Cloud Storage
'''''''''''''

.. _Rackspace Cloud: https://www.rackspace.com/cloud/
.. _AWS: https://aws.amazon.com/

The :mod:`slimta.cloudstorage` module makes available connectors to two cloud
service providers, `Rackspace Cloud`_ and `AWS`_. |Envelope| data and queue
metadata are written to a cloud object store. Optionally, a reference to their
location in the object store is then written to a cloud message queue, which
can alert relayers in other processes of the availability of a new message in
the object store.

To use `Rackspace Cloud`_ services, you need instances of the
:class:`~slimta.cloudstorage.rackspace.RackspaceCloudAuth`,
:class:`~slimta.cloudstorage.rackspace.RackspaceCloudFiles`, and optionally
:class:`~slimta.cloudstorage.rackspace.RackspaceCloudQueues`::

    auth = RackspaceCloudAuth({'username': 'slimta', 'api_key': 'XXXXXXXXXXXX'},
                              region='IAD')
    cloud_files = RackspaceCloudFiles(auth)
    cloud_queues = RackspaceCloudQueues(auth)

Using `AWS`_ services is a bit different. First, it requires installation and
configuration of the :mod:`boto` library::

    $ pip install python-slimta[aws]

Using the :mod:`boto` library, we need to come up with references to a
:class:`~boto.s3.bucket.Bucket` and optionally a
:class:`~boto.sqs.queue.Queue`. Then, use them to create
:class:`~slimta.cloudstorage.aws.SimpleStorageService` and
:class:`~slimta.cloudstorage.aws.SimpleQueueService` objects::

    from boto.s3.connection import S3Connection
    s3_conn = S3Connection('1A2B3C4D5E', 'XXXXXXXXXXXX')
    s3_bucket = s3_conn.get_bucket('slimta-queue')

    import boto.sqs
    sqs_conn = boto.sqs.connect_to_region('us-west-2',
            aws_access_key_id='1A2B3C4D5E',
            aws_secret_access_key='XXXXXXXXXXXX')
    sqs_queue = sqs_conn.create_queue('slimta-queue')

    from slimta.cloudstorage.aws import SimpleStorageService, SimpleQueueService
    s3 = SimpleStorageService(s3_bucket)
    sqs = SimpleQueueService(sqs_queue)

Once you have these objects created for your cloud service, link them together
into a queue storage driver using :class:`~slimta.cloudstorage.CloudStorage`::

    from slimta.cloudstorage import CloudStorage

    storage = CloudStorage(cloud_files, cloud_queues)
    # or...
    storage = CloudStorage(s3, sqs)

This object can then be used anywhere a :class:`~slimta.queue.QueueStorage`
object is required.
