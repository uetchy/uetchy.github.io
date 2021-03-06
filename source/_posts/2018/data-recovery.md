---
title: Guide to recover data from damaged HDD
date: 2018-02-15 12:45:00 +09:00
redirect_from: "/blog/2018/02/15/data-recovery"
---

This is a complete guide describes how to rescue your data from old and wrecked HDD.

Suppose you are using macOS and your endangered disk is formatted with HFS+.

# Beforehand

## Inspect

Use `diskutil list` to verify that which drive is damaged.

This article assumes that `disk2` is the damaged AND a partition `disk2s2` is what you expected to be rescued. You don't want to save `disk2s1` that is usually EFI partition.

## Damage Control

To prevent extra load, unmount the damaged disk: `diskutil unmountDisk disk2`.

# Rescue

If you never been `ddrescue`, `brew install ddrescue` to install them on your machine.

```bash
cd /Volumes/AnotherDriveLargerThanDamagedDrive
sudo ddrescue -n -v /dev/disk2s2 ./hdimage.dmg ./mapfile
```

So this command will start rescuing your data from `/dev/disk2s2` partition to `hdimage.dmg` while writing log to `mapfile`.

You might want to rescue data as fast as possible. option `-n` is here for. This will skip scraping phase that causes aggressive disk access.

Option `-v` stand for verbose logging.

```bash
sudo ddrescue -r5 -v /dev/rdisk2s2 ./hdimage.dmg ./mapfile
```

When the first command completed, do it again with different parameters to aggressively scrape bad area failed to access the first time.

Option `-r5` means ddrescue will try rescuing damaged area for 5 times.

And `/dev/disk2s2` become `/dev/rdisk2s2` this time. `r` stand for raw so this will access the disk more direct way.

> Beware: You MUST use same `hdimage.dmg` and `mapfile` between two commands. `mapfile` remains information of which blocks were rescued.

# Aftercare

Mount `hdimage.dmg` and copy files and directories to a new drive. If the image is broken, you can recover it using `testdisk`.
