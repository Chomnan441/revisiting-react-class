# ESLint, Prettier, and ErrorLens management
_Exported on 6/24/2026 at 14:10:52 GMT+7 from Cursor (3.8.23)_

---

**User**

extention 
- eslint
- prettier 
- errorlens
พอดีกำลังจะ  commit  ไฟล์ แต่ไม่อยากให้ 3 extention ไปด้วยจัดการให้หน่อย แล้วถ้าอยากใช้ 3 extension นี้ตลอดเวลาทำงานทุกอย่างทุกไฟล์ และทุกครั้งที่ commit ไม่อยากให้ 3 extention ไปด้วยจัดการให้หน่อย

---

**Cursor**

กำลังตรวจสอบไฟล์ `.vscode` และ `.gitignore` เพื่อไม่ให้การตั้งค่า extension ถูก commit แต่ยังใช้งาน ESLint, Prettier และ Error Lens ได้ตลอดในเครื่อง


กำลังย้ายการตั้งค่าไปที่ User Settings ของ Cursor (ใช้ได้ทุกโปรเจกต์) และอัปเดต `.gitignore` ให้ไม่ commit โฟลเดอร์ `.vscode`


จัดการให้แล้ว สรุปดังนี้:

## สิ่งที่ทำ

### 1. ไม่ให้ extension config ถูก commit
แก้ `client/.gitignore` ให้ ignore ทั้งโฟลเดอร์ `.vscode/` (เดิมมี exception `!.vscode/extensions.json` ที่จะทำให้ไฟล์นั้นถูก commit ได้)

ตอนนี้ `extensions.json` และ `settings.json` ใน `.vscode/` **จะไม่ขึ้นใน git** อีกแล้ว — ไฟล์ยังอยู่ในเครื่องคุณ ใช้งานได้ตามปกติ แค่ไม่ถูก push ขึ้น repo

### 2. ย้ายการตั้งค่าไปที่ User Settings (ใช้ได้ทุกโปรเจกต์)
ย้ายการตั้งค่า ESLint, Prettier, Error Lens ไปที่ Cursor User Settings:

`%APPDATA%\Cursor\User\settings.json`

การตั้งค่านี้จะใช้ได้กับ **ทุกโปรเจกต์ ทุกไฟล์** โดยไม่ต้องพึ่ง `.vscode/settings.json` ในแต่ละ repo

## สิ่งที่คุณต้องทำครั้งเดียว

ติดตั้ง 3 extension ใน Cursor (ถ้ายังไม่ได้ติดตั้ง):

| Extension | ID |
|-----------|-----|
| ESLint | `dbaeumer.vscode-eslint` |
| Prettier | `esbenp.prettier-vscode` |
| Error Lens | `usernamehw.errorlens` |

เปิด Extensions (`Ctrl+Shift+X`) แล้วค้นหาชื่อ extension หรือ paste ID ได้เลย — ติดตั้งครั้งเดียว ใช้ได้ทุกโปรเจกต์

## ตอน commit

commit ได้ตามปกติ ไฟล์ที่ **จะไม่** ไปด้วย:
- `client/.vscode/extensions.json`
- `client/.vscode/settings.json`

ไฟล์ที่ **ควร** commit (เป็น config ของโปรเจกต์ ไม่ใช่ extension):
- `client/.prettierrc` — กฎการ format ของโปรเจกต์
- `client/package.json` — dependency อย่าง `eslint`, `prettier` สำหรับรันใน CI/terminal

สรุป: extension อยู่ในเครื่องคุณ + User Settings ใช้ทุกที่, ส่วน `.vscode/` ไม่ถูก commit อีกต่อไป

