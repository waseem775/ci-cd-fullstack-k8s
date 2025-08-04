# 📦 Build stage
FROM node:18 as build

WORKDIR /app

# ✅ Copy only package files first to leverage Docker cache
COPY package*.json ./
RUN npm install

# ✅ Copy the rest of the app
COPY . .

# 🔧 Build production-ready React app
RUN npm run build

# 🚀 Production stage
FROM nginx:alpine

# ✅ Copy build output to NGINX's default HTML directory
COPY --from=build /app/build /usr/share/nginx/html

# ✅ Optional: Custom NGINX config for SPA routing
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

# 🌀 Start NGINX
CMD ["nginx", "-g", "daemon off;"]
