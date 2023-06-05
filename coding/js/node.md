
[zlib](https://nodejs.dev/en/api/v19/zlib/)

[stream](https://nodejs.dev/en/api/v19/stream/)

[child process](https://nodejs.dev/en/api/v19/child_process/)

[events](https://nodejs.dev/en/api/v19/events/) - I love this

[promisify](https://nodejs.org/docs/latest-v14.x/api/util.html#util_util_promisify_original)

[pipeline](https://nodejs.org/docs/latest-v14.x/api/stream.html#stream_stream_pipeline_streams_callback)

[isDeepStrictEqual](https://nodejs.org/docs/latest-v14.x/api/util.html#util_util_isdeepstrictequal_val1_val2)

remote debugging
```bash
# make sure port in open on remote
ssh -L 9229:localhost:9229 user@remote.example.com
chrome://inspect
node --inspect /path/to/file.js
```

profiling
```shell
node --prof app.js # this will output a tick file
node --prof-process isolate-0xnnnnnnnnnnnn-v8.log # open tick file
```
## NPM

npm link
>linking will link same version of node packages
```
cd /path/to/dep
npm link
cd /path/to/app
npm link dep
```