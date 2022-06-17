# dockerのインストール

以下を実行してください。

```
bash ./docker-install.sh
```


## Rules

```
点数 = サービス維持 + フラグ獲得 + フラグ流出
```

サービス維持 : 10 pt ~ 0 pt

フラグ獲得 : +10 pt / 1 flag

フラグ流出 : 0 pt / 1 flag

## Score Server

### スコアサーバー (80番ポート)

<http://10.1.1.1>

score visualizer


### APIサーバー (3000番ポート)

<http://10.1.1.1:3000>


## Setup

ユーザー名とパスワードが管理者から渡されます。
API サーバーに問い合わせて API Key と IP アドレスを取得して下さい。

```
$ curl -d name="1" -d passwd="ndkwbvie" "http://10.1.1.1:3000/user/login"
```

<details>
<summary>別実装</summary>

```
$ http -f POST "http://10.1.1.1:3000/user/login" name="1" passwd="ndkwbvie"
```

```python
import urllib.parse
import urllib.request

url = "http://10.1.1.1:3000/user/login"
values = {"name": "1", "passwd": "ndkwbvie"}

data = urllib.parse.urlencode(values)
data = data.encode("ascii")
req = urllib.request.Request(url, data)
with urllib.request.urlopen(req) as response:
    res = response.read().decode()
    print(res)
```

</details>

```
{"key":"615868b579f5a7d0394cda0e8e9e0f81","ip":"192.168.0.11"}
```

## Flag

フラグをスコアサーバーに提出する

```
$ curl -gH "x-api-key:615868b579f5a7d0394cda0e8e9e0f81" -d flag="FLAG{eyJ1IjoiNSIsInMiOiIxIiwiZSI6IjEyIn0.w8EbP6Co/dcFzIzjFIhejxREFPARZ2nPfdBbZKAn3QI}" "http://10.1.1.1:3000/flag/submit"
```

<details>
<summary>別実装</summary>

```
$ http -f POST "http://10.1.1.1:3000/flag/submit" X-API-Key:"615868b579f5a7d0394cda0e8e9e0f81" flag="FLAG{eyJ1IjoiNSIsInMiOiIxIiwiZSI6IjEyIn0.w8EbP6Co/dcFzIzjFIhejxREFPARZ2nPfdBbZKAn3QI}"
```

```python
import urllib.parse
import urllib.request

api_key = "615868b579f5a7d0394cda0e8e9e0f81"
flag = "FLAG{eyJ1IjoiNSIsInMiOiIxIiwiZSI6IjEyIn0.w8EbP6Co/dcFzIzjFIhejxREFPARZ2nPfdBbZKAn3QI}"

url = "http://10.1.1.1:3000/flag/submit"
values = {"flag": flag}
headers = {"x-api-key": api_key}
data = urllib.parse.urlencode(values).encode("ascii")

req = urllib.request.Request(url, data, headers)
with urllib.request.urlopen(req) as response:
    res = response.read().decode()
    print(res)
```

</details>

```
{"message":"You get a flag."}
{"message":"This flag has already taken."}
{"message":"This flag is yours."}
{"message":"This flag is wrong."}
{"message":"API key is wrong."}
```

## Score

```
$ curl "http://10.1.1.1:3000/score"
```
