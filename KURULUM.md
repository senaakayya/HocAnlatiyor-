# HocAnlatiyor | Kurulum rehberi

Bu proje bir başlangıç sürümüdür. Sitede gerçek veli ve öğrenci hesapları, kişiye özel veri görünürlüğü, ders/ödev/gelişim/ödeme kayıtları vardır. **Ödeme alma sistemi değildir**; sadece ödeme durumunu gösterir. İlk kurulum için teknik adımlar gereklidir.

## 1. Supabase (ücretsiz plan)
1. https://supabase.com adresinde hesap oluşturup yeni bir proje açın.
2. SQL Editor içinde `schema.sql` dosyasının tüm içeriğini çalıştırın. Yeni ve boş bir proje kullanın.
3. Authentication > Providers > Email açık olsun. Gerçek kullanıma geçmeden önce e-posta doğrulamasını açık tutun.
4. Authentication > Users üzerinden **kendi e-posta adresinizle** öğretmen kullanıcısı oluşturun. E-posta doğrulama ve parola ayarlarını tamamlayın.
5. SQL Editor'de aşağıdaki sorguda e-posta ve ad kısmını kendinize göre değiştirip çalıştırın:

```sql
insert into public.profiles(id,full_name,role)
select id,'Sena Hoca','teacher' from auth.users
where email='SIZIN_EPOSTANIZ@example.com'
on conflict(id) do nothing;
```

**Önemli:** Bu sorguyu yalnızca kendi öğretmen hesabınız için çalıştırın. Başka kullanıcıya öğretmen yetkisi vermeyin.

## 2. Siteyi yayınlama
1. https://pages.github.com üzerinden ücretsiz GitHub Pages kurabilirsiniz. Yeni bir repo oluşturun; `index.html` dosyasını kök dizine yükleyin. Pages ayarlarından ana dalı yayınlayın.
2. Yayınlanan siteyi açın. Supabase Dashboard > Project Settings / API'den proje URL'sini ve **anon / publishable** anahtarını girin. **service_role / secret anahtarı asla kullanılmamalı.**
3. Supabase Authentication > URL Configuration alanında yayınlanan site adresini Site URL ve Redirect URLs kısmına ekleyin.
4. Öğretmen hesabıyla giriş yapın. Öğrenciler sekmesinden öğrenci oluşturun; Davetler sekmesinden veli veya öğrenci için ilgili e-postayla davet kodu oluşturun.
5. Davet kodunu yalnızca ilgili veliye/öğrenciye özel olarak iletin. Davetli kendi e-postasıyla hesap açıp kodu etkinleştirir.

## Güvenlik ve kullanım notları
- Bu proje henüz canlı ortama kurulmadı veya uçtan uca test edilmedi. Gerçek çocuk bilgileri ve ödeme kayıtları girmeden önce güvenlik, erişim ve veri koruma testleri yapılmalıdır.
- Öğrenci/veli bilgileri kişisel veridir; gerekli aydınlatma ve izin süreçlerini yürütün. Ödeme ekranına kart numarası veya banka şifresi yazmayın.
- Davet kodları tek kullanımlıktır ve e-postayla eşleşir. E-posta doğrulamasını kapatmayın.
- Öğretmen hesabı için güçlü, benzersiz şifre ve mümkünse MFA kullanın.
- Supabase ve GitHub Pages ücretsiz planlarının sınırları ve koşulları değişebilir. Özel alan adı zorunlu değildir.
- Site `esm.sh` üzerinden JavaScript kütüphanesi yükler; internet bağlantısı gereklidir.
- Bu ilk sürümde PDF yükleme, bildirim gönderme, ödeme tahsilatı ve gelişmiş raporlama yoktur.
