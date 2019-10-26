# Toil

My notes on usage of [Toil](https://toil.readthedocs.io/en/latest/index.html) - an *open-source pure-Python workflow engine that lets people write better pipelines*.

## Jobs

### Create job

You can create a `Job` by 

- inheriting from `toil.job.Job`, 
- writing a function `f` that has same arguments as the `Job` above would have
  - create `Job` from the function `f` by wrapping with `Job.wrapFn(f, arg1, arg2)`. The `arg1` and `arg2` are arguments accepted by `f`
- writing a Job function `jf` whose first argument is a reference to the wrapping job
  - create `Job` from the function `jf` by wrapping with `Job.wrapJobFn(jf, arg1, arg2)`

### Manage multiple jobs



## Managing files

In Toil the [`toil.fileStore.FileStore`](https://toil.readthedocs.io/en/latest/developingWorkflows/toilAPIFilestore.html#toil.fileStore.FileStore) class is used by jobs to manage persistent and temporary files. Each `Job` has a `fileStore` attribute.

```python
# Create a local temporary file.
scratchFile = job.fileStore.getLocalTempFile()

# Write something in the scratch file.
with open(scratchFile, 'w') as fH:
    fH.write("What a tangled web we weave")

# Write a copy of the file into the file-store; fileID is the key that can be used to retrieve the file.
# This write is asynchronous by default
fileID = job.fileStore.writeGlobalFile(scratchFile)

# Write another file using a stream; fileID2 is the
# key for this second file.
with job.fileStore.writeGlobalFileStream(cleanup=True) as (fH, fileID2):
    if sys.version_info >= (3, 0):
        # if python 3
        fH.write(b"Out brief candle")
    else:
        # if python 2
        fH.write("Out brief candle")

# Now read the first file; scratchFile2 is a local copy of the file that is read-only by default.
scratchFile2 = job.fileStore.readGlobalFile(fileID)

# Read the second file to a desired location: scratchFile3.
scratchFile3 = os.path.join(job.tempDir, "foo.txt")
job.fileStore.readGlobalFile(fileID2, userPath=scratchFile3)

# Read the second file again using a stream.
with job.fileStore.readGlobalFileStream(fileID2) as fH:
    print(fH.read()) #This prints "Out brief candle"

# Delete the first file from the global file-store.
job.fileStore.deleteGlobalFile(fileID)
```

## Start pipeline

This is some working boilerplate which can be stored in `example.py`

```python
from toil.common import Toil
from toil.job import Job
from argparse import ArgumentParser

def beer(job, message):
    job.fileStore.logToMaster("Which beer do you like? {}".format(message))

def main(options=None):
    if not options:
        # deal with command line arguments
        parser = ArgumentParser(description="The pipeline description")
        Job.Runner.addToilOptions(parser)
        parser.add_argument("beer", default="Urpiner", help="Which beer?")
        

        options = parser.parse_args()
    
    options.logLevel = "INFO"
    options.clean = "always"
    
    with Toil(options) as toil:
        toil.start(Job.wrapJobFn(beer, options.beer))


if __name__ == "__main__":
    main()
```

> Run by `python example.py file:my-file-store "Pilsner Urquell"`