<p align="center">
  <a href="https://github.com/mahanmi/iran-clash-example" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Dreamacro/clash/master/docs/logo.png">
      <img width="160" height="160" src="https://raw.githubusercontent.com/Dreamacro/clash/master/docs/logo.png">
    </picture>
  </a>
</p>
<h1 align="center"/>Clash Config Example for Iran</h1>

<p align="center">
    Special Thanks to <a href="https://github.com/hiddify/hiddify-config">Hiddify</a> and <a href="https://github.com/bootmortis/iran-hosted-domains">Bootmortis</a> for providing hosts
</p>

## فهرست مطالب
- [مقدمه](#مقدمه)
  - [قابلیت ها](#قابلیت-ها)
- [آموزش ساخت کانفیگ](#آموزش-ساخت-کانفیگ)
  - [ساخت پروکسی](#ساخت-پروکسی)
  - [اضافه کردن پروکسی به تمپلیت کلش](#اضافه-کردن-پروکسی-به-تمپلیت-کلش)


# مقدمه
کلش یک پروکسی rule-based است که به کمک آن میتوان که برای هر دامین مد نظر rule مشخص کرد تا از پروکسی استفاده کند یا نه و همین قضیه خیلی به کمتر شدن حجم مصرف کاربر و دیرتر فیلتر شدن سرور شما کمک خواهد کرد.

## قابلیت ها
- وصل شدن اتوماتیک به پروکسی با پینگ کمتر
- وصل شدن مستقیم به سایت های ایرانی(هنگام روشن بودن فیلترشکن)
- بلاک کردن سایت های تبلیغات
- دور زدن تحریم
- قابلیت تونل کردن کل سیستم(TUN)

# آموزش ساخت کانفیگ
برای شروع باید از قبل کانفیگ v2ray داشته باشید
## ساخت پروکسی
از فولدر [proxy-example](https://github.com/mahanmi/iran-clash-example/tree/main/example/proxy-example) پروتکل مد نظر خود را انتخاب کنید و طبق آموزش تکمیل کنید.
برای مثال من از vmess-ws-tls  استفاده میکنم
<p align="center">
  <a href="https://github.com/mahanmi/iran-clash-example" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/mahanmi/iran-clash-example/main/.github/images/image%20copy.png">
      <img width="500" height="524" src="https://raw.githubusercontent.com/mahanmi/iran-clash-example/main/.github/images/image%20copy.png">
    </picture>
  </a>
</p>

با توجه به این عکس این اطلاعات رو وارد میکنم

```
#VMESS-WS Protocol
- alterId: 0
  cipher: auto
  benchmark-url: http://www.gstatic.com/generate_204
  benchmark-timeout: 5
  name: "mahan"
  network: ws
  port: 443
  server: psh.clash.shop
  servername: de-server.klyk.shop
  skip-cert-verify: true
  tls: true
  sni: de-sni.clash.shop
  alpn:
   - http/1.1
  type: vmess
  udp: true
  uuid: e8c5d892-47e7-4eeb-b4c3-bd82591d4127
  ws-opts:
    headers:
      Host: de-server.klyk.shop
    path:/mw
```
## اضافه کردن پروکسی به تمپلیت کلش
پروکسی که در بخش قبل ساختیم رو در قسمت proxies اضافه میکنیم
```

proxies:
#VMESS-WS Protocol
- alterId: 0
  cipher: auto
  benchmark-url: http://www.gstatic.com/generate_204
  benchmark-timeout: 5
  name: "mahan"
  network: ws
  port: 443
  server: psh.clash.shop
  servername: de-server.klyk.shop
  skip-cert-verify: true
  tls: true
  sni: de-sni.clash.shop
  alpn:
   - http/1.1
  type: vmess
  udp: true
  uuid: e8c5d892-47e7-4eeb-b4c3-bd82591d4127
  ws-opts:
    headers:
      Host: de-server.klyk.shop
    path:/mw
proxy-groups:
- name: Proxy
  proxies:
  - "♻️ Automatic"
# اینجا رو هم متناسب با اسم پروکسی هایی که ساختید پر کنید
  - "mahan"
  type: select
- interval: 300
  name: OnFiltererdSites
  proxies:
  - Proxy
  - DIRECT
  type: select
- interval: 300
  name: OnSanctionedSites
  proxies:
  - Proxy
  - DIRECT
  type: select
- name: General
  proxies:
  - Proxy
  - DIRECT
  type: select
- interval: 300
  name: OnIranSites
  proxies:
  - DIRECT
  - REJECT
  type: select
- name: "♻️ Automatic"
  type: fallback
  url: 'https://youtube.com'
  interval: 300
  proxies:
# و همچنین اینجا
  - "mahan"
  strategy: consistent-hashing
```
حالا فایل رو با فرمت clash.yaml سیو کنید و داخل نرم افزار کلشتون ایمپورتش کنید.
موفق باشید.