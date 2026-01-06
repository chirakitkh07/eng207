###  Lab 1: Analyze Real System Architecture

## นายจิรกิตติ์ คำป่าตัน 67543210014-6

## เลือกระบบ Grab Food / LINE MAN

## 1) System Overview
ระบบนี้ทำอะไร?

-GrabFood / LINE MAN คือแพลตฟอร์ม “Marketplace + Logistics” สำหรับสั่งอาหาร ที่ต้องประสานงาน 4 โลกพร้อมกัน:

- ลูกค้า: ค้นหาร้าน/เมนู → สั่ง → จ่ายเงิน → ติดตาม → รีวิว
- ร้านอาหาร: รับออเดอร์ → ยืนยัน/ปรุง → แจ้งพร้อมรับ → - - - จัดการเมนู/สต๊อก/โปรโมชั่น
- ไรเดอร์: รับงาน → รับอาหาร → ส่ง → อัปเดตสถานะ/ตำแหน่ง
- ระบบกลาง: จับคู่ไรเดอร์, คิดค่าส่ง, จัดการสถานะ,     แจ้งเตือน, ป้องกัน fraud, วิเคราะห์ข้อมูลแบบ real-time

## Stakeholders หลักคือใคร?
- Customer (ผู้สั่งอาหาร)
- Merchant/Restaurant (ร้านอาหาร)
- Rider/Driver (ไรเดอร์)
- Operations / Support (ทีมแก้ปัญหา ออเดอร์ล่ม/คืนเงิน/ข้อร้องเรียน)
- Platform/Engineering (ทีมดูแลระบบ, SRE)
- Payment partners / Banks / Payment gateway
- Regulators / PDPA & Compliance

## Quality Attributes สำคัญ
สำหรับ food delivery “ของจริง” มักเน้น:
- Availability (ล่มตอนเที่ยง = รายได้หาย)
- Performance/Latency (กดสั่ง/จ่าย/ติดตามต้องไว)
- Scalability (พีคเป็นช่วง ๆ + แคมเปญ)
- Reliability/Consistency (สถานะออเดอร์ต้อง “ไม่มั่ว”)
- Security & Privacy (ข้อมูลที่อยู่/การชำระเงิน)
- Observability (ต้องตามหาเหตุขัดข้องข้ามหลาย service ได้)

## 2) Architecture Analysis
ใช้ Architectural Style อะไร?

## Style A: Microservices Architecture

- แยกโดเมนหลักเป็นบริการย่อย เช่น User/Auth, Catalog, Order, Payment, Delivery/Dispatch, Notification, Review ฯลฯ

- ช่วยให้ “แต่ละส่วน scale และ deploy แยก” (เหมาะกับระบบโตเร็ว/ทีมใหญ่)

- Grab มีสื่อสารเรื่องการจัดการ microservices และระบบ platform/mesh ที่รองรับ microservices จำนวนมาก 
Grab Tech
+1

- LINE MAN Wongnai ก็มีเนื้อหาเชิง microservices และ tooling สนับสนุนการ monitor ในหลายบริการ 
life.wongnai.com
+1

## Style B: Event-Driven Architecture (EDA)

แทนที่จะให้ทุกอย่างเรียกกันแบบ synchronous อย่างเดียว จะมี “เหตุการณ์ (events)” เช่น OrderCreated, PaymentConfirmed, FoodPrepared, RiderAssigned, Delivered

ใช้ message/stream platform อย่าง Kafka เพื่อสื่อสารแบบหลวม (decouple) และรองรับงาน real-time

