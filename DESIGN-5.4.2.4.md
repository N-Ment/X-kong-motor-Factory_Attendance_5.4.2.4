# NOVA UI 5.4.2.4

## Design tokens

ไฟล์ `public/css/release.css` เป็นชุดสีและรูปแบบรุ่นใหม่ โหลดหลัง `style.css` เพื่อคงส่วนกล้องและการทำงานเดิม

| Token | Light | Dark |
|---|---|---|
| พื้นหลัง | #F7F7F4 | #0C0C0C |
| การ์ด | #FFFFFF | #171717 |
| ตัวอักษร | #171C1B | #F5F5F5 |
| ตัวอักษรรอง | #4D5C58 | #BDBDBD |
| Accent | #64C9CF | #EEEEEE |
| เส้นขอบ | #D7DFDC | #454545 |
| โพสต์อิท | ฟ้าอ่อน / เหลือง / เหลืองส้ม / ส้ม | เทา 4 ระดับ |

## Navigation

- Owner/Admin: ภาพรวม → โพสต์อิท → งานแต่ละหมวด
- พนักงาน: หน้าหลัก → สแกน / สรุป OT / ประวัติ / คำขอลา / ปฏิทิน / บัญชี
- Desktop ตั้งแต่ 1000px: เมนูหลักด้านซ้าย
- Mobile: เมนูหลักล่าง 4 รายการ พร้อม safe area และพื้นที่ท้ายหน้าเพื่อเลื่อนพ้นปุ่มได้
- ทุกหมวดตั้งค่าเปิดเฉพาะเนื้อหาหมวดนั้น และหยุดกล้องลงทะเบียนเมื่อเปลี่ยนหมวด

## Forms and data

- ปุ่มหลักสูงอย่างน้อย 44px, ปุ่มรองขนาดเล็กอย่างน้อย 40px
- ช่องค้นหา/ตัวกรองมี label แสดงตลอด; กดค้นหาหรือ Enter เพื่อใช้ค่า ไม่มีการล้างแบบฟอร์มระหว่างพิมพ์
- ตาราง desktop มีหัวตารางและคอลัมน์ระบุตัวตนค้างอยู่
- Mobile แสดงประวัติลงเวลาและ OT รายเดือนเป็นการ์ด ลดการเลื่อนแนวนอน
- สถานะมีข้อความประกอบ; โหมดมืดไม่ใช้สีแยกความหมาย
- checkbox มีชื่อพนักงาน/วันที่สำหรับเครื่องมืออ่านหน้าจอ และปุ่มเลือกทั้งหมดแสดงสถานะเลือกบางส่วน
- รายการจำนวนมากแบ่งหน้า; ตัวเลือกทุกคนหมายถึงเฉพาะหน้าปัจจุบัน
- รองรับ focus จากคีย์บอร์ด และ prefers-reduced-motion

## Server behavior

- `reviewAttendanceBatch`: ids 1–100, deduplicate, lock ต่อ batch, ตรวจ role ผ่าน API, ป้องกันอนุมัติเวลาตนเอง, ตรวจเวลาออกและสถานะใหม่, เก็บ AuditLog ต่อแถว
- `employeeSummary`: คงข้อมูลสรุปเดิม และเพิ่ม monthly, approvedOt, pendingOt; คำนวณทุกเดือนในช่วง ไม่เกิน 366 วัน
- `reviewLeave`: อนุญาต manager/owner พิจารณาคำขอลาตนเองตามสิทธิ์ Admin ที่ผู้ใช้กำหนด
- `setupAll`: สร้าง OWNER ก่อนส่ง PIN ชั่วคราวให้การสร้าง Developer ครั้งแรก; credentials เดิมไม่ถูกแทนที่

## Files changed

- `public/js/app.js`: navigation, filtering/pagination, row selection, monthly summaries, state cleanup
- `public/css/release.css`: design tokens, components, responsive layouts
- `public/index.html`: stylesheet และ theme color
- `apps-script/Attendance.gs`: batch approval
- `apps-script/Code.gs`: API route และ monthly summary
- `apps-script/Leave.gs`: Admin self leave
- `apps-script/Setup.gs`: shared initial credential
- `tests/backend-smoke.cjs`, `tests/ui-smoke.cjs`, `tests/responsive-browser.cjs`: regression checks

UI automation ใช้ API จำลอง; การจับคู่ใบหน้า การอ่าน GPS และการเขียนชีตจริงยังต้องตรวจบนระบบติดตั้งจริง
