# ぱっとタイムアウトテスト用 Web サーバを立てる

タイムアウトテスト用 Web サーバを irb で.

```ruby
require 'webrick'
s = WEBrick::HTTPServer.new(:Port => 8000)
s.mount_proc('/') {|req, res|
  sleep 300
  res['Content-Type'] = 'text/plain'
  res.body = '300 secs passed'
}
trap(:INT){s.shutdown}
s.start
```

irb に流し込んだ後は http://localhost:8000/ にアクセスすれば OK.
