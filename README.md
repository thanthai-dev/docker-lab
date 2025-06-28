# Todo App - Docker Lab

## 📋 รายละเอียดโปรเจค

แอปพลิเคชัน Todo List ที่สร้างด้วย **Docker Compose** ประกอบด้วย 3 ส่วนหลัก:

* **Frontend**: React.js (Port 3000)
* **Backend**: Node.js + Express (Port 4000)
* **Database**: PostgreSQL (Port 5432)

## 🚀 วิธีการเริ่มต้น

### ข้อกำหนดเบื้องต้น

* ติดตั้ง Docker หรือ Podman หรือ Rancher ในเครื่อง
* Port 3000, 4000, 5432 ว่าง

### การติดตั้งและรัน

1. **Clone โปรเจค**

2. **แก้ไข dockerfile, docker-compose ให้สมบูรณ์**

3. **สร้างและรันคอนเทนเนอร์**

```bash
# สร้างและรันทันหมด
docker-compose up --build

# รันในพื้นหลัง
docker-compose up -d --build
```

4. **เปิดแอป**

   * Frontend: [http://localhost:3000](http://localhost:3000)
   * Backend API: [http://localhost:4000](http://localhost:4000)
   * Database: localhost:5432

## 📁 โครงสร้างและคำอธิบาย

### โครงสร้างไฟล์
```
docker-lab/
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

### Frontend (React)

* **Port**: 3000
* **Technology**: React 18
* **Features**: เพิ่ม/ลบ Todo, Real-time API calls
* **Environment**: `REACT_APP_API_URL` สำหรับเชื่อมต่อ Backend

### Backend (Node.js)

* **Port**: 4000
* **Technology**: Express.js + PostgreSQL
* **APIs**:

  * `GET /api/todos` - ดึงรายการ Todo ทั้งหมด
  * `POST /api/todos` - เพิ่ม Todo ใหม่
  * `DELETE /api/todos/:id` - ลบ Todo
  * `GET /health` - Health check
* **Database**: Auto-create table เมื่อ start server

### Database (PostgreSQL)

* **Port**: 5432
* **Database**: `tododb`
* **User**: `postgres`
* **Password**: `password`

## 🔧 การใช้ Environment Variables

### Environment Variables ของ Frontend

```env
REACT_APP_API_URL: http://localhost:4000
```

### Environment Variables ของ Backend

```env
PORT: 4000
DB_USER: postgres
DB_NAME: tododb
DB_PASSWORD: password
DB_HOST: database
DB_PORT: 5432
```

### Environment Variables ของ Database (PostgreSQL)
```env
POSTGRES_DB: tododb
POSTGRES_USER: postgres
POSTGRES_PASSWORD: password
```

## 🛠️ คำสั่งที่สำคัญ

```bash
# สร้างและรัน
docker-compose up --build

# รันในพื้นหลัง
docker-compose up -d --build

# ดู logs ทั้งหมด
docker-compose logs -f

# ดู logs เพียง service
docker-compose logs -f frontend

# หยุด และลบ container
docker-compose down

# และลบ volume
docker-compose down -v

# สร้างใหม่ใหม่ service
docker-compose up --build frontend

# เข้าไปใน container
docker exec -it todo-backend sh

docker exec -it todo-postgres psql -U postgres -d tododb
```

## 💠 การทดสอบ

### 1. ทดสอบ Frontend

* เปิด [http://localhost:3000](http://localhost:3000)
* ลองเพิ่ม Todo ใหม่
* ลองลบ Todo

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

# ลบ Todo (ID=1)
curl -X DELETE http://localhost:4000/api/todos/1
```

### 3. ทดสอบ Database

```bash
# เข้าไปใน PostgreSQL
psql -U postgres -d tododb

# ดู table
\dt

# ดูข้อมูล
SELECT * FROM todos;

# ออก psql
\q
```

## 🚹 การแก้ไขปัญหา

1. **Port ซ้ำกัน**

```bash
netstat -tulpn | grep :3000
netstat -tulpn | grep :4000
netstat -tulpn | grep :5432
```

2. **Database connection failed**

```bash
docker-compose logs database
docker-compose restart database
```

3. **Frontend ไม่เชื่อมต่อ Backend**

* ตรวจสอบ `REACT_APP_API_URL`

4. **Build failed**

```bash
docker system prune -a
docker-compose build --no-cache
```

## 📚 สิ่งที่เรียนรู้จากแลปนี้

### Docker Concepts

* Dockerfile
* docker-compose
* Environment Variables
* Volumes
* depends\_on

### Best Practices

* Alpine image
* .env file
* meaningful container names

## 📝 หมายเหตุ

* `docker-compose down -v` จะลบเดตาทั้งหมด
* Production ควรใช้ .env
* ควรเปลี่ยน password ก่อนใช้

---

**Happy Dockerizing! 🐳**
