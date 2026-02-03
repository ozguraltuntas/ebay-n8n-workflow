# eBay Bulk Upload Workflow (n8n)

Google Drive'dan ürün bilgilerini okuyup eBay'e yüklenebilir CSV oluşturan n8n workflow'u.

## Özellikler
- Google Drive'dan content.docx parse etme (mammoth)
- Görselleri otomatik toplama (SKU + parent klasör)
- Google Sheets'ten fiyat ve kategori bilgisi alma
- OpenAI ile eBay title oluşturma (80 karakter, araç bilgisi)
- eBay CSV formatında export
- Daha önce işlenmiş klasörleri atlama (_exported.txt)

## Gereksinimler
- n8n (Docker ile) - v1.70.3+
- Google Drive API credential
- Google Sheets API credential
- OpenAI API key
- Mammoth paketi (docx parse için)

## Docker Kurulum
```bash
# Dockerfile
FROM n8nio/n8n:1.70.3
USER root
RUN cd /usr/local/lib/node_modules/n8n && npm install mammoth
USER node
```
```bash
# docker-compose.yml
services:
  n8n:
    build: .
    ports:
      - "5678:5678"
    environment:
      - NODE_FUNCTION_ALLOW_EXTERNAL=mammoth
    volumes:
      - n8n_data:/home/node/.n8n
volumes:
  n8n_data:
```

## Kullanım
1. JSON dosyasını n8n'e import et
2. Credential'ları ayarla (Google Drive, Google Sheets, OpenAI)
3. Form'dan marka adı gir
4. CSV otomatik olarak Google Drive'a yüklenir

## Google Drive Klasör Yapısı
```
MAXTEL/
├── wiper blade/
│   ├── 12602221/
│   │   └── 12602221/
│   │       ├── content.docx
│   │       └── 3.jpg
│   │   ├── 1.jpg, 2.jpg, std1.jpg...
│   └── price (Google Sheet)
│   └── ebay category id (Google Sheet)
```