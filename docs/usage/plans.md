# Plans

Plans in Mottainai are special [tasks](tasksandpipelines.md) with an extra field, *planned*, which is indicating the recurrence of a task.

This means they heredit the full syntax from the tasks, the only difference stands for the extra field ( "*planned*" ).

Plans can be submitted to the infrastructure with the [CLI](cli.md), or with [Replicant](https://github.com/MottainaiCI/replicant).

## Syntax

*Full task example*

```yaml
# Task name
name: "My task"

# Image used by the task player
image: "my/image"

# Script to be executed
script:
  - echo "hello world"

# Task type
type: docker

# We want the task to be executed weekly
planned: "@weekly"
```

*planned* fully supports the cron syntax, here you can find a short summary.


**Note**: Mottainai it's using internally [robfig/cron](https://godoc.org/github.com/robfig) , this is an extract from their documentation for reference. [Head over here for a full explanation of the supported syntax ](https://godoc.org/github.com/robfig/cron#hdr-CRON_Expression_Format).


### Predefined schedules

Entry                  | Description                                | Equivalent To
-----                  | -----------                                | -------------
@yearly (or @annually) | Run once a year, midnight, Jan. 1st        | 0 0 0 1 1 *
@monthly               | Run once a month, midnight, first of month | 0 0 0 1 * *
@weekly                | Run once a week, midnight between Sat/Sun  | 0 0 0 * * 0
@daily (or @midnight)  | Run once a day, midnight                   | 0 0 0 * * *
@hourly                | Run once an hour, beginning of hour        | 0 0 * * * *

### Intervals

You may also schedule a job to execute at fixed intervals, starting at the time it's added. This is supported by using the **planned** field like this:

    @every <duration>

where "duration" is a string accepted by time.ParseDuration (http://golang.org/pkg/time/#ParseDuration).

For example, "*@every 1h30m10s*" would indicate a schedule that activates after 1 hour, 30 minutes, 10 seconds, and then every interval after that.

**Note**: The interval does not take the job runtime into account. For example, if a job takes 3 minutes to run, and it is scheduled to run every 5 minutes, it will have only 2 minutes of idle time between each run.

## Replicant

To deploy all plans defined in a Git repository, you can use [Replicant](https://github.com/MottainaiCI/replicant).

In order to keep trace of the plans in the infrastructure, [Replicant](https://github.com/MottainaiCI/replicant) parses all plans in a Git repository, and automatically submits and keep the infrastructure in sync with the content.

You can even combine and run a Replicant task which is a webhook  inside Mottainai, that automatically keeps your infrastructure updated. <!-- (TODO: Add example and use case) -->
