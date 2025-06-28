# ใช้ Node.js 18 Alpine เป็น base image
FROM node:18-alpine

# ตั้ง working directory
WORKDIR /app

# Copy package.json และ package-lock.json (ถ้ามี)
COPY package*.json ./

# ติดตั้ง dependencies
RUN npm install

# Copy source code ทั้งหมด
COPY . .

# Expose port ที่ React app จะรัน
EXPOSE 3000

# รันคำสั่งเริ่มต้น
CMD ["npm", "start"]