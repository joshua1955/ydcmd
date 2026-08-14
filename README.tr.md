# ydcmd

REST API uzerinden Yandex.Disk bulut depolama ile calismak icin konsol istemcisi.

> **Bu proje, [abbat](https://github.com/abbat/ydcmd) projesinin fork'udur**

## ⚠️ Bu surumdeki onemli degisiklikler

**Guncel Yandex.Disk API'siyle (2024) calisacak sekilde guncellendi**

### 🔧 Ana duzeltmeler:
- **Temel API URL'si guncellendi**: `cloud-api.yandex.net` → `cloud-api.yandex.com`
- **Python 3 uyumlulugu duzeltildi**: unicode ve JSON sorunlari cozuldu
- **OAuth token islemesi iyilestirildi**: token dogrulama eklendi
- **SSL calismasi duzeltildi**: sertifika dogrulamasi olmadan calisma destegi eklendi
- **Hata islemesi iyilestirildi**: daha bilgilendirici hata mesajlari

### 🚀 Sonuc:
- ✅ Guncel Yandex.Disk API'siyle tam uyumluluk
- ✅ Python 3.x ile calisir
- ✅ Dogru OAuth token islemesi
- ✅ Windows/Linux/macOS uzerinde kararli calisma

## Kurulum

Kaynak koddan:

```bash
git clone https://github.com/your-username/ydcmd.git
cd ydcmd
sudo cp ydcmd.py /usr/local/bin/ydcmd
```

## Nasil yardim edebilirsiniz

* Bu belgeyi ana dilinize cevirin
* Dokumantasyondaki hatalari duzeltin
* Bilgiyi arkadaslarinizla paylasin
* Gelistiriciyseniz PR gonderin

## Yapilandirma

Istemciyle calismak icin OAuth token almaniz gerekir. **Basit yontem** - yerlesik komutu kullanin:

```bash
python ydcmd.py token
```

Komut guzel bir yonerge ve yetkilendirme baglantisi verir. Sadece:

1. Baglantiyi tarayicinizda acin
2. Uygulamaya erisim izni verin
3. Sayfadaki yetkilendirme kodunu kopyalayin
4. Terminale yapistirin ve Enter'a basin

Betik otomatik olarak:
- ✅ OAuth token alir
- ✅ `~/.ydcmd.cfg` yapilandirma dosyasini olusturur veya gunceller
- ✅ Tokeni guvenli bicimde kaydeder

Bundan sonra tum ydcmd komutlarini hemen kullanabilirsiniz!

### Alternatif yontem (kendi uygulamanizi kaydetme):

1. Yandex'te bir uygulama kaydedin
2. Izinleri ayarlayin: `Yandex.Disk REST API`
3. `Development client` ozelligini etkinlestirin
4. Yetkilendirme icin alinan `client_id` degerini kullanin

Daha fazla bilgi: [tokeni elle alma](https://yandex.ru/dev/id/doc/en/tokens/debug-token).

## Kullanim

Kisa yardimi konsolda almak icin betigi parametresiz veya `help` komutuyla calistirabilirsiniz. Genel cagri bicimi:

```
ydcmd [command] [options] [arguments]
```

**Komutlar**:

* `help` - komutlar ve uygulama secenekleri hakkinda kisa yardim al;
* `ls` - dosya ve dizin listesini al;
* `rm` - dosya veya dizin sil;
* `cp` - dosya veya dizin kopyala;
* `mv` - dosya veya dizin tasi;
* `put` - dosya veya dizini depolamaya yukle;
* `get` - dosya veya dizini depolamadan al;
* `cat` - depolamadaki dosyayi stdout'a yazdir;
* `mkdir` - dizin olustur;
* `stat` - nesne hakkinda meta veri al;
* `info` - depolama hakkinda meta veri al;
* `last` - son yuklenen dosyalar hakkinda meta veri al;
* `share` - nesneyi yayimla (dogrudan baglanti al);
* `revoke` - daha once yayimlanan nesneye erisimi kapat;
* `du` - depolamada dosyalarin kapladigi alani tahmin et;
* `clean` - dosya ve dizinleri temizle;
* `restore` - dosya veya dizini cop kutusundan geri yukle;
* `download` - internetten depolamaya dosya indir;
* `token` - uygulamanin calismasi icin oauth token al;
* `batch` - toplu islemler (yukleme, indirme, silme, tasima, kopyalama);
* `stats` - depolama istatistikleri ve izleme;
* `find` - ada/desene gore dosya ara;
* `sync` - gelismis esitleme islemleri.

**Secenekler**:

* `--config=<S>` - yapilandirma dosyasi adi (varsayilan dosyadan farkliysa);
* `--timeout=<N>` - ag baglantisi kurma zaman asimi, saniye;
* `--retries=<N>` - hata kodu dondurmeden once api metodunu cagirma denemesi sayisi;
* `--delay=<N>` - api metodunu cagirma denemeleri arasindaki zaman asimi, saniye;
* `--limit=<N>` - dosya ve dizin listesi almak icin tek cagrida dondurulen oge sayisi;
* `--token=<S>` - oauth token (guvenlik nedeniyle yapilandirma dosyasinda veya `YDCMD_TOKEN` ortam degiskeniyle belirtmeniz onerilir);
* `--quiet` - hata cikisini bastir, islemin basarisi donus koduyla belirlenir;
* `--verbose` - genisletilmis bilgi yazdir;
* `--debug` - hata ayiklama bilgisi yazdir;
* `--chunk=<N>` - giris/cikis islemleri icin KB cinsinden veri blogu boyutu;
* `--ca-file=<S>` - guvenilir sertifika merkezlerinin sertifikalarini iceren dosya adi (bos ise sertifika gecerlilik denetimi yapilmaz);
* `--ciphers=<S>` - sifreleme algoritmalari kumesi;
* `--version` - surumu yazdir ve cik.

### Dosya ve dizin listesini alma

```
ydcmd ls [options] [disk:/object]
```

**Secenekler**:

* `--human` - dosya boyutunu insan tarafindan okunabilir bicimde yazdir;
* `--short` - dosya ve dizin listesini ek bilgi olmadan yazdir (satir basina bir ad);
* `--long` - genisletilmis liste yazdir (olusturma zamani, degistirme zamani, boyut, dosya adi).

Hedef nesne belirtilmezse depolamanin kok dizini kullanilir.

### Dosya veya dizin silme

```
ydcmd rm <disk:/object1> [disk:/object2] ...
```

**Secenekler**:

* `--trash` - cop kutusuna sil;
* `--poll=<N>` - asenkron islem yapilirken durum sorgulamalari arasindaki sure, saniye;
* `--async` - komutu islemin tamamlanmasini (`poll`) beklemeden calistir.

Dosyalar kurtarma imkani olmadan silinir. Dizinler yinelemeli olarak silinir (ic ice dosya ve dizinler dahil).

### Dosya veya dizin kopyalama

```
ydcmd cp <disk:/object1> <disk:/object2>
```

**Secenekler**:

* `--poll=<N>` - asenkron islemler yapilirken durum sorgulamalari arasindaki sure, saniye;
* `--async` - komutu islemin tamamlanmasini (`poll`) beklemeden calistir.

Ad cakismasi durumunda dizinler ve dosyalarin uzerine yazilir. Dizinler yinelemeli olarak kopyalanir (ic ice dosya ve dizinler dahil).

### Dosya veya dizin tasima

```
ydcmd mv <disk:/object1> <disk:/object2>
```

**Secenekler**:

* `--poll=<N>` - asenkron islemler yapilirken durum sorgulamalari arasindaki sure, saniye;
* `--async` - komutu islemin tamamlanmasini (`poll`) beklemeden calistir.

Ad cakismasi durumunda dizinler ve dosyalarin uzerine yazilir.

### Dosyayi depolamaya yukleme

```
ydcmd put <file> [disk:/object]
```

**Secenekler**:

* `--rsync` - depolamadaki dosya ve dizin agacini yerel agacla esitle;
* `--no-recursion` - ic ice dizinlerin icerigini yukleme;
* `--no-recursion-tag=<S>` - belirtilen dosyayi iceren dizinlerde ic ice dizinlerin icerigini yukleme;
* `--exclude-tag=<S>` - belirtilen dosyayi iceren dizinleri yukleme sirasinda atla;
* `--skip-hash` - md5/sha256 butunluk denetimlerini atla;
* `--threads=<N>` - isci surec sayisi;
* `--iconv=<S>` - gerekirse dosya ve dizin adlarini belirtilen kodlamadan geri yuklemeyi dene (orn. `--iconv=cp1251`);
* `--progress` - islem ilerlemesini yazdir (varsayilan olarak acik, python-progressbar modulunun kurulmasi onerilir).

Hedef nesne belirtilmezse dosya yukleme icin depolamanin kok dizini kullanilir. Hedef nesne dizini gosteriyorsa (`/` ile bitiyorsa), kaynak dosya adi dizin adina eklenir. Hedef nesne varsa onay istenmeden uzerine yazilir. Sembolik baglantilar yok sayilir.

### Dosyayi depolamadan alma

```
ydcmd get <disk:/object> [file]
```

**Secenekler**:

* `--rsync` - yerel dosya ve dizin agacini depolamadaki agacla esitle;
* `--no-recursion` - ic ice dizinlerin icerigini indirme;
* `--skip-hash` - md5/sha256 butunluk denetimlerini atla;
* `--threads=<N>` - isci surec sayisi;
* `--progress` - islem ilerlemesini yazdir (varsayilan olarak acik, python-progressbar modulunun kurulmasi onerilir).

Hedef dosya adi belirtilmezse depolamadaki dosya adi kullanilir. Hedef nesne varsa onay istenmeden uzerine yazilir.

### Depolamadaki dosyayi stdout'a yazdirma

```
ydcmd cat <disk:/object1> [disk:/object2] ...
```

### Dizin olusturma

```
ydcmd mkdir <disk:/path1> [disk:/path2] ...
```

### Nesne hakkinda meta veri alma

```
ydcmd stat [disk:/object]
```

Hedef nesne belirtilmezse depolamanin kok dizini kullanilir.

### Depolama hakkinda meta veri alma

```
ydcmd info
```

**Secenekler**:

* `--long` - boyutlari insan tarafindan okunabilir bicim yerine bayt olarak goster.

### Son yuklenen dosyalar hakkinda meta veri alma

```
ydcmd last [N]
```

**Secenekler**:

* `--human` - dosya boyutunu insan tarafindan okunabilir bicimde yazdir;
* `--short` - dosya listesini ek bilgi olmadan yazdir (satir basina bir ad);
* `--long` - genisletilmis liste yazdir (olusturma zamani, degistirme zamani, boyut, dosya).

N parametresi ayarlanmazsa REST API'den gelen varsayilan deger kullanilir.

### Nesne yayimlama

```
ydcmd share <disk:/object1> [disk:/object2] ...
```

Komut depolamadaki nesne adini ve ona ait baglantiyi dondurur.

### Erisimi kapatma

```
ydcmd revoke <disk:/object1> [disk:/object2] ...
```

### Kaplanan alani tahmin etme

```
ydcmd du [disk:/object]
```

**Secenekler**:

* `--depth=<N>` - N seviyesine kadar dizin boyutlarini goster;
* `--long` - boyutlari insan tarafindan okunabilir bicim yerine bayt olarak goster.

Hedef nesne belirtilmezse depolamanin kok dizini kullanilir.

### Dosya ve dizinleri temizleme

```
ydcmd clean <options> [disk:/object]
```

**Secenekler**:

* `--dry` - silme islemi yapma, silinecek nesnelerin listesini yazdir;
* `--type=<S>` - silinecek nesne turu (`file` - dosyalar, `dir` - dizinler, `all` - tumu);
* `--keep=<S>` - korunmasi gereken nesneleri secme olcutu:
  * Verilerin silinmesi gereken tarihten **onceki** tarihi secmek icin ISO biciminde tarih dizesi kullanabilirsiniz (orn. `2014-02-12T12:19:05+04:00`);
  * Goreli zamani secmek icin sayi ve olcu kullanabilirsiniz (orn. `7d`, `4w`, `1m`, `1y`);
  * Kopya sayisini secmek icin olcusuz sayi kullanabilirsiniz (orn. `31`).

Hedef nesne belirtilmezse depolamanin kok dizini kullanilir. Nesnelerin siralanmasi ve filtrelenmesi degistirme tarihine gore yapilir (olusturma tarihine gore degil).

### Dosya veya dizini cop kutusundan geri yukleme

```
ydcmd restore <trash:/object> [name]
```

**Secenekler**:

* `--poll=<N>` - asenkron islemler yapilirken durum sorgulamalari arasindaki sure, saniye;
* `--async` - komutu islemin tamamlanmasini (`poll`) beklemeden calistir.

Ad cakismasi durumunda dizinler ve dosyalarin uzerine yazilir. Dizinler yinelemeli olarak geri yuklenir (ic ice dosya ve dizinler dahil).

### Internetten depolamaya dosya indirme

```
ydcmd download <URL> [disk:/object]
```

**Secenekler**:

* `--poll=<N>` - asenkron islemler yapilirken durum sorgulamalari arasindaki sure, saniye;
* `--async` - komutu islemin tamamlanmasini (`poll`) beklemeden calistir;
* `--no-redirects` - indirme sirasinda yonlendirmeleri yasakla.

Hedef nesne belirtilmezse depolamanin kok dizini kullanilir ve dosya adi URL icerigine gore secilir (mumkunse).

### OAuth token alma

```
ydcmd token [code]
```

Arguman belirtilmeden calistirilirse komut, kod almak icin baglanti yazdirir. Baglantiyi tarayicida acin, uygulamaya erisim izni verin ve alinan kodu OAuth token almak icin arguman olarak kullanin.

### Toplu islemler

```
ydcmd batch <operation> [options] [args]
```

**Islemler**:

* `upload <local_dir> [remote_dir]` - tum dizini yukle;
* `download <remote_dir> [local_dir]` - tum dizini indir;
* `delete <file1> [file2] ...` - birden cok dosyayi sil;
* `move <src1> <src2> ... <dest>` - birden cok dosyayi hedefe tasi;
* `copy <src1> <src2> ... <dest>` - birden cok dosyayi hedefe kopyala.

### Istatistikler ve izleme

```
ydcmd stats [--detailed]
```

Depolama kullanim istatistiklerini gosterir:
- Toplam alan ve kullanilan alan
- Gorsel ilerleme cubuguyla kullanim yuzdesi
- Ayrintili istatistikler (`--detailed` secenegiyle)
- Optimizasyon ipuclari

### Dosya arama

```
ydcmd find <pattern> [directory]
```

**Ornekler**:
- `ydcmd find *.txt` - tum .txt dosyalarini bul
- `ydcmd find "*.mp4" /Videos` - Videos klasorunde .mp4 dosyalarini bul
- `ydcmd find "document*"` - "document" ile baslayan dosyalari bul

### Gelismis esitleme

```
ydcmd sync <operation> [options]
```

**Islemler**:

* `init <local_dir> <remote_dir>` - esitlemeyi baslat ve `<local_dir>/.ydcmd-sync.cfg` olustur;
* `status [local_dir]` - esitleme durumunu goster;
* `pull [local_dir] [remote_dir]` - uzaktan yerele esitle;
* `push [local_dir] [remote_dir]` - yerelden uzaga esitle;
* `diff [local_dir] [remote_dir]` - dizinler arasindaki farklari goster.

Local dizin belirtilmezse guncel dizin (`.`) kullanilir.

`push` ve `pull` degismeyen dosyalari atlar: once boyut, sonra md5/sha256 karsilastirir. `skip-hash = yes` aciksa boyut eslesmesinden sonra hash kontrolu yapilmaz.
`push` bos local dosyalari da atlar ve `Skipped empty` yazar.

Tek komut icin hash olmadan hizli modu acin:

```bash
ydcmd sync push --skip-hash
ydcmd sync pull --skip-hash
```

`sync init` sonrasi `<local_dir>/.ydcmd-sync.cfg` dosyasini duzenleyin:

```ini
[sync]
remote-dir = /Backup
pattern =
include = *.jpg, docs/*.pdf
exclude = *.tmp, cache/*, .git/*
tag-filter = .ydcmd-ignore
```

Filtre degerleri virgul veya boslukla ayrilir ve shell-style desenler kullanir. `pattern`, include desenleri icin alias'tir. `include`, esitlemeyi eslesen yol veya ada sahip dosyalarla sinirlar. `exclude`, eslesen dosya ve dizinleri atlar. `tag-filter`, icinde bu ada sahip dosya bulunan tum dizini atlar. Sync config dosyasinin kendisi yuklenmez veya indirilmez.

## Yapilandirma

Kolaylik icin `~/.ydcmd.cfg` adli yapilandirma dosyasi olusturmaniz ve izinlerini `0600` veya `0400` olarak ayarlamaniz onerilir. Dosya bicimi:

```ini
[ydcmd]
# comment
<option> = <value>
```

Ornek:

```ini
[ydcmd]
token   = your_oauth_token
verbose = yes
ca-file = /etc/ssl/certs/ca-certificates.crt
```

Tam yapilandirma ornegi:

```ini
[ydcmd]
timeout = 30
poll = 1
retries = 3
delay = 30
limit = 100
chunk = 512
token = your_oauth_token
quiet = no
verbose = no
debug = no
async = no
rsync = no
no-recursion = no
no-redirects = no
skip-md5 = no
skip-hash = no
threads = 0
progress = yes
base-url = https://cloud-api.yandex.com/v1/disk
app-id = 2415aa2e6ceb4839b1202e15ac83536c
app-secret = b8ae32ce025c451f84bd7df17029cb55
ca-file = /etc/ssl/certs/ca-certificates.crt
ciphers = kEECDH+AES128:kEECDH+AES256:kRSA+AES128:kRSA+AES256:AES128-SHA:AES256-SHA:!aNULL:!MD5
depth = 1
dry = no
type = all
trash = no
```

`token` degerini commit etmeyin veya yayimlamayin.

Parametreler:

| Parametre | Anlam |
| --- | --- |
| `timeout` | Ag baglantisi ve API istegi zaman asimi, saniye. |
| `poll` | Asenkron islem durum kontrolleri arasindaki bekleme, saniye. |
| `retries` | Gecici hatadan sonra API istegi tekrar sayisi. |
| `delay` | API istegi tekrar denemeleri arasindaki bekleme, saniye. |
| `limit` | Tek dosya-liste API cagrisinda istenen nesne sayisi. |
| `chunk` | Dosya islemleri icin okuma/yazma blok boyutu, KB. |
| `token` | Yandex OAuth token. Zorunlu ve gizli. |
| `quiet` | `yes` hata cikisini bastirir. |
| `verbose` | `yes` ayrintili islem cikisini acar. |
| `debug` | `yes` hata ayiklama cikisini acar. |
| `async` | `yes` desteklenen asenkron islemlerin bitmesini beklemez. |
| `rsync` | `yes` `put/get --rsync` sirasinda fazla dosyalari siler. |
| `no-recursion` | `yes` ic ice dizinlerin yinelemeli islenmesini kapatir. |
| `no-redirects` | `yes` `download` icin HTTP redirect kullanmaz. |
| `skip-md5` | Eski parametre, `skip-hash` ile ayni. |
| `skip-hash` | `yes` md5/sha256 butunluk denetimlerini kapatir. |
| `threads` | Dizin yukleme/indirme icin worker surec sayisi. `0` havuz yok demektir. |
| `progress` | `yes` `python-progressbar` varsa progress bar gosterir. |
| `base-url` | Yandex.Disk REST API temel URL'si. |
| `app-id` | Yerlesik uygulamanin OAuth client id degeri. |
| `app-secret` | Yerlesik uygulamanin OAuth client secret degeri. |
| `ca-file` | HTTPS icin guvenilir CA sertifika dosyasi. Bos ise sertifika dogrulamasi yok. |
| `ciphers` | HTTPS icin TLS cipher suites. |
| `depth` | `du` icin dizin boyutu cikti derinligi. |
| `dry` | `clean` icin `yes`: silmeden nelerin silinecegini gosterir. |
| `type` | `clean` icin nesne turu: `file`, `dir` veya `all`. |
| `trash` | `rm` icin `yes`: kalici silme yerine cope tasir. |

### ⚠️ Onemli ayarlar:

- **`token`** - OAuth token (calisma icin gerekli)
- **`ca-file`** - SSL sertifikalari dosyasinin yolu (istege bagli)
  - Belirtilmezse uygulama SSL sertifika dogrulamasi olmadan calisir
  - Windows'ta bos birakmak gerekebilir: `ca-file = `

## Ortam degiskenleri

* `YDCMD_TOKEN` - oauth token, `--token` secenegine gore onceliklidir;
* `SSL_CERT_FILE` - guvenilir sertifika merkezlerinin sertifikalarini iceren dosya adi, `ca-file` secenegine gore onceliklidir.

## Cikis kodlari

Otomatik modda calisirken (cron uzerinden), komut calistirma sonucunu almak yararli olabilir:

* `0` - basarili tamamlama;
* `1` - genel uygulama hatasi;
* `4` - HTTP-4xx durum kodu (istemci hatasi);
* `5` - HTTP-5xx durum kodu (sunucu hatasi).
