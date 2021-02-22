# 12-Factor App Methodology



- [1. Codebase](https://www.joomlatools.com/developer/platform/12-factor-app/#1-codebase)
- [2. Dependencies](https://www.joomlatools.com/developer/platform/12-factor-app/#2-dependencies)
- [3. Configuration](https://www.joomlatools.com/developer/platform/12-factor-app/#3-configuration)
- [4. Backing Services](https://www.joomlatools.com/developer/platform/12-factor-app/#4-backing-services)
- [5. Build, release, run](https://www.joomlatools.com/developer/platform/12-factor-app/#5-build-release-run)
- [6. Processes](https://www.joomlatools.com/developer/platform/12-factor-app/#6-processes)
- [7. Port Binding](https://www.joomlatools.com/developer/platform/12-factor-app/#7-port-binding)
- [8. Concurrency](https://www.joomlatools.com/developer/platform/12-factor-app/#8-concurrency)
- [9. Disposability](https://www.joomlatools.com/developer/platform/12-factor-app/#9-disposability)
- [10. Dev/prod parity](https://www.joomlatools.com/developer/platform/12-factor-app/#10-devprod-parity)
- [11. Logs](https://www.joomlatools.com/developer/platform/12-factor-app/#11-logs)
- [12. Admin Processes](https://www.joomlatools.com/developer/platform/12-factor-app/#12-admin-processes)

The [12-Factor App](http://12factor.net/) defines a methodology for developing and deploying web applications, specifically software-as-a-service apps. Modern web applications are complex beasts. You need to provide for every step of the process: from virtualization, deployment, setting up the runtime and developer environments to managing databases and networking. All while keeping performance at a maximum.

The 12-Factor App methodology doesn't eliminate this complexity, but it will provide a solid framework for organizing your project in order to maintain a healthy and scaleable application. These 12 principles each apply to a subset of your application and will guide you in finding the ideal way to manage your application as a whole.

## 1. Codebase

> One codebase tracked in revision control, many deploys

Your codebase should be tracked in a central version control system that is easily accessible to all your developers. We prefer to use [Git](https://git-scm.com/) and share all our repositories through [GitHub](https://www.github.com/).

There is only one codebase per app, but there will be many *deploys* of the app. This means that you might have *deployed* your application to production and to staging, for example. Both environments share the same codebase, but might be in a different state. Staging could have some commits not yet deployed to production for testing.

## 2. Dependencies

> Explicitly declare and isolate dependencies

Your application might rely on external libraries and packages to run. You should never assume that these packages exist on the target system. Instead, your application must declare all dependencies and their correct version explicitly.

When working with PHP and Joomlatools Platform, we can rely on [Composer](https://www.getcomposer.org/) to install dependencies for us. All we have to do is declare them in the `composer.json` manifest.

## 3. Configuration

> Store config in the environment

Configuration options should never be hardcoded, for two reasons:

1. You do not want sensitive information like database credentials or API keys to be committed into the repository to prevent security leaks.
2. Your configuration varies per environment. For example, you might want to enable debugging on your local environment while this would be overkill on production.

Instead of hardcoding this information, we rely on environment variables to handle this sensitive information.

## 4. Backing Services

> Treat backing services as attached resources

A backing service is one that requires a network connection to run, like MySQL or Memcached. If the location or connection details of such a service changes, you should not have to make code changes. Instead, these details should be available in the configuration.

For example, your development environment talks to a MySQL server on your local machine. On production, your database runs on another server. The only difference will be the URL to connect to in the configuration.

## 5. Build, release, run

> Strictly separate build and run stages

Build, release, and run stages should be treated as completely distinct from one another:

1. The *build* stage is fully controlled by the developer. This is where we tag a new release and fix bugs.
2. The *release* stage prepares the build for execution in the target environment. In this stage, you can run tests to verify if the code behaves as intented.
3. The *run* stage executes the application and should not need any intervention.

For example, it's now impossible to make changes to the runtime. Instead, you make changes to the code in the build stage where you have total control. This reduces risk and ensures your team that everything is running well.

Automation and tooling will help to make this principle simpler. A tool like [GitHub](https://www.github.com/) can be used to tag your latest build, while [Capistrano](http://capistranorb.com/) can be used to deploy the new build to the server.

## 6. Processes

> Execute the app as one or more stateless processes

Stateless applications are designed to degrade gracefully. That means if a part of your application stack fails, the app itself does not become a failure. In other words, the state of your system is completely defined by your databases and shared storage, and not by each individual running application instance.

## 7. Port Binding

> Export services via port binding

Your application service should also be accessible via a URL, just as your [backing services](https://www.joomlatools.com/developer/platform/12-factor-app/#backing-services) are. This enables your application to be fully self-contained.

This means you should be able to run the Joomlatools Platform using PHP's [built-in webserver](https://secure.php.net/manual/en/features.commandline.webserver.php) on your development machine, while your production server can route your domain name to the application by adding a more complicated webserver on top.

## 8. Concurrency

> Scale out via the process model

Every process inside your application should be treated as a first-class citizen. That means that each process should be able to scale, restart, or clone itself when needed. This approach will improve the sustainability and scalability of your application as a whole.

## 9. Disposability

> Maximize robustness with fast startup and graceful shutdown

When you deploy new code, you want the new version to start right away and be able to deal with incoming traffic. This principle is a natural result of the [backing services](https://www.joomlatools.com/developer/platform/12-factor-app/#backing-services) and [concurrency](https://www.joomlatools.com/developer/platform/12-factor-app/#concurrency) principles: after a crash or new deployment, your app should have everything it needs waiting in databases or caches. Reloading the code should only take a few seconds max.

## 10. Dev/prod parity

> Keep development, staging, and production as similar as possible

Your development environment should resemble production as close as possible. That means working on the same operating system, using the same backing services and the same dependencies, and so on. This reduces the number of bugs and downtime and allows your organisation to have a much more rapid development cycle.

That's why we do all our development work on our [Joomlatools Box](https://www.joomlatools.com/developer/tools/vagrant/). Not only does this resemble our production environments, but it also ensures all our developers work with the same tools.

## 11. Logs

> Treat logs as event streams

Logging is important for debugging and checking up on the general health of your application. However, your application should not be concerned with the storage and management of these logs. Instead, log entries should be treated as an event stream that is routed to a separate service for archival and analysis.

Examples of such external services are [New Relic](https://newrelic.com/) or [Logentries](https://logentries.com/).

## 12. Admin Processes

> Run admin/management tasks as one-off processes

Once your application is running in production, you'll want to do a lot of simple administrative tasks from time to time. You could need to run a database migration or fetch analytical data to gather business insights.

One-off admin processes are long-running processes that should be run in an identical environment as the regular long-running processes of the app. They run against the same live release, using the same codebase, configuration and database.

You can use our [Joomlatools Console](https://www.joomlatools.com/developer/tools/console/) to execute one-off tasks, such as [clearing](https://www.joomlatools.com/developer/tools/console/commands/cache/#cacheclear) the cache or [re-indexing](https://www.joomlatools.com/developer/tools/console/commands/finder/#finderindex) Smart Search content. To apply or rollback database migrations we make use of [Phinx](https://phinx.org/).