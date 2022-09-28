# Commands

---

### `php artisan messenger:install` | `--uuids` | `--force`
- Installs the base messenger files, publishing the config and service provider. This will also register the published service provider in your `app.php` config file inside the providers array.
  - You will be asked to confirm running this command, as well as an option to run the migrations before completion.
- `--uuids` flag when your provider models use UUIDs instead of auto-incrementing integers as their primary keys.
- `--force` flag to overwrite any existing published files.

---

### `php artisan messenger:attach:messengers` | `--provider=` | `--force`
- Attaches the [Messenger][link-messenger-model] model to your existing registered provider records.
  - By default, this will chunk queries for each of your providers at 100, looping through and checking if a `Messenger` model record exists for each provider, and creating one if it does not.
  - You will be asked to confirm running this command.
- `--provider=` flag you can set the singular provider you want to attach messengers for. Eg: `--provider="App\Models\User"`
- `--force` flag to attach messengers without checking if one exist for each provider.

---

### `php artisan messenger:calls:check-activity` | `--now`
- Check active calls for active participants, end calls with none.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:calls:down` | `--duration=30` | `--now`
- End all active calls and disable the calling system for the specified minutes (30 default).
- `--duration=X` flag to set timeframe in minutes for calling to be disabled.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:calls:up`
- Put the call system back online if it is temporarily disabled.

---

### `php artisan messenger:invites:check-valid` | `--now`
- Check active invites for any past expiration or max use cases and invalidate them.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:purge:documents` | `--now` | `--days=30`
- Purge all soft deleted document messages that were archived past the set days (30 default). The document files will be removed from storage and message models pruned from the database.
- `--days=X` flag to set how many days in the past to start at.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:purge:audio` | `--now` | `--days=30`
- Purge all soft deleted audio messages that were archived past the set days (30 default). The audio files will be removed from storage and message models pruned from the database.
- `--days=X` flag to set how many days in the past to start at.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:purge:videos` | `--now` | `--days=30`
- Purge all soft deleted video messages that were archived past the set days (30 default). The video files will be removed from storage and message models pruned from the database.
- `--days=X` flag to set how many days in the past to start at.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:purge:images` | `--now` | `--days=30`
- Purge all soft deleted image messages that were archived past the set days (30 default). The image files will be removed from storage and message models pruned from the database.
- `--days=X` flag to set how many days in the past to start at.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:purge:messages` | `--days=30`
- Purge all soft deleted messages that were archived past the set days (30 default).
- `--days=X` flag to set how many days in the past to start at.

---

### `php artisan messenger:purge:threads` | `--now` | `--days=30`
- Purge all soft deleted threads that were archived past the set days (30 default). The thread directories and sub files will be removed from storage and the thread models pruned from the database.
- `--days=X` flag to set how many days in the past to start at.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:purge:bots` | `--now` | `--days=30`
- Purge all soft deleted bots that were archived past the set days (30 default). The bot directories and sub files will be removed from storage and the bot models pruned from the database.
- `--days=X` flag to set how many days in the past to start at.
- `--now` flag to run immediately without dispatching jobs to queue.

---

### `php artisan messenger:make:bot {name}`
- Generates a new bot handler class. The class will be placed in your default `App` namespace, under the `Bots` directory. eg: `App\Bots`
- `name` The name of the generated bot handler class.

---

### `php artisan messenger:make:packaged-bot {name}`
- Generates a new packaged bot class. The class will be placed in your default `App` namespace, under the `Bots` directory. eg: `App\Bots`
- `name` The name of the generated packaged bot class.

---

## Example Kernel Scheduler utilizing the commands
```php
<?php

namespace App\Console;

use Illuminate\Console\Scheduling\Schedule;
use Illuminate\Foundation\Console\Kernel as ConsoleKernel;

class Kernel extends ConsoleKernel
{
    /**
     * Define the application's command schedule.
     *
     * @param  \Illuminate\Console\Scheduling\Schedule  $schedule
     * @return void
     */
    protected function schedule(Schedule $schedule)
    {
        $schedule->command('messenger:calls:check-activity')->everyMinute();
        $schedule->command('messenger:invites:check-valid')->everyFifteenMinutes();
        $schedule->command('messenger:purge:threads')->dailyAt('1:00');
        $schedule->command('messenger:purge:messages')->dailyAt('2:00');
        $schedule->command('messenger:purge:images')->dailyAt('3:00');
        $schedule->command('messenger:purge:documents')->dailyAt('4:00');
        $schedule->command('messenger:purge:audio')->dailyAt('5:00');
        $schedule->command('messenger:purge:bots')->dailyAt('6:00');
        $schedule->command('messenger:purge:videos')->dailyAt('7:00');
    }
}
```

[link-messenger-model]: https://github.com/RTippin/messenger/blob/1.x/src/Models/Messenger.php