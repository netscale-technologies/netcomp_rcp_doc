# File uploading

* A core service is available to upload files of any size to the platform.
* Files will be encrypted on upload (using AES128), with an unique 16-byte random password for each one (this can be deactivated if needed)
* A hash (sha256) is calculated on upload and checked on download
* Files can be stored in several backends. Default backend for _binary post_ and _multipart post_ is Amazon S3. Default backend for _api-based creation_ is inlin
with the database (but size is limited to 16KB)
* When uploaded to amazon, files are stored in the following S3 buckets. File name will be the same as returned UID
* There is currently no security implementation yet, so anyone can upload and download files (if they know the file UID)
* Once the file is uploaded, there are a number of APIs available to interact with them (see [Files](files.md))

|env|bucket
|---|---
|dev|nk-rcp-files-dev
|qa|nk-rcp-files-qa
|prod|nk-rcp-files-prod

Base endpoints for uploading/downloadings are:

|env|base
|---|---
|dev|https://files-dev.nk.rcp.care
|qa|https://files-qa.nk.rcp.care
|prod|https://files.nk.rcp.care


## Uploading

There are three ways to upload a file:

1. POST binary request
2. POST multipart upload
3. Direct API call

### POST binary request
   
You must perform a POST request to the address `(base)/upload` with a binary body containing the file, and providing at least headers `content-type` and `length`. 
You can use several query parameters (some of them are mandatory):

|parameter|mandatory|description
|---|---|---
|class|Y|Class of the file (for later lookup)
|type|N|Type or subclass of the file (for later lookup)
|parent_uid|Y|UID of parent object. Used to link the file and perform authentication

If the operation is successful, you will receive a _file UID_ that can be used for later API access or direct download

Example of file upload with curl:

```
curl -v \
--data-binary @~/Downloads/programming-phoenix-liveview_B9.0.pdf \
-H "Content-Type: application/pdf" \
https://files-dev.nk.rcp.care/upload?class=test&parent_uid=123
```

### POST multipart upload

You must perform a POST request to the address `(base)/upload` with a body conforming to a standard http specification
using header `content-type: multipart/form-data; boundary=...`  (using RFC2388, see [example](https://www.w3.org/TR/html401/interact/forms.html#h-17.13.4))

Only one file is allowed per request. Standard variables `name` and `filename`, if provided in header `content-disposition` will be stored along with file's data.
You must provide the same parameters for above method.

### Direct API call

You can also create files using the proxy's API call `upload_file` see (see [Files](files.md))


## Downloading

To download a file, you must perform a GET request to (base)/download/(file_uid)

* File will be search in database
* Storage and specific encryption scheme, password and hash will be retrieved from database
* File will be found, decrypted, hash checked and included in get's response as body
* Headers `content-type` and `content-type` will be populated







