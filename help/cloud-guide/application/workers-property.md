---
title: Workers
description: Learn how to configure the workers property in the Commerce application configuration file.
exl-id: d6816925-5912-45ca-8255-6c307e58542d
---
# Workers property

You can define zero or multiple work instances for each application. A worker instance runs as a container, independent from the web instance and without a running Nginx instance. Also, you do not need to set up a web server on the worker instance (using Node.js or Go) because the router cannot direct public requests to the worker.

A worker instance has the exact same code and compilation output as a web instance. The container image is built once and deployed multiple times if needed using the same `build` hook and `dependencies`. You can customize the container and allocated resources.

Use worker instances for background tasks including:

-  Tasks like queue workers or updating indexes.
-  Tasks to run periodic reporting that are too long for a cron job.
-  Tasks should happen "now", but not block a web request.
-  Tasks are large enough that they risk blocking a deploy, even if they are subdivided.
-  The task in question is a continually running process rather than a stream of discrete units of work.

A basic, common worker configuration could look like this:

```yaml
workers:
    queue:
        size: S
        commands:
            start: |
                php worker.php
```

This example defines a single worker named queue, with a "small" container, and runs the command `php worker.php` on startup. If `worker.php` exits, it is automatically restarted.
