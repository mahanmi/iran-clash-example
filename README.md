<p align="center">
  <a href="https://github.com/mahanmi/iran-clash-example" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Dreamacro/clash/master/docs/logo.png">
      <img width="160" height="160" src="https://raw.githubusercontent.com/Dreamacro/clash/master/docs/logo.png">
    </picture>
  </a>
</p>
<h1 align="center"/>Clash Config Example for Iran</h1>

## فهرست مطالب
- [مقدمه](#مقدمه)
  - [قابلیت ها](#قابلیت-ها)
  - [قوانین (RULE) های استفاده شده](#قوانین-rule-های-استفاده-شده)
- [اضافه کردن تمپلیت کلش به مرزبان](#اضافه-کردن-تمپلیت-کلش-به-مرزبان)
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

## قوانین (RULE) های استفاده شده
- سایت های لوکال(LocalAreaNetwork)

Credit : https://github.com/blackmatrix7/ios_rule_script
```yaml
LocalAreaNetwork:
    type: http
    behavior: classical
    interval: 7200
    path: ./ruleset/LocalAreaNetwork.yaml
    url: https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Lan/Lan.yaml
```
- سایت های فیلتر(Blocked-Sites)

Credit : https://github.com/filteryab/ir-blocked-domain
```yaml
blocked-sites:
    type: http
    behavior: classical
    interval: 7200
    path: ./ruleset/blocked-sites.yaml
    url: https://github.com/mahanmi/iran-clash-example/releases/latest/download/blocked-sites.yaml
  blocked-sites-all:
    type: http
    behavior: domain
    format: text
    interval: 7200
    path: ./ruleset/blocked-sites-all.txt
    url: https://github.com/mahanmi/iran-clash-example/releases/latest/download/blocked-sites-all.txt
```
- سایت های ایرانی(Iran-Sites)

Credit : https://github.com/bootmortis/iran-hosted-domains
```yaml
iran-sites:
    type: http
    behavior: domain
    format: text
    interval: 7200
    path: ./ruleset/iran-sites.txt
    url: "https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/clash_rules_other.txt"
  iran-ads:
    type: http
    format: text
    behavior: domain
    url: "https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/clash_rules_ads.txt"
    path: /ruleset/iran_ads.txt
    interval: 7200
```
- سایت های تحریم(Sactioned-Sites)
```yaml
sanctioned-sites:
    type: http
    behavior: classical
    interval: 7200
    path: ./ruleset/sactioned-sites.yaml
    url: https://github.com/mahanmi/iran-clash-example/releases/latest/download/sanctioned-sites.yaml
```
- سایت های اپل(Apple-Sites)
```yaml
apple-sites:
    type: http
    behavior: domain
    format: text
    interval: 7200
    path: ./ruleset/apple-sites.txt
    url: https://github.com/mahanmi/iran-clash-example/releases/latest/download/apple-sites.txt
```
# اضافه کردن تمپلیت کلش به مرزبان
دانلود تمپلیت روی سرور با کد زیر
```
sudo wget -P /var/lib/marzban/templates/clash/  https://github.com/mahanmi/iran-clash-example/releases/latest/download/marzban-template.yml
```
سپس در فایل .env مشخصات زیر رو اضافه کنید
```
CUSTOM_TEMPLATES_DIRECTORY="/var/lib/marzban/templates/"
CLASH_SUBSCRIPTION_TEMPLATE="clash/marzban-template.yml"
```
از کانفیگ های پیشرفته کلش  خود لذت ببرید.

برای آپدیت تمپلیت کافیست فقط همون کامند اول رو دوباره ران کنید.
# آموزش ساخت کانفیگ
برای شروع باید از قبل کانفیگ v2ray داشته باشید
## ساخت پروکسی
از فولدر [proxy-example](https://github.com/mahanmi/iran-clash-example/tree/main/example/proxy-example) پروتکل مد نظر خود را انتخاب کنید و طبق آموزش تکمیل کنید.
برای مثال من از vmess-ws-tls  استفاده میکنم

<h3>توجه داشته باشید که کلش از پروتکل vless پشتیبانی نمیکنه</h6>
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
