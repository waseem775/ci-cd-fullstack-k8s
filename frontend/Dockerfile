# 📦 Build stage
FROM node:18 as build

WORKDIR /app

# ✅ Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# ✅ Copy the rest of the React source code
COPY . .

# 🔧 Build the production React app
RUN npm run build

# 🚀 Production stage
FROM nginx:alpine

# ✅ Copy build output to NGINX’s default HTML directory
COPY --from=build /app/build /usr/share/nginx/html

# 🌐 Optional: custom nginx config for SPA routing (if you have it)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# 🛡️ Expose port
EXPOSE 80

# 🌀 Start NGINX
CMD ["nginx", "-g", "daemon off;"]
