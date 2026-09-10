# Uzun Hikaye — QR Yönlendirici

Masalardaki QR kodun gittiği **kalıcı adres**. Tek işi, ziyaretçiyi güncel menü
adresine yönlendirmek.

```
QR kod  →  yonlendirme.vercel.app  →  menü sitesi
           (bu proje — asla değişmez)   (istediğiniz zaman değişir)
```

Menü sitesi: [uzun-hikaye-qr](https://github.com/cagr1tekin/uzun-hikaye-qr)

## Neden ayrı bir proje?

Basılı QR kod değiştirilemez. Masalara yapıştırıldıktan sonra adresi
değiştirmek, tüm masaları tek tek dolaşmak demektir.

Yönlendirici menü sitesinin içinde olsaydı, menü `uzunhikaye.com`'a
taşındığında yönlendirici de onunla taşınırdı ve QR'lar ölürdü. Ayrı bir Vercel
projesi olduğu için menü nereye giderse gitsin bu adres sabit kalır.

**Bu projenin adresi bir kez belirlenir, bir daha asla değiştirilmez.** Vercel
proje adı `.vercel.app` alt alan adını belirlediği için proje adını da
değiştirmeyin.

## Hedefi değiştirme

Tek yapılacak `vercel.json` içindeki `destination` satırını güncellemek:

```json
{
  "redirects": [
    {
      "source": "/(.*)",
      "destination": "https://uzunhikaye.com/",
      "permanent": false
    }
  ]
}
```

Kaydedip push edin. Vercel ~20 saniyede yayına alır, QR'lar yeni adrese gitmeye
başlar. Masalardaki kodlara dokunulmaz.

### `permanent` neden `false` kalmalı

| Değer | HTTP | Sonuç |
|-------|------|-------|
| `false` | 307 | Tarayıcı her seferinde buraya sorar. Hedef değişince **herkes yeni adrese gider.** |
| `true` | 301 | Tarayıcı adresi *kalıcı olarak* önbelleğe alır. Hedefi sonradan değiştirseniz bile, daha önce QR'ı okutmuş telefonlar **eski adrese gitmeye devam eder.** |

`permanent: true` yapmak bu projenin varlık sebebini ortadan kaldırır. Elle
önbellek temizlemeden düzelmez. **`false` bırakın.**

## Yayına alma

1. Vercel → **Add New Project** → bu repoyu seçin
2. Project Name: `yonlendirme` (QR'a yazılacak adres bundan doğar — sonradan değiştirmeyin)
3. Framework Preset: **Other**, Build Command boş, Output Directory boş
4. Deploy

Sonra `https://yonlendirme.vercel.app` adresini tarayıcıda açıp menüye
düştüğünü doğrulayın. QR'ı ancak ondan sonra bastırın.

## Dosyalar

```
vercel.json    Yönlendirme kuralı — değiştirilecek tek dosya
index.html     Yapılandırma bozulursa görünen uyarı sayfası
```

`index.html` normal çalışmada hiç görünmez; yönlendirme sunucu tarafında,
sayfa yüklenmeden önce gerçekleşir. Hedef adresi bilerek bu dosyaya
yazmadık — tek kaynak `vercel.json` olsun ki ikisi birbirinden ayrı düşmesin.

## Sınır

Bu adres Vercel'e bağlı. Uzun vadede en güvenlisi, yönlendiriciyi de kendi
sahip olduğunuz bir alan adına taşımaktır (ör. `qr.uzunhikaye.com`) — o zaman
Vercel'den bağımsız olursunuz. Bunu yapacaksanız **QR'ı bastırmadan önce**
yapın.
