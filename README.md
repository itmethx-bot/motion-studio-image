# Motion Studio image builder

ชุดสร้าง Docker image สำหรับ Motion Studio แบบอัปโหลดภาพและวิดีโอสองไฟล์
โค้ดและภาพอิมเมจเท่านั้น ไม่มีรูป คลิปส่วนตัว API key หรือ model weights ใน repository

เปิด repository แบบ Public เพื่อใช้ standard GitHub-hosted runner โดยไม่มีค่าเวลาสร้าง
Workflow ทำงานเมื่อกด Actions → Build Motion Studio image → Run workflow เท่านั้น
ไม่สร้างหรือเช่า RunPod และไม่ใช้ RunPod API key ใน GitHub
GitHub ให้ GITHUB_TOKEN ชั่วคราวกับ workflow เพื่อเผยแพร่ image ไป GHCR โดยอัตโนมัติ

เมื่อ build สำเร็จ ดูชื่อ ghcr.io/บัญชี/repository@sha256:... ใน Job summary
GitHub อาจตั้ง package ใหม่เป็น Private ต้องเปลี่ยน Package settings → Change visibility → Public
ก่อนให้ RunPod ดึง image โดยไม่มี registry credentials
จากนั้นใช้ชื่อพร้อม digest ใน Template ของ RunPod ที่ Motion Studio แสดง

ComfyUI ถูกตรึงไว้ที่ f1072eb0350638a3390ddb6afbcaa8c6b237c6fd
PyTorch 2.9.1 / CUDA 12.8 runtime image ถูกตรึง digest จาก registry ทางการ
Native node source check ผ่าน แต่ยังไม่ได้ทดสอบ GPU กับรุ่นนี้
แยกคนหลักเป็นภาพรูปลักษณ์บนพื้นกลาง และใช้ฉากว่างกับผังตำแหน่งแทนรูปเต็มเพื่อลดคนซ้ำ
ส่วนที่ภาพอ้างอิงไม่เห็นใช้โครงร่างคนหลักจากคลิปเป็นต้นแบบการสร้างใหม่ สูงสุด 3 เฟรม
ไม่ตัดแปะอวัยวะต้นฉบับ ไม่แยกอวัยวะรายส่วน และยังไม่ยืนยันสีผิว สัดส่วนหรือคุณภาพวิดีโอ
ผู้ใช้เช่าและเชื่อม Pod เอง สร้างต่อเนื่องบนเครื่องเดิม และ Stop / Terminate เอง
ไม่มี provider API, account API key หรือคำสั่งหยุดและลบเครื่องในระบบที่เผยแพร่
โมเดลประมาณ 59GB จะดาวน์โหลดและตรวจ checksum เมื่อเปิด Pod ใหม่
PC ต้องออนไลน์จนบันทึกวิดีโอและหลักฐานครบ แล้วผู้ใช้ตรวจไฟล์ก่อนลบ Pod

References:
- https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry
- https://docs.github.com/en/billing/concepts/product-billing/github-actions
