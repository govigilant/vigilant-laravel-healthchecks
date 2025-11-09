# Vigilant Laravel Healthchecks

A package to integrate [Vigilant](https://github.com/govigilant/vigilant)'s healthchecks with Laravel.

## Features

- 🔍 **Health Checks**: Monitor databases, cache, queues, Redis, Horizon, scheduler, and more
- 📊 **System Metrics**: Track CPU, memory, disk usage, and database size
- 🛠️ **Customizable**: Add custom checks and middleware

## Installation

Install the package via Composer:

```bash
composer require govigilant/vigilant-laravel-healthchecks
```

## Configuration

Publish the configuration file:

```bash
php artisan vendor:publish --provider="Vigilant\Healthchecks\ServiceProvider"
```

This creates `config/vigilant-healthchecks.php` where you can customize checks and metrics.

Set the API token in your `.env` file:

```env
VIGILANT_HEALTHCHECK_TOKEN=your-vigilant-api-key-here
```

### Scheduler

This package automatically schedules a command and a job to verify if your sheduler and queue workers are running.
If you do not want or want to customize this behavior, you can disable the automatic scheduling in the config file by setting `schedule` to `false`.


## Usage

### Accessing the Health Endpoint

Once installed, the health check endpoint is available with your configured token at:

```
POST /api/vigilant/health
```

### Basic Configuration

Configure checks and metrics in `config/vigilant-healthchecks.php`:

```php
use Vigilant\Healthchecks\Checks\DatabaseCheck;
use Vigilant\Healthchecks\Checks\CacheCheck;
use Vigilant\Healthchecks\Checks\RedisCheck;
use Vigilant\Healthchecks\Checks\Metrics\CpuLoadMetric;
use Vigilant\Healthchecks\Checks\Metrics\MemoryUsageMetric;

return [
    'checks' => [
        DatabaseCheck::make(),
        CacheCheck::make(),
        RedisCheck::make(),
        HorizonCheck::make(),
        SchedulerCheck::make(),
    ],

    'metrics' => [
        CpuLoadMetric::make(),
        MemoryUsageMetric::make(),
        DiskUsageMetric::make(),
    ],

    'schedule' => true,
];
```

### Configuring Specific Connections

Check specific database connections, cache stores, or Redis connections:

```php
use Vigilant\Healthchecks\Checks\DatabaseCheck;
use Vigilant\Healthchecks\Checks\CacheCheck;
use Vigilant\Healthchecks\Checks\RedisCheck;

return [
    'checks' => [
        // Check default database connection
        DatabaseCheck::make(),

        // Check a specific database connection
        DatabaseCheck::configure('mysql'),

        // Check default cache store
        CacheCheck::make(),

        // Check a specific cache store
        CacheCheck::configure('redis'),

        // Check default Redis connection
        RedisCheck::make(),

        // Check a specific Redis connection
        RedisCheck::configure('sessions'),
    ],
];
```

## Available Checks

| Check | Description |
|-------|-------------|
| **DatabaseCheck** | Verifies database connection and query execution |
| **CacheCheck** | Tests cache read/write operations |
| **RedisCheck** | Verifies Redis connection health |
| **RedisMemoryCheck** | Monitors Redis max memory usage |
| **QueueCheck** | Checks queue workers are processing jobs |
| **HorizonCheck** | Verifies Laravel Horizon is running |
| **SchedulerCheck** | Ensures Laravel scheduler is active |
| **StorageCheck** | Validates storage directory permissions |
| **DiskSpaceCheck** | Monitors available disk space |
| **DebugModeCheck** | Warns if debug mode is enabled in production |
| **EnvCheck** | Validates environment configuration |

## Available Metrics

| Metric | Description |
|--------|-------------|
| **CpuLoadMetric** | Current CPU load average |
| **MemoryUsageMetric** | System memory usage percentage |
| **DiskUsageMetric** | Disk space usage percentage |
| **DatabaseSizeMetric** | Total database size |
| **LogFileSizeMetric** | Laravel log file size |

## Quality

Run the quality checks:

```bash
composer quality
```

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Vincent Boon](https://github.com/VincentBean)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.
