
# Weekly Script

This little script helps when you need to run a certain script (like a backup)
weekly. I use this to backup my databases and keep a week of window between
them. I do my backups twice a day, from Monday through Friday, and the backup
done in Monday will only get overridden in the next Monday.

## Using It

Clone this repository

```bash
    git clone https://github.com/nerde/weekly.git
```

Create your script:

```bash
    cd weekly
    cp execute.example execute
    vim execute
```

In it, type whatever you want to do when the script gets run. The first
parameter in this script will be a destination directory guessed by the main
script (we'll talk about this one later):

```bash
    echo $1 # Will print something like ./2-Tuesday_08-33
```

The second one will contain just the "day" part:

```bash
    echo $2 # Will print something like 2-Tuesday_08-33
```

## Running It

To run this, you will not call directly your own `execute` script. You will
call the main script instead:

```bash
    ./run
```

By default, the destination path will be where the `run` script is, concatenated
with the "day/hour" part. You may want to change this behaviour. To do that,
you just need to pass a single parameter to the `run` script in order to tell it
where you want the destination to be:

```bash
    ./run ~/backup
```

This way, the first parameter in the `execute` script will be
`/home/user/backup/2-Tuesday_08-33`. The second one will be the same.

## Crontab

Now, the magic really happens when you use it with your crontab. There, you
can define when you want the script to be run:

```bash
    crontab -e
```

To run my backups, I use something like this:

```
30 12,19 * * 1-5 /path/to/weekly/run /home/user/backup/
```

This way, my `/home/user/backup` folder will have the following directories:

```
    1-Monday_12-30
    1-Monday_19-30
    2-Tuesday_12-30
    2-Tuesday_19-30
    3-Wednesday_12-30
    3-Wednesday_19-30
    4-Thursday_12-30
    4-Thursday_19-30
    5-Friday_12-30
    5-Friday_19-30
```

In each one I have the corresponding backup and it will get replaced only once
a week. This way, if I have to go back in time, I can do it up to one week ago.
