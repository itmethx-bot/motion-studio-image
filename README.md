# Motion Studio image builder

ชุดสร้าง Docker image สำหรับเว็บ Motion Studio แบบอัปโหลดรูปภาพและวิดีโอสองไฟล์

## ไฟล์ที่เผยแพร่

ซอร์ส 47 ไฟล์อยู่ใน [motion-studio-source.zip](motion-studio-source.zip) พร้อมรายการ SHA-256 ของแต่ละไฟล์ ไม่มีรูป วิดีโอส่วนตัว API key หรือ model weights รวมอยู่ในชุดนี้

SHA-256 ของ ZIP: `a2ce0c07b64bfb727c7421fbffd5c896f95aade802930f6a97d35c3dd9a61942`

## สร้าง image

1. เปิด Actions → Build Motion Studio image → Run workflow
2. งานจะตรวจแฮช แตกซอร์ส ตรวจ dependency และสร้าง Docker image บน runner มาตรฐานของ GitHub
3. เมื่อสำเร็จ ชื่อ `ghcr.io/itmethx-bot/motion-studio-image@sha256:...` จะแสดงใน Job summary
4. ตั้ง Package settings → Change visibility → Public เพื่อให้ RunPod ดาวน์โหลดได้โดยไม่ต้องมี registry credentials
5. ใส่ชื่อ image พร้อม digest ใน Motion Studio ที่รันบน PC แล้วบันทึกการตั้งค่า

GitHub Actions ใช้ GITHUB_TOKEN ชั่วคราวสำหรับเผยแพร่ image ไม่ต้องใส่ RunPod API key บน GitHub งานสร้าง image นี้ไม่สร้าง Pod และไม่ใช้ GPU ของ RunPod

## ข้อจำกัดและสถานะการตรวจสอบ

ComfyUI ถูกตรึง commit `f1072eb0350638a3390ddb6afbcaa8c6b237c6fd` และ PyTorch 2.9.1 / CUDA 12.8 runtime ถูกตรึง digest ของ image ทางการ การตรวจ build บน CPU ไม่ได้ยืนยันคุณภาพวิดีโอ สิทธิ์ RunPod API หรือการ Terminate จริง ต้องทดสอบบน GPU แยกต่างหาก

ระบบตั้งใจสร้าง Pod ใหม่ต่อหนึ่งงานโดยไม่ใช้ Network Volume ดาวน์โหลดและตรวจ checksum ของโมเดลประมาณ 59 GB เมื่อเริ่ม Pod บันทึกวิดีโอและหลักฐานลง PC ก่อน Stop และ Terminate การดาวน์โหลดโมเดลซ้ำอาจใช้เวลาและมีค่าเช่า GPU ระหว่างเตรียมเครื่อง PC ต้องออนไลน์จนบันทึกผลและยืนยันลบ Pod สำเร็จ

ภาพส่วนของร่างกายที่ไม่ปรากฏในรูปอ้างอิงเป็นการประมาณของโมเดล ไม่รับประกันรูปลักษณ์จริง ความถูกต้องทางกายวิภาค หรือผลลัพธ์ที่เหมือนกันสำหรับทุกคลิป

## เอกสารอ้างอิง

- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [GitHub Actions billing](https://docs.github.com/en/billing/concepts/product-billing/github-actions)
- [RunPod storage](https://docs.runpod.io/pods/storage/types)

## Initial-scene prop filter revision

Only interaction objects with a valid mask in frame zero seed appearance references and layout. Later-only generic object candidates are excluded with an explicit warning; genuine later-entering objects are also unsupported. Source analysis summaries and the failure stage are retained on the PC before automatic Pod cleanup. This is a filtering policy, not semantic object recognition or verified physical contact.
