# Ta-lim-platformasi# Milliy-ed (EduAdapt)

**AI yordamida moslashuvchan ta'lim platformasi — o'qituvchi, o'quvchi, ota-ona va maktab rahbariyati uchun yagona muhit.**

---

## Muammo

Maktablarda bitta o'qituvchi 30+ o'quvchiga bir xil materialni beradi. Kim ortda
qolayotganini o'z vaqtida aniqlash qiyin, ota-ona esa farzandining holatini
ko'pincha chorak oxirida biladi.

## Yechim

EduAdapt har bir o'quvchining darajasini real vaqtda kuzatadi va savollarni
shunga moslaydi. AI o'qituvchiga yordam beradi, lekin **hech qachon uning o'rnini
bosmaydi** — yakuniy bahoni doim o'qituvchi qo'yadi.

## Asosiy imkoniyatlar

| Modul | Nima qiladi |
|---|---|
| **Adaptiv test** | To'g'ri javobdan keyin savol qiyinlashadi, xatodan keyin osonlashadi |
| **AI kontent yaratish** | O'qituvchi mavzu nomini yozadi — AI mashq taklif qiladi |
| **Human-in-the-loop baholash** | AI baho taklif qiladi, o'qituvchi tasdiqlaydi yoki tahrirlaydi |
| **Erta ogohlantirish** | Darajasi 45% dan tushgan o'quvchi avtomatik belgilanadi |
| **Davomat va ota-ona aloqasi** | Kundalik davomat, ota-ona bilan to'g'ridan-to'g'ri yozishma |
| **Uyda ta'lim** | Maktabga borolmaydigan o'quvchi uchun individual reja va moslashtirishlar |
| **Jalb qilish markazi** | Chekka hududlar uchun mobil laboratoriya va murabbiy tizimi |
| **Malaka oshirish** | O'qituvchilar uchun xalqaro kurslar va nishonlar |
| **Admin paneli** | Maktab/tuman bo'yicha jamlangan ko'rsatkichlar |

## Xavfsizlik

Bu bolalar ma'lumoti bilan ishlaydigan tizim, shuning uchun xavfsizlik alohida ishlangan:

- **Har bir o'quvchi — o'z hisobi.** O'qituvchi bir martalik kod beradi, o'quvchi
  o'ziga parol yaratadi. Boshqa o'quvchining nomidan kirish mumkin emas.
- **Barcha parollar bcrypt bilan hashlangan**, hech qachon brauzerga yuborilmaydi.
- **Test javoblari serverda qoladi** — brauzerga faqat savol va variantlar boradi,
  javobni server tekshiradi.
- **Bilim darajasi serverda hisoblanadi** — mijoz o'ziga baho qo'ya olmaydi.
- **Ota-ona faqat o'z farzandini ko'radi** (har oilaga alohida kod).
- **AI kaliti faqat serverda**, foydalanuvchi boshiga so'rov limiti bilan.
- **Avtomatik zaxira nusxa** har 24 soatda, oxirgi 14 kun saqlanadi.

## Texnologiyalar

**Frontend:** React 18, Vite, Tailwind CSS, Recharts
**Backend:** Node.js, Express, SQLite, JWT, bcrypt
**AI:** Anthropic Claude API (server orqali proxy)

## Loyiha tuzilishi

```
src/                 React frontend
  lib/api.js         backend bilan aloqa
  views/             rollar bo'yicha ekranlar
server/              Express backend
  src/db.js          SQLite sxemasi
  src/auth.js        JWT va rol tekshiruvi
  src/routes/        API endpointlari
  src/backup.js      avtomatik zaxira nusxa
```

## Ishga tushirish

```bash
# Backend
cd server
cp .env.example .env      # JWT_SECRET va ADMIN_PASSWORD ni to'ldiring
npm install
npm run dev               # http://localhost:8787

# Frontend (yangi terminalda)
cp .env.example .env
npm install
npm run dev               # http://localhost:5173
```

`JWT_SECRET` uchun: `openssl rand -hex 32`

AI funksiyalari uchun `server/.env` ga `ANTHROPIC_API_KEY` qo'shing.
Kalitsiz ham platformaning qolgan hamma qismi ishlaydi.

## Kim qanday kiradi

| Rol | Nima kerak |
|---|---|
| O'qituvchi | Sinf kodi + sinf paroli |
| O'quvchi (birinchi marta) | Sinf kodi + ism + bir martalik kod, so'ng o'z parolini yaratadi |
| O'quvchi (keyin) | Sinf kodi + ism + o'z paroli |
| Ota-ona | Sinf kodi + farzand ismi + ota-ona kodi |
| Admin | Ism + admin paroli |

Bir martalik kod ishlatilgach o'chadi. O'quvchi parolini unutsa, o'qituvchi
"Parolni tiklash" tugmasi bilan yangi kod chiqaradi.

## Zaxira nusxa va eksport

Server ishga tushganda va har 24 soatda `server/data/backups/` ga nusxa oladi,
oxirgi 14 tasini saqlaydi. O'qituvchi uchta CSV yuklab oladi: ro'yxat va kodlar,
baholar, davomat. Fayllar Excel'da to'g'ri ochiladi.

## Holati

Ishlaydigan MVP. Backend to'liq sinovdan o'tkazilgan: rol chegaralari, sinflar
izolyatsiyasi, parol xavfsizligi va ma'lumot sizib chiqishi tekshirilgan.
