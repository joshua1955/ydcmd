# ydcmd

Console client for working with Yandex.Disk cloud storage via REST API.

> **This is a fork of the project by [abbat](https://github.com/abbat/ydcmd)**

## ⚠️ Important changes in this version

**Updated to work with current Yandex.Disk API (2024)**

### 🔧 Key fixes:
- **Updated base API URL**: `cloud-api.yandex.net` → `cloud-api.yandex.com`
- **Fixed Python 3 compatibility**: resolved unicode and JSON issues
- **Improved OAuth token handling**: added token validation
- **Fixed SSL work**: added support for working without certificate verification
- **Improved error handling**: more informative error messages

### 🚀 Result:
- ✅ Full compatibility with current Yandex.Disk API
- ✅ Works with Python 2.7+ and Python 3.x
- ✅ Correct OAuth token handling
- ✅ Stable operation on Windows/Linux/macOS

## Installation

From source code:

```bash
git clone https://github.com/your-username/ydcmd.git
cd ydcmd
sudo cp ydcmd.py /usr/local/bin/ydcmd
```

## How you can help

* Translate this document to your native language
* Fix errors in documentation
* Share information with your friends
* Submit PR if you are a developer

## Setup

To work with the client, you need to obtain an OAuth token. **Simple method** - use the built-in command:

```bash
python ydcmd.py token
```

The command will output a beautiful instruction and authorization link. Simply:

1. Follow the link in your browser
2. Grant access to the application
3. Copy the authorization code from the page
4. Paste it in the terminal and press Enter

The script will automatically:
- ✅ Get OAuth token
- ✅ Create or update configuration file `~/.ydcmd.cfg`
- ✅ Save token in secure format

After that you can immediately use all ydcmd commands!

### Alternative method (registering your own application):

1. Register an application on Yandex
2. Set permissions: `Yandex.Disk REST API`
3. Enable `Development client`
4. Use the received `client_id` for authorization

## Usage

You can get brief help in the console by running the script without parameters or with the `help` command. General call format:

```
ydcmd [command] [options] [arguments]
```

**Commands**:

* `help` - get brief help on commands and application options;
* `ls` - get list of files and directories;
* `rm` - delete file or directory;
* `cp` - copy file or directory;
* `mv` - move file or directory;
* `put` - upload file or directory to storage;
* `get` - get file or directory from storage;
* `cat` - output file from storage to stdout;
* `mkdir` - create directory;
* `stat` - get metadata about object;
* `info` - get metadata about storage;
* `last` - get metadata about last uploaded files;
* `share` - publish object (get direct link);
* `revoke` - close access to previously published object;
* `du` - estimate space occupied by files in storage;
* `clean` - clean files and directories;
* `restore` - restore file or directory from trash;
* `download` - download file from internet to storage;
* `token` - get oauth token for application work.

**Options**:

* `--config=<S>` - configuration file name (if different from default file);
* `--timeout=<N>` - timeout in seconds for establishing network connection;
* `--retries=<N>` - number of attempts to call api method before returning error code;
* `--delay=<N>` - timeout between attempts to call api method in seconds;
* `--limit=<N>` - number of elements returned by one call to get list of files and directories;
* `--token=<S>` - oauth token (for security reasons, it is recommended to specify in configuration file or through environment variable `YDCMD_TOKEN`);
* `--quiet` - suppress error output, operation success result is determined by return code;
* `--verbose` - output extended information;
* `--debug` - output debug information;
* `--chunk=<N>` - data block size in KB for input/output operations;
* `--ca-file=<S>` - name of file with certificates of trusted certification centers (if empty, certificate validity check is not performed);
* `--ciphers=<S>` - set of encryption algorithms;
* `--version` - output version and exit.

### Getting list of files and directories

```
ydcmd ls [options] [disk:/object]
```

**Options**:

* `--human` - output file size in human-readable format;
* `--short` - output list of files and directories without additional information (one name per line);
* `--long` - output extended list (creation time, modification time, size, file name).

If target object is not specified, root directory of storage will be used.

### Deleting file or directory

```
ydcmd rm <disk:/object>
```

**Options**:

* `--trash` - delete to trash;
* `--poll=<N>` - time in seconds between status polling when performing asynchronous operation;
* `--async` - execute command without waiting for completion (`poll`) operation.

Files are deleted without possibility of recovery. Directories are deleted recursively (including nested files and directories).

### Copying file or directory

```
ydcmd cp <disk:/object1> <disk:/object2>
```

**Options**:

* `--poll=<N>` - time in seconds between status polling when performing asynchronous operations;
* `--async` - execute command without waiting for completion (`poll`) operation.

In case of name collision, directories and files will be overwritten. Directories are copied recursively (including nested files and directories).

### Moving file or directory

```
ydcmd mv <disk:/object1> <disk:/object2>
```

**Options**:

* `--poll=<N>` - time in seconds between status polling when performing asynchronous operations;
* `--async` - execute command without waiting for completion (`poll`) operation.

In case of name collision, directories and files will be overwritten.

### Uploading file to storage

```
ydcmd put <file> [disk:/object]
```

**Options**:

* `--rsync` - synchronize tree of files and directories in storage with local tree;
* `--no-recursion` - do not upload contents of nested directories;
* `--no-recursion-tag=<S>` - do not upload contents of nested directories, for directories containing file;
* `--exclude-tag=<S>` - skip uploading directories containing file;
* `--skip-hash` - skip md5/sha256 integrity checks;
* `--threads=<N>` - number of worker processes;
* `--iconv=<S>` - if necessary, try to restore file and directory names from specified encoding (e.g., `--iconv=cp1251`);
* `--progress` - output operation progress (enabled by default, recommended to install python-progressbar module).

If target object is not specified, root directory of storage will be used for file upload. If target object points to directory (ends with `/`), source file name will be added to directory name. If target object exists, it will be overwritten without confirmation request. Symbolic links are ignored.

### Getting file from storage

```
ydcmd get <disk:/object> [file]
```

**Options**:

* `--rsync` - synchronize local tree of files and directories with tree in storage;
* `--no-recursion` - do not download contents of nested directories;
* `--skip-hash` - skip md5/sha256 integrity checks;
* `--threads=<N>` - number of worker processes;
* `--progress` - output operation progress (enabled by default, recommended to install python-progressbar module).

If target file name is not specified, file name in storage will be used. If target object exists, it will be overwritten without confirmation request.

### Output file from storage to stdout

```
ydcmd cat <disk:/object>
```

### Creating directory

```
ydcmd mkdir <disk:/path>
```

### Getting metadata about object

```
ydcmd stat [disk:/object]
```

If target object is not specified, root directory of storage will be used.

### Getting metadata about storage

```
ydcmd info
```

**Options**:

* `--long` - display sizes in bytes instead of human-readable format.

### Getting metadata about last uploaded files

```
ydcmd last [N]
```

**Options**:

* `--human` - output file size in human-readable format;
* `--short` - output list of files without additional information (one name per line);
* `--long` - output extended list (creation time, modification time, size, file).

If parameter N is not set, default value from REST API will be used.

### Publishing object

```
ydcmd share <disk:/object>
```

Command returns object name in storage and link to it.

### Closing access

```
ydcmd revoke <disk:/object>
```

### Estimating occupied space

```
ydcmd du [disk:/object]
```

**Options**:

* `--depth=<N>` - display directory sizes up to level N;
* `--long` - display sizes in bytes instead of human-readable format.

If target object is not specified, root directory of storage will be used.

### Cleaning files and directories

```
ydcmd clean <options> [disk:/object]
```

**Options**:

* `--dry` - do not perform deletion, but output list of objects for deletion;
* `--type=<S>` - type of objects for deletion (`file` - files, `dir` - directories, `all` - all);
* `--keep=<S>` - criterion for selecting objects that need to be preserved:
  * To select date **before** which data needs to be deleted, you can use date string in ISO format (e.g., `2014-02-12T12:19:05+04:00`);
  * To select relative time, you can use number and dimension (e.g., `7d`, `4w`, `1m`, `1y`);
  * To select number of copies, you can use number without dimension (e.g., `31`).

If target object is not specified, root directory of storage will be used. Sorting and filtering of objects is performed by modification date (not creation date).

### Restoring file or directory from trash

```
ydcmd restore <trash:/object> [name]
```

**Options**:

* `--poll=<N>` - time in seconds between status polling when performing asynchronous operations;
* `--async` - execute command without waiting for completion (`poll`) operation.

In case of name collision, directories and files will be overwritten. Directories are restored recursively (including nested files and directories).

### Downloading file from internet to storage

```
ydcmd download <URL> [disk:/object]
```

**Options**:

* `--poll=<N>` - time in seconds between status polling when performing asynchronous operations;
* `--async` - execute command without waiting for completion (`poll`) operation;
* `--no-redirects` - prohibit redirects during download.

If target object is not specified, root directory of storage will be used, and file name will be chosen based on URL content (if possible).

### Getting OAuth token

```
ydcmd token [code]
```

Without specifying argument, command will output link for getting code. Open link in browser, grant access to application and use received code as argument to get OAuth token.

## Configuration

For convenience, it is recommended to create configuration file named `~/.ydcmd.cfg` and set permissions `0600` or `0400` on it. File format:

```ini
[ydcmd]
# comment
<option> = <value>
```

For example:

```ini
[ydcmd]
token   = your_oauth_token
verbose = yes
ca-file = /etc/ssl/certs/ca-certificates.crt
```

### ⚠️ Important settings:

- **`token`** - OAuth token (required for operation)
- **`ca-file`** - path to SSL certificates file (optional)
  - If not specified, application works without SSL certificate verification
  - On Windows may need to be left empty: `ca-file = `

## Environment variables

* `YDCMD_TOKEN` - oauth token, has priority over `--token` option;
* `SSL_CERT_FILE` - name of file with certificates of trusted certification centers, has priority over `ca-file` option.

## Exit codes

When working in automatic mode (via cron), it may be useful to get command execution result:

* `0` - successful completion;
* `1` - general application error;
* `4` - HTTP-4xx status code (client error);
* `5` - HTTP-5xx status code (server error).