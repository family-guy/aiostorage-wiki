---
title: Basic Usage
order: 1
---

As `aiostorage` is asynchronous, any interaction with it needs to 
make use of `asyncio`

```
import asyncio
from aiostorage import Backblaze
```

As we are dealing with coroutines, and coroutines can only be called by 
other coroutines or an event loop, a top-level function `run()` needs to
 be defined and run inside an event loop

```
loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```

Suppose we have some files that we would like to upload.

In `run()`, we create a storage instance, supply our 
cloud storage credentials, and loop through the files, appending each one as
 a `future` to a `list`

```
async def run():
    storage = Backblaze(
        account_id='my-account-id',
        app_key='my-app-key'
    )
    futures = []
    for file in files:
        future = asyncio.ensure_future(my_upload_file(storage, file))
        futures.append(future)
    await asyncio.wait(futures)
```

where `my_upload_file` is a helper coroutine that does the authenticating and 
uploading of each file to a given bucket

```
async def my_upload_file(storage, file):
    await storage.authenticate()
    bucket_id = 'my-bucket-id'
    await storage.upload_file(bucket_id, file['path'], file['content_type'])
```

The key line is:

`await asyncio.wait(futures)`

It ensures the files are 
uploaded in parallel (but in the same thread), i.e. the time taken to upload
 all files roughly equals the time taken to upload the slowest 
 file.
 
**Caveat:** Any time gains will depend on how
 the cloud storage provider handles such behaviour. Further, in 
 extreme cases, files may not be able to be uploaded at all.