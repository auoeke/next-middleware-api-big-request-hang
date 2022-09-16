`middleware.js`:
```js
export default function middleware() {}
```
`pages/api/api.js`:
```js
export default async function api(request, response) {
    response.status(200).end();
}
```
`DELETE`, `POST` and `PUT` requests to the API endpoint with a body size of 16 KiB or greater don't receive a response. The command below causes cURL to hang until the server exits.
```sh
$ curl -i localhost:3000/api/api -d "$(printf %16384s)"
# Meanwhile I stopped the server.
curl: (52) Empty reply from server
```
Requests to the root are fine:
```sh
$ curl -i localhost:3000 -d "$(printf %16384s)"
HTTP/1.1 200 OK
Cache-Control: no-store, must-revalidate
X-Powered-By: Next.js
Content-Type: text/html; charset=utf-8
Vary: Accept-Encoding
Date: Fri, 16 Sep 2022 19:05:53 GMT
Connection: keep-alive
Keep-Alive: timeout=5
Transfer-Encoding: chunked

...
```
