# Todo App - Docker Lab

## 📋 รายละเอียดโปรเจค

แอปพลิเคชัน Todo List ที่สร้างด้วย **Docker Compose** ประกอบด้วย 3 ส่วนหลัก:

- **Frontend**: React.js (Port 3000)
- **Backend**: Node.js + Express (Port 4000)
- **Database**: PostgreSQL (Port 5432)

## 🚀 วิธีการเริ่มต้น

### ข้อกำหนดเบื้องต้น
- Docker และ Docker Compose ติดตั้งในเครื่อง
- Port 3000, 4000, 5432 ว่าง

### การติดตั้งและรัน

1. **Clone หรือสร้างโฟลเดอร์โปรเจค**
```bash
mkdir todo-app
cd todo-app
```

2. **สร้างโครงสร้างไฟล์ตามที่กำหนด**
```
todo-app/
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   ├── public/
│   │   └── index.html
│   └── src/
│       ├── App.js
│       ├── index.js
│       └── App.css
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── docker-compose.yml
└── README.md
```

3. **สร้างและรันคอนเทนเนอร์**
```bash
# สร้างและรันทั้งหมด
docker-compose up --build

# หรือรันในพื้นหลัง
docker-compose up -d --build
```

4. **เปิดแอป**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:4000
   - Database: localhost:5432

## 📁 โครงสร้างและคำอธิบาย

### Frontend (React)
- **Port**: 3000
- **Technology**: React 18
- **Features**: เพิ่ม/ลบ Todo, Real-time API calls
- **Environment**: `REACT_APP_API_URL` สำหรับเชื่อมต่อ Backend

### Backend (Node.js)
- **Port**: 4000
- **Technology**: Express.js + PostgreSQL
- **APIs**:
  - `GET /api/todos` - ดึงรายการ Todo ทั้งหมด
  - `POST /api/todos` - เพิ่ม Todo ใหม่
  - `DELETE /api/todos/:id` - ลบ Todo
  - `GET /health` - Health check
- **Database**: Auto-create table เมื่อ start server

### Database (PostgreSQL)
- **Port**: 5432
- **Database**: `tododb`
- **User**: `postgres`
- **Password**: `password`
- **Volume**: `postgres_data` สำหรับ persistent storage

## 🔧 คำสั่งที่สำคัญ

```bash
# สร้างและรัน
docker-compose up --build

# รันในพื้นหลัง
docker-compose up -d --build

# ดู logs ทั้งหมด
docker-compose logs -f

# ดู logs เฉพาะ service
docker-compose logs -f frontend
docker-compose logs -f backend
docker-compose logs -f database

# หยุดและลบคอนเทนเนอร์
docker-compose down

# หยุดและลบทั้งข้อมูล
docker-compose down -v

# สร้างใหม่เฉพาะ service
docker-compose up --build frontend

# เข้าไปใน container
docker exec -it todo-backend sh
docker exec -it todo-postgres psql -U postgres -d tododb
```

## 🛠️ การทดสอบ

### 1. ทดสอบ Frontend
- เปิด http://localhost:3000
- ลองเพิ่ม Todo ใหม่
- ลองลบ Todo

### 2. ทดสอบ Backend API
```bash
# Health check
curl http://localhost:4000/health

# ดึงรายการ Todo
curl http://localhost:4000/api/todos

# เพิ่ม Todo ใหม่
curl -X POST http://localhost:4000/api/todos \
  -H "Content-Type: application/json" \
  -d '{"text":"ทดสอบ API"}'

# ลบ Todo (แทนที่ 1 ด้วย ID ที่ต้องการ)
curl -X DELETE http://localhost:4000/api/todos/1
```

### 3. ทดสอบ Database
```bash
# เข้าไปใน PostgreSQL
docker exec -it todo-postgres psql -U postgres -d tododb

# ดู table
\dt

# ดูข้อมูลใน table
SELECT * FROM todos;

# ออกจาก psql
\q
```

## 🐛 การแก้ไขปัญหา

### ปัญหาที่พบบ่อย

1. **Port ซ้ำกัน**
```bash
# ตรวจสอบ port ที่ใช้งาน
netstat -tulpn | grep :3000
netstat -tulpn | grep :4000
netstat -tulpn | grep :5432

# หยุด process ที่ใช้ port
sudo kill -9 <PID>
```

2. **Database connection failed**
```bash
# ตรวจสอบ database logs
docker-compose logs database

# Restart database
docker-compose restart database
```

3. **Frontend ไม่เชื่อมต่อ Backend**
- ตรวจสอบ `REACT_APP_API_URL` ใน docker-compose.yml
- ตรวจสอบว่า Backend running บน port 4000

4. **Build failed**
```bash
# ลบ images และ containers เก่า
docker system prune -a

# Build ใหม่
docker-compose build --no-cache
```

## 📚 สิ่งที่เรียนรู้จากแลปนี้

### Docker Concepts
- **Dockerfile**: สร้าง custom images
- **docker-compose**: จัดการหลาย containers
- **Environment Variables**: การส่งค่า config
- **Volumes**: การเก็บข้อมูลถาวร
- **Networks**: การเชื่อมต่อระหว่าง containers
- **depends_on**: การกำหนดลำดับการรัน

### Best Practices
- ใช้ Alpine images เพื่อขนาดเล็ก
- ใช้ multi-stage builds สำหรับ production
- แยก environment variables
- ใช้ health checks
- ตั้งชื่อ container ที่มีความหมาย

## 🚀 ความท้าทายเพิ่มเติม

1. **เพิ่ม Nginx Reverse Proxy**
2. **เพิ่ม Redis สำหรับ Caching**
3. **เพิ่ม Health Checks**
4. **ใช้ Docker Secrets สำหรับ passwords**
5. **สร้าง Production build ด้วย multi-stage**
6. **เพิ่ม Monitoring ด้วย Prometheus**

## 📝 หมายเหตุ

- ข้อมูลใน Database จะหายเมื่อรัน `docker-compose down -v`
- สำหรับ Production ควรใช้ environment variables จากไฟล์ `.env`
- ควรเปลี่ยน default passwords ก่อนใช้จริง

---
**Happy Dockerizing! 🐳**