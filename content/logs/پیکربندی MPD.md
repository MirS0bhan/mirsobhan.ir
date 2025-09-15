---
title: پیکربندی MPD
---

‏MPD یا Music Player Daemon یک نرم‌افزار متن‌باز و سرویس‌محور پخش موسیقیه که فایل‌های صوتی رو پخش می‌کنه، پلی‌لیست‌ها رو مدیریت می‌کنه و یک دیتابیس موسیقی نگه می‌داره.  کاربری‌اش مثل یه سروره که کلاینت‌ها از راه دور کنترلش می‌کنن، نتیجتا از روی اینترنت و شبکه هم قابل مدیریته. کاری که MPD انجام میده استریم روی شبکه نیست تا هر کارخواه صدا رو بگیره و پخش کنه، در واقع با دسترسی به پایگاه‌داده موزیکش روی همون سامانه‌ای که نصب شده صدارو پخش می‌کنه. اما قابلیت استریم همون صدا رو روی شبکه با httpd داره و میشه رادیو برخط(online) هم ساخت باهاش.

- [وبگاه](https://www.musicpd.org/)
- [آرچ‌ویکی](https://wiki.archlinux.org/title/Music_Player_Daemon)

## نحوه پیکربندی
نیازمند پیکربندی پیچیده‌ای نیست.
- بسته mpd از مخزن توزیعتون نصب کنید.
- این پرونده ``~/.config/mpd/mpd.conf`` رو بسازید و پیکربندی زیر رو داخلش قرار بدید:
```
# See: /usr/share/doc/mpd/mpdconf.example

pid_file "/run/mpd/mpd.pid"
db_file "/var/lib/mpd/mpd.db"
state_file "/var/lib/mpd/mpdstate"
playlist_directory "/var/lib/mpd/playlists"

music_directory "~/Music"

audio_output {
  type    "pipewire"
  name    "Pipewire Sound Server"
}
```
- با زدن دستور `systemctl enable --user --now mpd` و سپس `systemctl start --user --now mpd` می‌تونی سرویسش رو فعال کنید تا همیشه روشن باقی بمونه. مهم اینه با پرچم کاربر اجرا بشه تا mpd بتونه دسترسی داشته باشه به خروجی تعریف شده وگرنه با خطا مواجه میشه.

## کارخواه (client)
بهترین کارخواه میزکاری که الان در لینوکس وجود داره برای mpd ایوفونیکا هست. که می‌تونید از [فلت‌هاب](https://flathub.org/apps/io.github.htkhiem.Euphonica) نصب کنید. و نسخه اندروید MALP‌ هست که ظاهر مدرنی نداره. سیاهه کامل کارخواه های MPD رو از این [پیوند](https://www.musicpd.org/clients/) می‌تونید بخونید. توجه کنید که کارخواه mpd مسئول پخش صدا نیست بلکه فقط مدیریت کننده اونه.

مرسوم ترین کارخواه تحت خط فرمان هم mpc هست.


![[Pasted image ۲۰۲۵۰۹۱۵۱۸۲۷۰۶.png]]

## رادیو برخط
رادیو برخط (Online Radio) نوعی پخش زنده صوت یا موسیقی است که از طریق اینترنت انجام می‌شود. این سرویس مشابه ایستگاه‌های رادیویی سنتی هست، اما به جای امواج رادیویی، محتوا از طریق شبکه‌های اینترنتی به شنوندگان منتقل می‌شود. برای mpd هم میشه خروجی httpd تعریف کرد تا روی شبکه استریم کنه موسیقی رو ([راهنما](https://mpd.readthedocs.io/en/latest/plugins.html#httpd)) و اینطوری با استفاده از برنامه های دیگه مثل [shortwave](https://apps.gnome.org/fa/Shortwave/) بشه گوش کرد. و به راحتی می‌تونید با تنظیم خروجی httpd روی شبکه استریم کنید و با کارخواه های رادیوبرخط بشنوید. یا روی سرور با دوستان بخرید MPD تنظیم کنید روی httpd و همتون همزمان توی خونه بشنوید.



![[Pasted image ۲۰۲۵۰۹۱۵۱۸۲۲۵۲.png]]

پیکربندی مورد نیاز برای استریم روی شبکه:
```
audio_output {
       type            "httpd"
       name            "My HTTP Stream"
       encoder         "vorbis"                # optional, vorbis or lame
       port            "8000"
       bind_to_address "0.0.0.0"               # optional, IPv4 or IPv6
      quality         "5.0"                   # do not define if bitrate is defined
       format          "44100:16:1"
       max_clients     "0"                     # optional 0=no limit
}

```