# safari webm streaming bug reproduction

This repo demonstrates the following bug in Safari 14.1 on Big Sur (tested 2021-09-09, reported to Apple as `FB9611824`):  
When a `video/webm` file is streamed over HTTP, playback fail when the URL does not end in `.webm`.

The result is hosted on <vercel.com> to provide these test URLs. Both serve the same video file with the same headers (`content-type: video/webm`), only the path name is different.

- <https://safari-webm-bug.vercel.app/nasa-iss>
- <https://safari-webm-bug.vercel.app/nasa-iss.webm>

To reproduce locally :

```sh
curl 'https://staging.madek.zhdk.ch/media/4670c653-4883-449c-9a77-6a4240d4dda5' > nasa-iss.webm
cp nasa-iss.webm nasa-iss
echo '{"headers":[{"source":"*","headers":[{"key":"Content-Type","value":"video/webm"}]}]}' > serve.json
npx serve # (needs Node.js)

# works:
open -a Safari 'http://localhost:5000/nasa-iss.webm'

# does not work, even though its the same file and Content-Type is correct:
open -a Safari 'http://localhost:5000/nasa-iss'
```
