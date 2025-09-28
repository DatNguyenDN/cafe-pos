# Sử dụng Node 18
FROM node:18

# Tạo thư mục làm việc
WORKDIR /app

# Copy package.json và package-lock.json
COPY package*.json ./

# Cài dependencies (chỉ production)
RUN npm install --only=production

# Copy toàn bộ source code
COPY . .

# Build project
RUN npm run build

# Expose port (giống main.ts)
EXPOSE 4000

# Start app
CMD ["npm", "run", "start:prod"]
