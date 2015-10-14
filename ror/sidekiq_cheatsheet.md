Sidekiq & Apartment Sidekiq cheatsheet
---

## List all tenant

    Tenant.pluck('distinct schema').each{ |schema| puts schema }
    Tenant.pluck('distinct schema')

## Get current tenant

    Apartment::Tenant.current

## Switch to tenant

    Apartment::Database.switch(schema)

## Check if sidekiq queue is present

    q = Sidekiq::Queue.new('automation_task')
    q.present?

## Get sidekiq options

    Sidekiq.options

## Get the number of scheduled jobs

    Sidekiq::Stats.new.scheduled_size
    Sidekiq::ScheduledSet.new.count

## Sidekiq Queue

    queue = Sidekiq::Queue.new("automation_task")

### Get the number of jobs within a queue

    queue.size

### Delete all jobs in a queue by removing queue (not recommend)

    queue.clear

### Delete all jobs in a queue that match conditions

    queue.each do |job|
      job.klass # => 'MyWorker'
      job.args # => [1, 2, 3]
      job.delete if job.jid == 'abcdef1234567890'
    end

### Find job by jid

    Sidekiq::Queue.new.find_job(somejid)

### Calculate the latency (in seconds) of a queue (now - when the oldest job was enqueued)

    queue.latency

## Sidekiq Statistics

    stats = Sidekiq::Stats.new
    stats.processed
    stats.failed
    stats.queues
    stats.enqueued

## Reset stats

### To reset processed jobs:

    Sidekiq.redis {|c| c.del('stat:processed') }

### To reset failed jobs:

    Sidekiq.redis {|c| c.del('stat:failed') }

## Get Redis information

    redis_info = Sidekiq.redis { |conn| conn.info }
    redis_info['connected_clients'] # => "16"

## References

* https://github.com/mperham/sidekiq/wiki/API

* https://github.com/mperham/sidekiq/blob/master/lib/sidekiq/api.rb

* http://stackoverflow.com/questions/12683222/are-there-console-commands-to-look-at-whats-in-the-queue-and-to-clear-the-queue
