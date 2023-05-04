<!-- shared with plugins that use queues. -->

This plugin utilizes a queue to decouple the production and
consumption of data.  This reduces the number of concurrent requests
made to the upstream server under high load situations and provides
for buffering during temporary network or upstream outages.

A number of parameters can be tuned to influence the behavior and
capacity of the queues used by the plugin.  Please see [XXX](the
documentation on queues) for detail information on the usage of these
parameters.

The queue parameters all reside in a record under the key `queue` in
the `config` parameter section of the plugin.

parameters:
  - name: max_batch_size
    type: number
    default: 1
    description: Maximum number of entries to be processed together
        as a batch.
  - name: max_coalescing_delay
    type: number
    default: 1
    description: >
        Maximum number of (fractional) seconds to elapse
        after the first entry was queued before the queue starts
        processing entries.  This parameter has no effect when
        `batch_max_size` is 1, as queued entries will be sent
        immediately in that case.
  - name: max_entries
    type: number
    default: 10000
    description: >
        Maximum number of entries that can be waiting on the queue.
        Once this number of requests is reached, the oldest entry is
        deleted from the queue before a new one is added.
  - name: max_bytes
    type: number
    default: nil
    description:
        Maximum number of bytes that can be waiting on a queue.
        Once this many bytes are present on a queue, old entries
        up to the size of a new entry to be enqueued are deleted
        from the queue.
  - name: max_retry_time
    type: number
    default: 60
    description: >
        Time in seconds before the queue gives up trying to send a
        batch of entries.  Once this time is exceeded for a batch,
        it is deleted from the queue without having been sent.  If
        this parameter is set to -1, no retries will be made for a
        failed batch.
  - name: max_retry_delay
    type: number
    default: 60
    description: >
        Maximum time in seconds between retries sending a batch of
        entries.  The interval between retries follows an
        exponential back-off algorithm capped at this number of
        seconds.
