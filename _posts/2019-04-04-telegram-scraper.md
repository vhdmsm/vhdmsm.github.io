---
layout: post
title: کراولر تلگرام
published: true
tags: telegram crawler python
---

{% include image.html
url="https://simply-communicate.com/wp-content/uploads/2018/04/maxresdefault.jpg"
%} احتمالا تا حالا شده که دلتون بخواد تلگرام رو کراول کنید و پست‌های
کانال‌ها، گروه‌ها و یا چت‌ها رو توی دیتابیس سیو کنید که بعدا شاید بخواید 
روش کار تحلیلی انجام بدید. تو این پست می‌خوام بگم که چطور خیلی ساده
می‌تونید این کار رو انجام بدید.

پروژه
[tg-cli](https://github.com/vysheng/tg) اولین پروژه‌ای بود که به صورت
غیر رسمی شروع به کار کرد و این امکان رو می‌داد که با استفاده از API
تلگرام بهش متصل بشید (مثل کلاینت تلگرامی که روی سیستم‌تون دارید) و
پیام‌ها رو به صورت json از طریق cli دریافت کنید. اما این پروژه چندین سال
پیش متوقف شد و دیگه ادامه پیدا نکرد و در نتیجه با آپدیت‌های جدید تلگرام
مثل استیکرها، پیغام‌های پین‌شده، bio کاربران و غیره سازگار نیست و در
نتیجه crash می‌کنه. منم چون یه زمانی دوس داشتم که روی تلگرام کار کنم یه
بخشی از باگ‌های پروژه
[tg-cli](https://github.com/vysheng/tg) رو فیکس کردم و با استفاده از
پایتون یه
[کراولری](https://github.com/vhdmsm/tg_crawler) روش ساختم که هنوز هم کار
می‌کنه اما با توجه به اینکه دیگه پروژه tg-cli متوقف شده استفاده از
پروژه‌ی من دیگه خیلی توصیه نمی‌شه و به جاش می‌خوام
[telegram-export](https://github.com/expectocode/telegram-export) که به
وسیله کلاینت جدیدی به اسم
[Telethon](https://github.com/LonamiWebs/Telethon) نوشته شده رو معرفی
کنم.

کلاینت Telethon با آپدیت‌های جدید تلگرام هم سازگاره و چیزی که خیلی جذابش
می‌کنه پایتون بودنشه 😍 (همینجطور که می‌بینید من عاشق پایتونم 😂).
همونطور که بالاتر اشاره کردم یه کراولر خوبی به وسیله پایتون و با استفاده
از Telethon نوشته شده که من یه باگ کوچیکشو فیکس کردم و فیچری بهش اضافه
کردم که اجازه میده انتخاب کنید که پیغام‌های کانال، گروه و یا چت شخصی
کراول بشه و که از طریق این
[لینک](https://github.com/expectocode/telegram-export) در دسترس هست.
برای نصب نسخه فورک‌شده‌ی من، بعد از clone پروژه و ترجیحا ایجاد یک
virtualenv، باید کتابخونه رو با اجرای کامند `python setup.py install `
نصب کنید. بعد از نصب باید فایل config.ini که sample اش داخل پروژه
telegram-export وجود داره رو داخل مسیر پروژتون ایجاد کنید که یه نمونه از
این فایل رو پایین‌تر گذاشتم و بخش‌هایی که برای راه‌انداختن باید پرشون کنید
ApiId، ApiHash، PhoneNumber و FromType هست. ApiId و ApiHash رو می‌تونید
از اکانت تلگرامتون از طریق
[my.telegram.org](https://my.telegram.org/) درخواست بدید. شماره هم وارد
می‌کنید و از طریق FromType مشخص می‌کنید که می‌خواید پیغام‌های چه جاهایی مثل
گروه، کانال یا چت شخصی گرفته بشه و در آخر کامند `telegram-export` رو
اجرا می‌کنید.
 
به عنوان یه نکته کوچیک درآخر، اگر نمی‌خواید که مدیا مثل عکس یا ویدئو
دانلود بشه، MaxSize رو صفر بذارید.
```
#!/usr/bin/env python

# Configuration file for telegram-export. Default values are commented.
# This file should be copied to config.ini and edited.
# Use ; or # for comment lines

[TelegramAPI]

################ You MUST edit these values to use the exporter #################
# You can either get your own from my.telegram.org, or use a published one (easier):
# https://github.com/telegramdesktop/tdesktop/blob/dev/Telegram/SourceFiles/config.h#L222
# https://git.io/vADys (permanent link)
ApiId = <Your API ID>
ApiHash = <Your API Hash>

# Your phone number. You must supply this. Should start with +country code eg +44.
PhoneNumber = +989<Your Phone Number>

# You can store your 2FA Password here if you don't want to enter it when logging in.
# It's more secure to not store your password in plain text
# SecondFactorPassword = xxxxxxxxxx

# This can be anything
; SessionName = exporter
SessionName = exporter

[Dumper]

# Types chan be group, user, channel
FromType = user, channel

# Output folder, where the database, media, and cache files will be stored.
# You can leave this as . to save to your current directory (when you call the
# program), or put a path like ~/Downloads or /home/username/tg-export/. It is
# usually a better idea to set a specific directory here.
OutputDirectory = .

# Either Whitelist or Blacklist should be present, not both. If both are
# present, only Whitelist will be used.
# These are lists of "entities", which can be usernames, phone numbers, or
# telegram IDs.
# - Whitelist will backup only the entities listed.
# - Blacklist will backup everything except the ones listed.
# It's usually  a good idea to set a whitelist, as otherwise you will have
# to wait for lots of dialogs you don't care about to be downloaded. You can
# get a list of dialogs and their associated IDs by running
# `telegram-export.py --list-dialogs`
# # The list must be a comma separated list of entities. You may have an
# optional comment on an entry by using the : symbol. For example,
#
# Blacklist = @username : this is a comment, -1001132836449 :another comment,
# +12345678, +232525252 : the previous phone number had no comment.
# Phones must start with '+'  as shown. Usernames can be with or without @
# Don't forget your commas! If you miss one after a comment, everything until
# the next comma will be treated as a comment.


############################# 'Advanced Options' #############################

# The file types to download, comma separated. Options are:
# "photo", "document", "video", "audio", "sticker", "voice", "chatphoto".
# An empty list (default if omitted) means all are allowed.
# Note that "chatphoto" includes profile pictures as well.
; MediaWhitelist = chatphoto, photo, sticker
MediaWhitelist = chatphoto, photo, sticker

# The maximum allowed file size for a document before skipping it.
# For instance, "800KB" will only download files smaller or equal to 800KB.
# Allowed units are "B", "KB", "MB" and "GB" (decimal point allowed).
#
# No unit defaults to "MB". Setting to "0"  will not download any media.
# Note that this only applies to documents (everything but normal photos).
; MaxSize = 1MB
MaxSize = 0

# Sets the log level used across the entire dumper. Supported values
# are the same available in the "logging" module. Defaults to DEBUG.
# Available levels (less to more verbose): ERROR, WARNING, INFO, DEBUG, NOTSET
; LogLevel = INFO
LogLevel = INFO

# Database filename (without '.db')
; DBFileName = export
DBFileName = scraper

# The format string to be used when downloading media. You can use any literal
# string you wish in the name, relative names (including directories, separated
# by the '/' character) or absolute paths. Anything inside {} will be replaced
# with a proper value, and possible placeholders are:
# {sender_id}   - Sender ID
# {context_id}  - Context ID
# {name}        - Sanitized name of the context (chat)
# {filename}    - Sanitized name of the file
# {sender_name} - Sanitized sender name
# {type}        - The media type (e.g. photo, document, video...)
#
# For instance, you could do:
#   MediaFilenameFmt = "usermedia/{name}/{type}/{filename}"
#
# That would save files under "usermedia/Chat Name/media type/media file".
# The extension will always be added automatically as a pair of ".ID.EXT". This
# allows the program to ensure that duplicate files won't be downloaded even
# if you change the format string at a later point. You shouldn't change this.
#
# To format the date of the mesage, you can use the format specifiers
# described under the following link anywhere you wish:
# https://docs.python.org/3.5/library/datetime.html#strftime-and-strptime-behavior
# eg. for the year you would put %Y in the format string and for a literal %
# you would put %%, though that would be a bit weird.
; MediaFilenameFmt = usermedia/{name}-{context_id}/{type}-{filename}
MediaFilenameFmt = usermedia/{name}-{context_id}/{type}-{filename}

# Time after which an unchanged user should be dumped anyway, to avoid a long
# information gap (see EXPLANATIONS.md). In minutes.
; InvalidationTime = 7200
InvalidationTime = 7200

# Chunk size in which to retrieve messages. 100 (default, max) if not present.
; ChunkSize = 100

# Maximum chunks to retrieve from a chat (if too many). 0 (default) means all.
; MaxChunks = 0

# Sets the log level used across libaries (excluding the dumper).
# Accepts the same values as LogLevel
; LibraryLogLevel = WARNING
LibraryLogLevel = WARNING

# Sets proxy support
# Proxy = socks5://user:password@127.0.0.1:1080
# Proxy = http://127.0.0.1:8080
```