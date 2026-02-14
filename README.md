# InfazMachine MVP

## Solution Design
- **Veri modeli:** `CaseFile`, `Sentence`, `CalculationRun` (Prisma). `CalculationRun` seçilen politika ve girdi/çıktı JSON'unu saklar.
- **Engine yaklaşımı:** `src/engine/*` altında saf fonksiyonlar. UI'dan bağımsız şekilde `calculateInfaz(input)` çağrılır.
- **Politika seçimi:**
  - `strictest_ratio_applies` (varsayılan): en ağır oran toplam süreli cezaya uygulanır.
  - `per_sentence_then_sum`: ilâm bazında hesaplanır, sonra toplanır.
- **UI:** ana sayfada JSON tabanlı hızlı giriş + sonuç; `/report` sayfasında yazdırılabilir rapor kabuğu.

## Kurulum
```bash
npm install
cp .env.example .env
npx prisma generate
npx prisma migrate dev --name init
npm run dev
```

## Test
```bash
npm test
```

## Bilinen sınırlamalar
- 5275 `107-108.docx` kaynak metni ortamda olmadığı için tam suç listeleri ve kombinasyon matrisleri **NEEDS_LEGAL_SOURCE** olarak işaretlendi.
- Takvim bazlı süre dönüşümü şimdilik `fixed_365_30` ile aynı davranır.
- Sunucu tarafı PDF üretimi yok; tarayıcı print kullanılmalı.

## Not implemented
- 105/A denetimli serbestlik: `Not implemented (needs legal source)`.
- m.107/3 ve m.107/4 tüm kombinasyon dalları için tam ayrıştırma: `NEEDS_LEGAL_SOURCE`.
- m.108/2 eklenecek miktar sınırı: sadece açıklama üretimi var, tam formül için kaynak metin bekleniyor.


## Troubleshooting
- `npm install` sırasında `403 Forbidden` alırsanız CI/ağ politikanız npm registry erişimini engelliyor olabilir.
- Proje `.npmrc` içinde varsayılan npm registry kullanır; kurumsal proxy gerekiyorsa ortamınıza uygun `HTTP_PROXY/HTTPS_PROXY` ayarlarını yapın.
- Prisma datasource MVP'de `sqlite` sabitlenmiştir. PostgreSQL'e geçiş için `prisma/schema.prisma` içindeki datasource provider'ı `postgresql` yapıp `DATABASE_URL`'i güncelleyin.
